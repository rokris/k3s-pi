# K3s-pi

Configure Raspberry Pi MicroSD
In order to perform this operation, you will need the following equipments:

* A computer with ssh enabled
* A Raspberry Pi 3 or 4
* A MircroSD card (8GB min)
* A Wi-Fi connection

First, you need to install the OS on your Raspberry Pi.

On your computer, insert the MicroSD card, to install Raspbian OS on it.

You need to download Raspberry Pi Imager 
Download Raspberry PI OS Lite 64 (Bullseye).img
Release 04.04.2022 (For PI 3/4/400)

Launch Raspberry Pi Imager, then click on “Select OS” and select Raspberry Pi OS (64-bit) you just downloaded.

Press Cmd+Shift+X (on Mac) or Ctrl+Shift+X (on Windows) to get to the Advanced options window. 
Enable SSH and WiFi.

SSH into Raspberry Pi

Update and Upgrade the Raspberry PI OS.

`$ sudo apt update && sudo apt full-upgrade -y`<br>
`$ sudo reboot`

Optional: Install extra helpfull tools and settings.

`$ sudo apt install avahi-daemon git -y`<br>
`$ nano ~/.bashrc`

> Add -> alias ll='ls -alF'

Disable swap file usage by changing the parameter in file.

`$ sudo nano /etc/dphys-swapfile`

> CONF_SWAPSIZE=0

Add following to the end of the line:

`$ sudo nano /boot/cmdline.txt`

> cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1

Turn off Autologin / Console

`$ sudo raspi-config`

> Select "System option"<br>
> Select "Boot Auto Login"<br>
> Select "Console"<br>
> Reboot (Yes)

---
## Prep for K3s:

`$ sudo iptables -F`<br>
`$ sudo update-alternatives --set iptables /usr/sbin/iptables-legacy`<br>
`$ sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy`<br>
`$ sudo reboot`

---

## Install K3S master node:
`$ curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644`

And that’s it! You have a Kubernetes cluster running! You can check it with the command:

`$ kubectl get all -A`<br>
`$ kubectl get pods -A`

When all is created and look good...

`$ sudo reboot`

---

## Install K3S worker nodes:

To install on worker nodes and add them to the cluster, run the installation script with the K3S_URL and K3S_TOKEN environment variables. Here is an example showing how to join a worker node:

To get the token value use this command on the master:

`$ sudo cat /var/lib/rancher/k3s/server/token`

On worker, run command to install K3s

`$ curl -sfL https://get.k3s.io | K3S_URL=https://<server>:6443 K3S_TOKEN=<token> sh -`
  
K3s config information for Remote clients is stored at master node.

`$ sudo cat /etc/racher/k3s/k3s.yaml`

---

## To uninstall K3s

On K3s master node run:

`$ /usr/local/bin/k3s-uninstall.sh`
