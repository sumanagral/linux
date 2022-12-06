# CMPE283-Assignment-2
- For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched. (You may skip this question if you are doing the lab by yourself):
We decided to work together throghout the implementation of project in GCP.

- Ninad Marathe: Built the kernel, comprehended the necessary stages, cpuid instruction and cpu leaf node research, forking the torvalds/linux repository, understanding where to make the necessary changes in files, creation of the vmx.c file, creation of the test file, and updating of the documentation.

- Suma Nagral: Built the kernel, VM installation of necessary kernel modules, knowledge on atomic variables compiled the test file, created the documentation, and added the discussed modifications to the cpuid.c file.

- Our task is to change the CPUID emulation code in KVM such that it returns more information when a particular CPUID "leaf function" is invoked.
- For CPUID leaf node %eax=0x4FFFFFFC:
- We have to return total number of exits (all catagories) in %eax
- For CPUID leaf node %eax=0x4FFFFFFD:
We have to return the top 32 bits of the total time spent processing all exits in%ebx.
The low 32 bits of the total time spent processing all exits in%ecx will be returned. * Return values for %ebx and %ecx are measured in processor cycles across all VCPUs. At a high level, you will need to:
- Modify the kernel code with the assignment's functionality:
Decide where to put the measurement code.
Create new CPUID leaf 0x4FFFFFFD, 0x4FFFFFFC
- Make a user-mode application that executes the necessary CPUID instructions needed to test.
This is possible on Ubuntu by installing the 'cpuid' package.
Run this user mode program in the nested VM
- Check for correct output.

# Steps to replicate
We followed the steps to install the VM on our Windows OS. 
1) Installed ISO - Ubuntu 18.5, allocated disk space of 200 GB, Memory of 8GB,  CPU 8 cores  
2) We have cloned the Linux github repository, using following command; git clone https://github.com/ninadsmarathe/linux
3) Install gcc make
4) Installed the dependency; sudo apt-get install libncurses-dev gawk flex bison openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf 
5) Create cmpe283-1.c 
using touch cmpe283-1.c command 
Modify the content as per as the requirement.
uname -r
sudo apt install linux-headers-$(uname -r)
cd Linux
cp /boot/config-5.10.0-19-cloud-amd64 .config
6) Check the Linux Version uname -a
7) sudo make oldconfig
8) cd linux
9) Following instruction in "Linux" folder: make -j 8 modules && make -j 8 && sudo make modules_install && sudo make install. sudo reboot
uname -a
9) We will make changes to cpuid.c at ~/linux/arch/X86/kvm

# Test your code
- Install cpudid package on your nested virtual machine
sudo apt-get update
sudo apt-get install cpuid

- Test leaf node 0x4FFFFFFC
Run the following command inside your nested virtual machine
cpuid -1 -l 0x4ffffffc

Update your kvm module
Follow the guidelines as described in the assignment and make changes to the linux/arch/x86/kvm/cpuid.c and linux/arch/x86/kvm/vmx/vmx.c files.

After modification, run the below commands to build your kernel source tree for the changes to take effect.

make -j 8 modules
make -j 8
sudo make INSTALL_MOD_STRIP=1 modules_install

- Build your kvm module
Check if kvm module is already loaded

lsmod | grep kvm

- Remove kvm if already loaded

sudo rmmod kvm_intel sudo rmmod kvm

- Load updated kvm modules

sudo modprobe kvm sudo modprobe kvm_intel

# Test your code
Install cpudid package on your nested virtual machine
sudo apt-get update

sudo apt-get install cpuid

# Test leaf node 0x4FFFFFFC
Run the following command inside your nested virtual machine

cpuid -1 -l 0x4ffffffc


