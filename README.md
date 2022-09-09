# k3s-pi

Configure Raspberry Pi MicroSD
In order to perform this operation, you will need the following equipments:

A computer with ssh enabled
A Raspberry Pi 3 or 4
A MircroSD card (8GB min)
A Wi-Fi connection

First, you need to install the OS on your Raspberry Pi. 

On your computer, insert the MicroSD card, to install Raspbian OS on it.
You need first to download Raspberry Pi Imager 
Download Raspberry PI OS Lite 64 (Bullseye).img
Release 04.04.2022 (For PI 3/4/400)

Launch Raspberry Pi Imager, then click on “Select OS” and select Raspberry Pi OS (64-bit) you just downloaded.

Press Cmd+Shift+X (on Mac) or Ctrl+Shift+X (on Windows) to get to the Advanced options window. 
Enable SSH and WiFi.

SSH into Raspberry Pi

- sudo apt update && sudo apt upgrade -y
- sudo reboot
- nano ~/.bashrc
- alias ll='ls -alF'
- sudo apt install avahi-daemon git -y
- sudo nano /boot/cmdline.txt
In file /boot/cmdline.txt add cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1 into the end of the file.
Turn off Autologin / Console
- sudo raspi-config
- Select "System option"
- Select "Boot Auto Login"
- Select "Console"
- Reboot

Then install K3S:
- curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644

- sudo reboot

And that’s it! You have a Kubernetes cluster running! You can check it with the command:
- kubectl get pods -A


To install on worker nodes and add them to the cluster, run the installation script with the K3S_URL and K3S_TOKEN environment variables. Here is an example showing how to join a worker node:

To get the token value use this command:
sudo cat /var/lib/rancher/k3s/server/token

curl -sfL https://get.k3s.io | K3S_URL=https://<server>:6443 K3S_TOKEN=<token> sh -
  
K3s config information for Remote clients is stored at master node.
- sudo cat /etc/racher/k3s/k3s.yaml

