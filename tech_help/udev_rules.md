# Adding a udev rule for the Open Manipulator X
Here is a short guide to fix the incrementing USB port issue some have been experiencing. Please do not hesitate to reach out with questions!

## Steps to resolve the issue:
### 1. get the serial number of your team's arm
Run the command `lsusb` to print information about all of the USB devices you have connected to your machine. The 3001 arm should register as `Future Technology Devices International, Ltd FT232H Single HS USB-UART/FIFO IC`. After confirming that the robot is conencted, run the verbose version of `lsusb` which is `lsusb -v`, find the device representing the 3001 arm again and record the serial number.

Example: in this case, my serial number is AAAAAA.
```
Bus 001 Device 020: ID 0403:6014 Future Technology Devices International, Ltd FT232H Single HS USB-UART/FIFO IC
Couldn't open device, some information will be missing
Device Descriptor:
  bLength                18
  bDescriptorType         1
  bcdUSB               2.00
  bDeviceClass            0 [unknown]
  bDeviceSubClass         0 [unknown]
  bDeviceProtocol         0 
  bMaxPacketSize0        64
  idVendor           0x0403 Future Technology Devices International, Ltd
  idProduct          0x6014 FT232H Single HS USB-UART/FIFO IC
  bcdDevice            9.00
  iManufacturer           1 FTDI
  iProduct                2 USB <-> Serial Converter
  iSerial                 3 AAAAAA
  bNumConfigurations      1
  Configuration Descriptor:
    bLength                 9
    bDescriptorType         2
    wTotalLength       0x0020
    bNumInterfaces          1
    bConfigurationValue     1
```

### 2. cd into the correct directory and create a .rules file
First, cd into your udev rules directory by running the command `cd /etc/udev/rules.d`. Then make a .rules file that will set the udev rule for the OM X arm by running the command `sudo touch 99-rbe-serial.rules`.

### 3. Add the udev rule 

Copy and paste the text below into the file you created in the previous step. be sure to replace the `ATTRS{serial}` value with your actual arm serial number. 

```                                                             
# RBE 3001 Open Manipulator-X udev rule
SUBSYSTEM=="tty", ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6014", ATTRS{serial}=="om_x_serial_number", SYMLINK+="rbearm", MODE="0666"
```

### 4. Reload udev rules
After adding and saving the new rule, reload and initialize all udev rules by running these two commands:
```
sudo udevadm control --reload-rules
sudo udevadm trigger
```

### 5. Editing the `OM_X_arm.m` class
Due to the port naming change, the OM_X_arm class will also have to be slightly edited. Open the file `OM_X_arm.m` in MATLAB and navigate to the comment `% Find serial port and connect to it` that should be on line 40. Comment out everyhting in the "try" section of the try/except statement and add this line instead: `self.deviceName = '/dev/rbearm'; % this is the linked USB port.`

For reference, this is what the entire block should look like:
```
 % Find serial port and connect to it
            try
                self.deviceName = '/dev/ft232h'; % this is the symlinked USB port.
            catch exception
                error("Failed to connect via serial, no devices found.")
            end
            self.port_num = portHandler(self.deviceName);
```

Your USB connection issues should now be resolved!

</br>
---
</br>
Last edited by: K. Herbstzuber - A25