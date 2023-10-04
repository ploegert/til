# Install Jupyter in WSL

First Make sure that Windows Subsystem for LInux is installed.

First thing is to make sure you have updates & python Installed

```bash
sudo apt update && upgrade
sudo apt install python3 python3-pip ipython3
sudo apt install jupyter-core

#Check your version
python3 --version

#Upgrade pip
python3 -m pip install --upgrade pip
```

You can run Jupyter Notebook in your WSL. Here WSL will act as a jupyter server accessible at localhost with port 8888. The steps to install Jupyter is as following-

1. Install Jupyter by typing the following command in your Bash Shell.

```
pip3 install jupyter
```

2\. Create alias to launch jupyter without browser from the WSL:

* Open your bash configuration and add the following line

```bash
vi ~/.bashrc
```

* add the following line "\~/.bashrc", saving the fil

```bash
#Add the following line after the case statement
alias jupyter-notebook="~/.local/bin/jupyter-notebook --no-browser"
```

![](<../../.gitbook/assets/image (18) (1).png>)

* After that you have to source your file by updating your Bash profile by entering:

```
source ~/.bashrc
```

Now whenever you will type Jupyter Notebook in your bash you will be directed to the local host. so type:

```
jupyter notebook
```



**Reference/Orig Source Credit goes to**: [https://medium.com/@sayanghosh\_49221/jupyter-notebook-in-windows-subsystem-for-linux-wsl-f075f7ec8691](https://medium.com/@sayanghosh\_49221/jupyter-notebook-in-windows-subsystem-for-linux-wsl-f075f7ec8691)

