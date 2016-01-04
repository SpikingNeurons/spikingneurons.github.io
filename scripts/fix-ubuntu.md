# Fix Ubuntu

## Get internet and github
+ Shortcuts
```sh
gedit ~/.bashrc
# add these lines
# ...........................................
alias sss='sudo service lightdm start'
alias bbb='gedit ~/.bashrc'
alias uuu='. ~/.bashrc'
# ...........................................
. ~/.bashrc
```
+ wlan -- getting internet
```sh
iwconfig wlan0 essid name key password
```
+ github
```sh
# Set git to use the credential memory cache
git config --global credential.helper cache
# Set the cache to timeout after 1 hour (setting is in seconds)
git config --global credential.helper 'cache --timeout=3600'
#
git config --global push.default simple
# Get back the saving scripts
cd ~
git clone https://github.com/praveenneuron/private-files.git
```

## Making the base ready

#### Some tricks
+ Hold shift during boot to get in recovery mode and edit files

#### Purge
```sh
sudo apt-get --purge remove <package_name>
```

#### More configs if issue
```sh
sudo nano /etc/init/gpu-manager.conf #comment everything in this file
OR/AND
sudo nano /etc/modprobe.d/blacklist.conf # add line ... blacklist nouveau
```

#### Force text mode for Ubuntu
```sh
sudo nano /etc/default/grub
Replace
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
with
GRUB_CMDLINE_LINUX_DEFAULT="text"
sudo update-grub
```

#### Basic softs
```sh
sudo apt-get install build-essential
sudo apt-get install cmake
sudo apt-get install cmake-qt-gui
sudo apt-get install g++
sudo apt-get install screen-resolution-extra
sudo apt-get install linux-headers-generic
sudo apt-get install linux-headers-$(uname -r)
sudo apt-get install git
sudo apt-get install libglu1-mesa-dev
sudo apt-get install libglu1-dev
sudo apt-get install libxi-dev
sudo apt-get install libxmu-dev
sudo apt-get install mesa-utils
sudo apt-get install freeglut3-dev
sudo apt-get autoremove
sudo apt-get update
```

## Installing Drivers and CUDA

#### Download deb file for CUDA 7.5
```sh
# checksums
cd ~/Downloads
# (md5sum: e1f61e1e62fb354ab7a662f82b1f4b0d)
md5sum cuda-repo-ubuntu1404_7.5-18_amd64.deb 
# install
sudo dpkg -i cuda-repo-ubuntu1404_7.5-18_amd64.deb
sudo apt-get update
sudo apt-get install cuda
```

#### Update environment
```sh
gedit ~/.bashrc
# ............add these lines to .bashrc...........................
export PATH=$PATH:/usr/local/cuda-7.5/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-7.5/lib64
export CUDA_SAMPLES=~/NVIDIA_CUDA-7.5_Samples/bin/x86_64/linux/release/
# ...................... Also add to global ldconfig ....
sudo /bin/bash -c 'echo "/usr/local/cuda-7.5/lib64" > /etc/ld.so.conf.d/nvidia.conf'
sudo gedit /etc/ld.so.conf.d/nvidia.conf # add /usr/local/cuda-7.0/lib64 to file
sudo ldconfig
# check Drivers
cat /proc/driver/nvidia/version
```

#### Install samples
```sh
#post
 cuda-install-samples-7.5.sh ~
 sudo apt-get install selinux-utils
 setenforce 0
# compile samples
cd ~/NVIDIA_CUDA-7.5_Samples/
make -j8
make -j8 -k # if you do not care for some samples (or install drive from run file)
cd $CUDA_SAMPLES
./deviceQuery
./bandwidthTest
./fluidsGL
```

## Something more if not working
```text


# some more softs
sudo apt-get install nvidia-visual-profiler
sudo apt-get install nvidia-nsight

## 4
sudo apt-get install nvidia-prime
sudo prime-select nvidia
sudo gedit /etc/X11/xorg.conf #add line so that we have
  Section "Screen"
   Identifier "nvidia"
   Device "nvidia"
   Option "UseDisplayDevice" "None"
  EndSection
```
