# singularity sandbox
~~~~
Cross-platform Singularity 

Host:Windows 10 / OpenSUSE / Centos   
Vagrant VM guest : Centos / Ubuntu   

Download Vagrant https://www.vagrantup.com/downloads.html

Download Oracle VM Virtualbox https://www.virtualbox.org/wiki/Downloads

"Ansible_Local" Provisioning https://www.vagrantup.com/docs/provisioning/ansible_local.html

Singularity  https://sylabs.io/docs/
Examples https://github.com/sylabs/singularity/tree/master/examples
https://singularity-tutorial.github.io/

~~~~
~~~~
$ singularity --version
2.6.1-dist
$ singularity test
~~~~
~~~~
$ sudo singularity build lolcow.sif docker://godlovedc/lolcow

$ sudo singularity run docker://godlovedc/lolcow
Docker image path: index.docker.io/godlovedc/lolcow:latest
Cache folder set to /root/.singularity/docker
[6/6] |===================================| 100.0%
Creating container runtime...
Exploding layer: sha256:9fb6c798fa41e509b58bccc5c29654c3ff4648b608f5daa67c1aab6a7d02c118.tar.gz
Exploding layer: sha256:3b61febd4aefe982e0cb9c696d415137384d1a01052b50a85aae46439e15e49a.tar.gz
Exploding layer: sha256:9d99b9777eb02b8943c0e72d7a7baec5c782f8fd976825c9d3fb48b3101aacc2.tar.gz
Exploding layer: sha256:d010c8cf75d7eb5d2504d5ffa0d19696e8d745a457dd8d28ec6dd41d3763617e.tar.gz
Exploding layer: sha256:7fac07fb303e0589b9c23e6f49d5dc1ff9d6f3c8c88cabe768b430bdb47f03a9.tar.gz
Exploding layer: sha256:8e860504ff1ee5dc7953672d128ce1e4aa4d8e3716eb39fe710b849c64b20945.tar.gz
Exploding layer: sha256:736a219344fbca3099ce5bd1d2dbfea74b22b830bac0e85ecca812c2983390cd.tar.gz
 _________________________________________
/ Conscience doth make cowards of us all. \
|                                         |
\ -- Shakespeare                          /
 -----------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
~~~~
Create an Empty Container
~~~~
$ sudo singularity image.create -s 4096 centos7.img
Creating empty 4096MiB image file: centos7.img
Formatting image with ext3 file system
Image is done: centos7.img

$ sudo singularity image.create -s 4096 ubuntu.img
Creating empty 4096MiB image file: ubuntu.img
Formatting image with ext3 file system
Image is done: ubuntu.img
~~~~
Bootstrapping a Singularity Container
~~~~
$ sudo singularity bootstrap ./ubuntu-1.img /vagrant/definitions/ubuntu.def
$ sudo singularity inspect --deffile ubuntu-1.img
Bootstrap: docker
From: ubuntu:latest

%runscript
exec echo "The runscript is the containers default runtime command!"

%files
/home/vagrant/ubuntu.def /data/ubuntu.def
%environment
VARIABLE=HELLOWORLD
Export VARIABLE

%labels
AUTHOR githubfoam

%post
apt-get update && apt-get -y install python3 git wget
mkdir /data
echo "The post section is where you can install and configure your container."

~~~~

~~~~
$ hostnamectl
   Static hostname: control01
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 87877ae844c24636a27af6b5969497c9
           Boot ID: 5182c1c54a2a4004bba348c8809dc916
    Virtualization: oracle
  Operating System: Ubuntu 18.10
            Kernel: Linux 4.18.0-10-generic
      Architecture: x86-64

      $ cat Singularity.scientific
      BootStrap: yum
      OSVersion: 7
      MirrorURL: http://ftp.scientificlinux.org/linux/scientific/%{OSVERSION}x/$basearch/os/
      Include: yum


      %runscript
          echo "This is what happens when you run the container..."


      %post
          echo "Hello from inside the container"
          yum -y install vim-minimal

      $ sudo singularity build --sandbox centos7 Singularity.scientific
      Building into existing container: centos7
      Using container recipe deffile: Singularity.scientific
      tar: ./.singularity.d/env/95-apps.sh: file changed as we read it
      tar: ./.singularity.d/env: file changed as we read it
      tar: ./.singularity.d: file changed as we read it
      tar: .: file changed as we read it
      Sanitizing environment
      Adding base Singularity environment to container
      ERROR: Neither yum nor dnf in PATH!
      ABORT: Aborting with RETVAL=1
      Cleaning up...
~~~~
~~~~
$ hostnamectl
   Static hostname: control01
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 87877ae844c24636a27af6b5969497c9
           Boot ID: 5182c1c54a2a4004bba348c8809dc916
    Virtualization: oracle
  Operating System: Ubuntu 18.10
            Kernel: Linux 4.18.0-10-generic
      Architecture: x86-64

      $ cat Singularity.bionic
      BootStrap: debootstrap
      OSVersion: bionic
      MirrorURL: http://us.archive.ubuntu.com/ubuntu/


      %runscript
          echo "This is what happens when you run the container..."


      %post
          echo "Hello from inside the container"
          apt-get update
          apt-get clean

$ sudo singularity build --sandbox bionic.sandbox Singularity.bionic
$ ls
bionic.sandbox  Singularity.bionic
$ sudo singularity shell bionic.sandbox
Singularity: Invoking an interactive shell within container...

Singularity bionic.sandbox:~> ls /
bin  boot  dev  environment  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  singularity  srv  sys  tmp  usr  var

Singularity bionic.sandbox:~> cat /etc/os-release
NAME="Ubuntu"
VERSION="18.04 LTS (Bionic Beaver)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 18.04 LTS"
VERSION_ID="18.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=bionic
UBUNTU_CODENAME=bionic

Singularity bionic.sandbox:~> whoami
root
$ singularity shell bionic.sandbox
Singularity: Invoking an interactive shell within container...

Singularity bionic.sandbox:~/test1> whoami
vagrant
Singularity bionic.sandbox:~/test1> sudo apt-get update && sudo apt-get -y install
bash: sudo: command not found

$ sudo singularity shell --writable bionic.sandbox
Singularity: Invoking an interactive shell within container...

Singularity bionic.sandbox:~>
$ singularity shell --writable bionic.sandbox/
Singularity: Invoking an interactive shell within container...

Singularity bionic.sandbox:~/test1>
$ cat Singularity.bionic
BootStrap: debootstrap
OSVersion: bionic
MirrorURL: http://us.archive.ubuntu.com/ubuntu/


%runscript
    echo "This is what happens when you run the container..."


%post
        echo "Hello from inside the container"
        apt-get update
        # NEW UPDATES
        sed -i 's/$/ universe/' /etc/apt/sources.list
        apt-get update
        apt-get -y install vim fortune cowsay lolcat
        apt-get clean

~~~~
