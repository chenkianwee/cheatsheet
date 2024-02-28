# Projects with Micro-Controller
## Connecting ESP32 to PC
- https://www.bordergate.co.uk/configuring-an-esp32-in-ubuntu-22-04/

1. check to see if your esp32 is connected to your pc. Key in lsusb and you should be able to see the bus information listed below
    ```
    lsusb 
    ```
    ```
    Bus 00x Device 00x: ID xxx:yyyy Silicon Labs CP210x UART Bridge
    ```

2. check for the tty. You shld see this at the very end.
    ```
    sudo dmesg
    ```
    ```
    [ xxxx.yyyy] usb 3-7: New USB device strings: Mfr=1, Product=2, SerialNumber=3
    [ xxxx.yyyy] usb 3-7: Product: CP2104 USB to UART Bridge Controller
    [ xxxx.yyyy] usb 3-7: Manufacturer: Silicon Labs
    [ xxxx.yyyy] usb 3-7: SerialNumber: xxxxx
    [ xxxx.yyyy] usbcore: registered new interface driver usbserial_generic
    [ xxxx.yyyy] usbserial: USB Serial support registered for generic
    [ xxxx.yyyy] usbcore: registered new interface driver cp210x
    [ xxxx.yyyy] usbserial: USB Serial support registered for cp210x
    [ xxxx.yyyy] cp210x 3-7:1.0: cp210x converter detected
    [ xxxx.yyyy] usb 3-7: cp210x converter now attached to ttyUSB0
    ```
## Wifi Extender Project
- https://github.com/martin-ger/esp32_nat_router
1. Download the files from the above website
2. Flash them into the controller.
    ```
    esptool.py --chip esp32 \
    --before default_reset --after hard_reset write_flash \
    -z --flash_mode dio --flash_freq 40m --flash_size detect \
    0x1000 build/esp32/bootloader.bin \
    0x8000 build/esp32/partitions.bin \
    0x10000 build/esp32/firmware.bin
    ```
3. Go to 192.168.4.1 to access the configuration. Follow the instruction on the github website above for configuring your new wifi. 