# Steps for installing conda on WSL2
These steps only matter if the device being used does not have conda installed. The reason we're using conda is so we can use older versions of the packages we need to get this code working (hopefully).

Refer to this link for original instructions: https://docs.anaconda.com/free/anaconda/install/linux/

## Prereqs
Install the GUI dependencies specified in the link. Use the Debian option since Ubuntu is based on Debian.

## Installation
1. Run the Linux x86 command that has the shell script referenced to get the installer. The installer will go in whatever directory is open, so keep that in mind.
2. Check the SHA256 key of the installer is correct (to verify that the installer is correct). Refer to this website (https://docs.anaconda.com/free/anaconda/reference/hashes/) and the corresponding SHA256 hash as the target hash for whatever installer was used. Run `shasum -a 256 /PATH/FILENAME` to generate the hash for the downloaded installer (and ideally, those two hashes will patch)
3. Now, run the installer by running the bash file itself `bash PATH/Anaconda3-2020.05-Linux-x86_64.sh`, where `PATH` is the path to the actual installer itself (the linux bash file mentioned will depend on the one downloaded in step 1)
4. Review the license agreement and keep going through the installation process; nothing too difficult
5. They ask where to install Anaconda. Just do the default location they suggest so that you don't need to finagle around with it. Make sure the path selected isn't `/usr` though.
6. Should be almost done. Close and reopen the Ubuntu instance to have the changes reflected. If it works, then there should be `(base)` next to each line of an Ubuntu terminal, indicating that conda was activated and the base environment is selected

## Installing older version of Python to a new environment
The wiki here (https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-python.html) details it quite well.
1. First we need to find available versions of python that can be used. In Ubuntu with conda active (assuming the `(base)` is present), run `conda search python`. This will list out all packages with names that contain the text `python`. Look for whateevr version you're after. For this repository, we want python version 3.7.6.
2. Now we make our new environment with the correct Python and Pytorch packages. In the same terminal, run `conda create -n srp2023 python==3.7.6`
Here, `srp2023` is the name of the new environment. Just replace that with whatever you want the environment to be called.
3. Activate the new environment by running `conda activate srp2023`, again replacing `srp2023` with whatever the environment is called. The text on the left should now show `(srp2023)` to indicate that environment is active.
4. To check that the python version used is correct, run `python --version` to confirm that it is correct.

## Installing correct version of pytorch and relevant libraries
1. With the correct environment active, run `pip install torch==1.5.0+cu101 torchvision==0.6.0+cu101 -f https://download.pytorch.org/whl/torch_stable.html`. The older versions of torchvision aren't available on conda, so we have to use pip to install instead.

## Installing correct version of Apex
Seems like apex-0.1 is a library from Nvidia for acceleration purposes that is used in EBLNet. Don't install this by doing `conda install apex` or `pip install apex` since it'll refer to a completely irrelevant library or one that is too recent. Do the following instead (based on a comment here: https://github.com/MILVLG/bottom-up-attention.pytorch/issues/98#issuecomment-1344032695)
1. (Optional if you already installed a newer version of apex). Run `pip uninstall apex`, then `rm -rf apex` to get rid of it completely. 
2. Close the correct repository  here: `git clone https://github.com/ptrblck/apex.git`
3. Go into the apex repository via `cd apex`
4. Checkout a specific branch: `git checkout apex_no_distributed`
5. Run `pip install -v --no-cache-dir ./` and that should install the older version of apex correctly. 


# NOTE: This isn't a complete guide. I have left out many libraries that need to be installed, but they can be installed reasonably easily like skimage and tqdm.