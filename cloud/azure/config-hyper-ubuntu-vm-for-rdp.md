# Config hyper ubuntu vm for rdp

The following should get you up and running so you can rpd into your local hyperv vm

```bash
sudo apt-get install unzip
sudo apt install -y linux-tools-virtual
sudo apt install -y linux-cloud-tools-virtual
cd Downloads/
wget https://raw.githubusercontent.com/Hinara/linux-vm-tools/ubuntu20-04/ubuntu/20.04/install.sh
sudo chmod +x install.sh
sudo ./install.sh
init 6
cd Downloads/
sudo ./install.sh 
sudo apt update
cd Downloads/
sudo apt install ./microsoft-edge-beta_97.0.1072.21-1_amd64.deb 
sudo apt-get install libatomic1

```
