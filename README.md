A simple way to set a local DNS server for you local network, to bypass restriction and disable all DNS injection from your internet provider DNS. As well as configure your netwoork local domain resolution.

# To use: 
1. prepare the microSD card
   Simply flash the raspbian:buster image on a memorystick, *a /boot partion is visible even on windows after the image flashing*.
   * add a content less file on the */boot* partition with name *ssh*
   * add on the first line of the */boot/cmdline.txt* the following words: ip=<your desired IP>'
     E.g.:
     ```bash
     dwc_otg.lpm_enable=0 console=serial0,115200 console=tty1 root=PARTUUID=eaca1473-02 rootfstype=ext4 elevator=deadline fsck.repair=yes rootwait ip=192.168.1.254
     ```
2. insert the microSD card intot the raspberry pi, then plug it (network and power alike)
3. on your ansible controller (another linux host):
   * Install git and ansible
     E.g. for ubuntu or debian:
     ```bash
     sudo apt-get update && sudo apt-get upgrade -y
     sudo apt-get install ansible sshpass git -y 
     sudo pip install pyOpenSSL
     ```
   * pull this git repo
     ```bash
     git clone --depth 1 https://github.com/frederic-blanc/rpi-dns.git rpi-dns
     ```
   * edit the inventory file rpi-dns/ansible/inventory.ini
     with you choosen dns  server ip and futur hostname
   * edit the deployment configuration to match your at rpi-dns/ansible/group_vars/all/main.yml
     do not forget to define the work_users element
   * launch the playbook
     ```bash
     cd rpi-dns/ansible
     ansible-playbook -vv prepare-dns-server.yml  -i inventory.ini --extra-vars 'ansible_user=pi ansible_ssh_pass=raspberry ansible_sudo_pass=raspberry'
     ansible-playbook -vv install-dns-server.yml  -i inventory.ini --extra-vars 'ansible_user=<WORK_USER> ansible_ssh_pass=<WORK_USER_PASSWORD> ansible_sudo_pass=<WORK_USER_PASSWORD>'
     ```
4. configure your computers to use your new DNS.
   
