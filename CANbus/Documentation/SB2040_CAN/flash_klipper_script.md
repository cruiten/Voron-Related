# Flash Klipper to the Octopus and the SB2040 in one step

This page documents how I implemented Eddie the Engineer's approach to flash Klipper to your primary MCU and your CAN toolhead MCU using a single shell script.

Eddie's does a great job describing the process, so I recommend that you first watch his video, which can be found [here](https://www.youtube.com/watch?v=1P4UrJxChL8).



This script makes it quite easy to flash Klipper to both MCUs in a matter of seconds...



The following steps are for a script to flash my Octopus 1.1 and my SB2040.

-----

[[Back to table of contents]](../README.md)

Please be aware that this script references the IDs of my Octopus and SB2040 MCUs...

- SSH into your RPi
- Execute the following command:

   ```sh
   cd ~/klipper
   ```

- Now use your favorite editor to create a new script file called `flashboards.sh` (I am using the `vi` editor)

   ```sh
   vi flashboards.sh
   ```

- I entered the following text:

  ```sh
  #!/bin/bash
  ### Thanks to Eddie the engineer for providing examples on how to flash Klipper on different devices!!!
  
  echo "Stopping Klipper..."
  sudo service klipper stop
  echo "Klipper has stopped"
  
  ### Flash the Octopus via USB...
  echo "Start processing for the Octopus..."
  make clean KCONFIG_CONFIG=config.octopus
  make menuconfig KCONFIG_CONFIG=config.octopus
  make -j4 KCONFIG_CONFIG=config.octopus
  make flash KCONFIG_CONFIG=config.octopus FLASH_DEVICE=/dev/serial/by-id/usb-Klipper_stm32f446xx_2F003D00075053424E363420-if00
  
  # Sometimes the Octopus is stuck in DFU mode...
  if [ $? -eq 0 ]; then
     echo "*** Successfully flashed Octopus"
  else
     echo "*** Unable to flash via serial path, attempting via USB device ID..."
     make flash KCONFIG_CONFIG=config.octopus FLASH_DEVICE=0483:df11
  fi
  
  echo
  echo ----------------------------------------------------------
  echo
  
  ### Flash the SB2040 via CAN Boot...
  echo "Start processing for the SB2040..."
  make clean KCONFIG_CONFIG=config.sb2040
  make menuconfig KCONFIG_CONFIG=config.sb2040
  make -j4 KCONFIG_CONFIG=config.sb2040
  python3 ~/CanBoot/scripts/flash_can.py -i can0 -f ~/klipper/out/klipper.bin -u d063055012c2
  
  echo "Starting Klipper..."
  sudo service klipper start
  echo "Klipper has started"
  ```

- Exit out of the editor, making sure to save your changes

- Execute the following command to allow execution of this file:

   ```sh
   sudo chmod +x flashboards.sh
   ```

- You invoke the script by typing the following command:

   ```sh
   ~/klipper/flashboards.sh
   ```





Once you have successfully completed the script, you can comment out the two lines that start with the following text: "`make menuconfig`".

(To comment a line, simply put a "#" character at the start of that line.)



If you comment out those two lines, you will not be prompted with the menus, and the Klipper images will be built based on the settings for each MCU that you saved when you did use the menus.

[[Back to table of contents]](../README.md)