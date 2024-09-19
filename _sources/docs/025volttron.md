# Volttron
- https://volttron.readthedocs.io/en/main/tutorials/quick-start.html

## Installation 
- Platform installation: https://www.youtube.com/watch?v=UrRQ5LNF2xU&list=TLGGlihJ3pKI1vgxODA5MjAyNA

1. Install all the dependencies. Volttron currenly only works with python3.10
    ```
    sudo apt-get install build-essential python3-dev python3-venv openssl libssl-dev libevent-dev git

    ```
2. git clone the volttron repo
    ```
    git clone https://github.com/VOLTTRON/volttron
    ```
3. setup the env with the following command
    ```
    python3 bootstrap.py
    ```
## Useful tutorials
- the driver framework: 
    - video: https://www.youtube.com/watch?v=bPE_-6nHuSY
    - docs: 
        - driver framework: https://volttron.readthedocs.io/en/develop/agent-framework/driver-framework/platform-driver/platform-driver.html#platform-driver
        - https://volttron.readthedocs.io/en/develop/developing-volttron/developing-drivers/driver-development.html

- bacnet driver: 
    - video: https://www.youtube.com/watch?v=qQNL2n936AU

## BACnet scan
1. Startup a volttron instance. cd to the volttron directory, by default volltron will create a .volttron folder in you home directory to store all your config files for this instance. You can change the folder by specifying it with the following:
    ```
    export VOLTTRON_HOME=~/.volttron1
    ```

2. execute the following to spin up volttron. 
    ```
    ./start-volttron  
    ```

3. install the listener agent with this command.
    ```
    vctl install examples/ListenerAgent/ --tag listener
    ```

4. install the platform driver 
    ```
    vctl install services/core/PlatformDriverAgent
    ```

5. store all your config files in configs folder
    ```
    mkdir configs
    ```

6. copy the bacpypes.ini file ot the config folder
    ```
    cp scripts/bacnet/BACpypes.ini configs
    ```

7. change the BACpypes.ini address to your machine ip address. Make sure you are connected to the BACnet network. The rest of the parameters does not matter.
    ```
    [BACpypes]
    objectName: something
    address: 192.168.1.1/24
    objectIdentifier: 10
    maxApduLengthAccepted: 1024
    segmentationSupported: segmentedBoth
    vendorIdentifier: 15
    ```

8. scan for bacnet points.
    ```
    python scripts/bacnet/bacnet_scan.py --csv-out configs/bacnet_scan.csv --ini configs/BACpypes.ini
    ```

9. Once you have the bacnet_scan.csv file generate the driver config file
    ```
    python scripts/bacnet/grab_multiple_configs.py configs/bacnet_scan.csv --out-directory configs/ --ini configs/BACpypes.ini
    ```

10. Now we need to setup a bacnet proxy to talk to the bacnet devices. copy the config file to the config folder
    ```
    cp services/core/BACnetProxy/config configs/bacnet_proxy.config
    ```

11. make sure to change the device_address field to the right ip address of your device
    ```
    {
    "device_address": "192.168.1.1/24",
    "max_apdu_length": 1024,
    "object_id": 599,
    "object_name": "Volttron BACnet driver",
    "vendor_id": 5,
    "segmentation_supported": "segmentedBoth"
    }
    ```

12. Install the bacnet proxy driver 
    ```
    vctl install services/core/BACnetProxy/ -c configs/bacnet_proxy.config --tag bacnet_proxy
    ```

13. setup the device driver. Here I am setting up the driver for device 8.
    ```
    vctl config store platform.driver devices/8 configs/devices/8
    ```

14. setup the registry config
    ```
    vctl config store platform.driver registry_configs/8.csv configs/registry_configs/8.csv --csv
    ```

15. you can check the platform.driver config using this 
    ```
    vctl config list platform.driver
    ```

16. start the platform driver 
    ```
    vctl start --tag driver
    ```

17. You can look at the log with this command
    ```
    tail -f volttron.log
    ```

## Configuring Volttron Central
- https://volttron.readthedocs.io/en/develop/deploying-volttron/multi-platform/volttron-central-deployment.html#volttron-central-deployment
1. 