# reComputer R1000 Getting Started
- Overview walkthrough: https://wiki.seeedstudio.com/recomputer_r1000_intro/
- Getting started guide: https://wiki.seeedstudio.com/recomputer_r1000_flash_OS/#for-linux
- install specific python version on your RPI: https://raspberrytips.com/install-latest-python-raspberry-pi/
    - volttron currently still only works on python3.10

- the drivers do not work with ubuntu 22
- however volttron only works on ubuntu and not RPI OS

## Arduino Opta
- https://opta.findernet.com/en/tutorial/getting-started
- https://forum.arduino.cc/t/opta-upload-failed-no-dfu-capable-usb-device-available/1149001/6
- opta modbus tcp: https://docs.arduino.cc/tutorials/opta/getting-started-with-modbus-tcp/
- opta modbus rs485: https://docs.arduino.cc/tutorials/opta/getting-started-with-modbus-rtu/
- modbus example: https://docs.arduino.cc/tutorials/opta/smart-compressor-app-note/

### For Windows
1. Follow the instruction here https://forum.arduino.cc/t/opta-upload-failed-no-dfu-capable-usb-device-available/1149001/3

### For Ubuntu
1. Add a new file '60-dfu.rules' into '/etc/udev/rules.d/60-dfu.rules'. The '60-dfu.rules' file should contain the following:
```
ACTION=="add", SUBSYSTEM=="usb", ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0364", TAG+="uaccess"
```

2. You can check for the idvendor and idproduct by plugging in your opta typing the following in the cmd.  
```
sudo dmesg -w
```
- Then double click the 'RESET' button on the opta board and you will see the information

3. Restart arduino ide and follow the getting started tutorial. You should be able to upload the data onto the controller.

