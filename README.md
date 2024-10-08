# DurinDoor is an embedded linux image with Shinobi pre-installed. 
## About
### Almost feature complete, boot time is roughly 25 seconds from power-on to login prompt. 

Rename incoming: DurinDoor. 

DurinDoor aims to create quick-booting, high uptime security camera enabled linux images for embedded devices. This project focuses on security and stability, while leveraging embedded linux's specific benefits like quick boot times, low memory footprint, low storage footprint, and low idle power usage. Initial building done on amd64 and arm64, testing done against a Raspberry Pi Zero W. 

It also tries to stay loosely within the core direction outlined in IronOxidizer/instant-pi:
- Buildroot for system builder
- musl for c libraries
- busybox / busybox init
- f2fs on rootfs

### STAGES:

- Docker Boot: ✓
- Buildroot Make: ✓
   - Automate buildroot make: x
- Image gen: ✓
   - Image Compression: x
   - Image encryption: x
- Image Boot: ✓
    - libcamera/rpi camera support: ✓ 
    - Networking ✓
        - DHCP: ✓
        - DNS: ✓
        - SFTP/SSH: ✓
        - HTTP/HTTPS: ✓
        - Wifi: ✓
             - Assoc: ✓
             - Auth: ✓
             - AP mode: x (Untested)
        - Iface modules load: ✓
        - Ifaces show in ifconfig/iwconfig: ✓
    - Encryption (ssh/certs): ✓
    - Shinobi: x
    - Automation: x
    - Hibernation: x

# Install

1. Download this repository and enter it.

    - If you **do not have Docker** installed run `sh INSTALL/docker.sh`.

3. Review and modify the `docker-compose.yml` file. 

    - Leave it as-is for default setup.

4. Run the preparation and starter script.
    ```
    bash setup_and_run.sh
    ```

# Building DurinDoor from source.

As it is right now, it's a pretty manual process. However, after completing the previous step:

   1. In a terminal window, do the following command to log into the new container
      ```
      docker exec {running-container-name} bash
      ```
   2. Ensure your current directory is /home/buildroot

   3. Buildroot very much dislikes running as root, so anything we do needs to respect the user "node" (Default user for this docker image). Run the following to set buildroot up to compile.
      ```
      su -c 'make linux_durindoor_rpi0w_defconfig' node
        ```
   4. Before building, examine the build and let buildroot fix any dependancy issues by running
      ```
      su -c 'make menuconfig' node
      ```
      
   5. That part is quick. The next part is not. Put on a kettle, get your favorite book ready, have a coffee pot on standby.
      ```
      su -c 'make' node
      ```

   6. This should spit out an image at /home/buildroot/output/image/sdcard.img. Plug that into your balena etcher, raspberry pi imager, `dd if= of=` if you're old school.

A folder was included for handing off files with the host (/home/buildrootFiles on the container, ~/buildrootFiles on the host). It's best to copy any files you need for installation or examination there before wiping the container for another build. 


#### Credits

This project uses code from the following sources: 

https://github.com/IronOxidizer/instant-pi/ (Various buildroot and boot config files used as skeletons)

https://gitlab.com/Shinobi-Systems/ShinobiDocker 

https://gitlab.com/buildroot.org/buildroot 

https://gitlab.com/Shinobi-Systems/Shinobi 

