# piVCCU

piVCCU is a project to install the original Homematic CCU2 firmware inside a virtualized container (lxc) on a Raspberry Pi running Raspbian Jessie or Stretch.

# Prequisites

* Raspberry Pi 2 or 3 (only 3 is tested right now)
* EQ3 HM-Mod-RPI-PCB
* Raspbian Jessie or Stretch

# Installation

1. Add the public key of the repository
   ```bash
   wget -q -O - http://alexreinert.github.io/piVCCU/public.key | sudo apt-key add -
   ```

2. Add the package repository
   ```bash
   sudo bash -c 'echo "deb http://alexreinert.github.io/piVCCU stable main" >> /etc/apt/sources.list'
   sudo apt update
   ```
   Instead of `stable` you can also use the `testing` tree, but be aware testing sometimes means not that stable.

3. Install custom kernel with the neccessary kernel modules for Homematic
   ```bash
   sudo apt install raspberrypi-kernel-homematic
   ```

4. Enable UART GPIO pins
   * Option 1: Raspberry Pi 2 *__(to be verified)__*
      ```bash
      sudo bash -c 'cat << EOT >> /boot/config.txt
      enable_uart=1
      EOT'
      ```
      
   * Option 2: Raspberry Pi 3 with disabled bluetooth (prefered)
      ```bash
      sudo bash -c 'cat << EOT >> /boot/config.txt
      dtoverlay=pi3_disable_bt
      EOT'
      sudo systemctl disable hciuart.service
      ```

   * Option 3: Raspberry Pi 3 with bluetooth wired to mini uart
      ```bash
      sudo bash -c 'cat << EOT >> /boot/config.txt
      dtoverlay=pi3_miniuart_bt
      enable_uart=1
      force_turbo=1
      EOT'
      ```

5. Disable serial console in command line
      ```bash
      sudo sed -i cmdline.txt -e "s/console=serial0,[0-9]\+ //"
      sudo sed -i cmdline.txt -e "s/console=ttyAMA0,[0-9]\+ //"
      ```

6. Add network bridge (if you are using wifi please refer to the debian documentation how to configure the network and the bridge)
   ```bash
   sudo apt-get purge dhcpcd5
   sudo bash -c 'cat << EOT > /etc/network/interfaces
   auto lo
   iface lo inet loopback
   
   iface eth0 inet manual
   
   auto br0
   iface br0 inet dhcp
     bridge_ports eth0
   EOT'
   ```

7. Reboot the system

8. Install CCU2 container
   ```bash
   sudo apt install pivccu
   ```

9. Start using your new virtualized CCU2, you can get the IP of the container using
   ```bash
   sudo pivccu-info
   ```

# Migration from other systems
1. Original CCU2 or RaspberryMatic

   Just restore a normal system backup *__(to be verified)__*

2. YAHM

   *__tbd__*

# Using CUxD and USB devices
*__tbd__*

# Build packages
If you like to build the .deb package by yourself
* Use Ubuntu 16.04 as build system
* Install prequisites *__tbd__*
* Clone source
* create_kernel.sh builds the custom kernel package
* create_pivccu.sh builds the container package
* Deploy the .deb files to an apt repository e.g. using reprepro

