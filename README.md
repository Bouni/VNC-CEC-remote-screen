# VNC-CEC-remote-screen

Second iteration of my VNC + CEC remote screen using a Raspberry Pi 3B+

The following instructions assume your setup computer is a Linux machine. It's maybe possible to do all this on Windows, but I have no idea how :-)
I assume you have already installed [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) and [git](https://git-scm.com/) on your setup machine.

## Setup

1. Download the latest [Raspbian Stretch Lite](https://www.raspberrypi.org/downloads/raspbian/)
2. Unzip and write the image to your Micro SD card using [etcher](https://www.balena.io/etcher/) for example
3. Create an empty file on the boot Partition named `ssh` to automatically enable the SSH server
4. Plug the Micro SD card into your Pi, connect it to the Network and let it boot up
5. On the Screen you can see what IP your Pi got
6. Copy your ssh key onto the Pi using `ssh-copy-id pi@<ip>`
7. Log into your Pi using ssh pi@<ip>
8. Change your password using `passwd`
9. Clone the repo using `git clone https://github.com/Bouni/VNC-CEC-remote-screen`

## Configuration 

Change the hosts.yaml file according to your needs:

To setup multiple Pi's, just add an entry like follows to the hosts.yaml, make sure the hostname is unique.
```
 hosts:
    tv-umkleide: <-- This is the hostname the Pi will get
      ansible_ssh_host: 192.168.178.100 <-- This is the IP the Pi got from your DHCP server
    tv-fahrzeughalle:
      ansible_ssh_host: 192.168.178.101
    ...
```

Change the VNC Server settings to what is set in the settings of your VNC server
```
  vars:
    vnc_server: 192.168.178.200
    vnc_password: Password123
```

Change the HDMI settings if necessary, I recommend to do that after you installed everything and took a look at the results. If you need adjustment, just change the value here in the hosts.yaml and run ansible again.
```
  vars:
    ...
    config_disable_overscan: 1
    config_overscan_left: 24
    config_overscan_right: 24
    config_overscan_top: 24
    config_overscan_bottom: 24
    config_framebuffer_width: 1920
    config_framebuffer_height: 1080
    config_hdmi_force_hotplug: 1
    config_hdmi_mode: 16
    config_hdmi_drive: 2
```

After adjusting the host.yaml to your needs, run `ansible-playbook -v -i hosts.yaml playbook.yaml`

