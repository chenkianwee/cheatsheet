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
## Understanding the Driver Framework
- in order for a driver to communicate with devices. Each device needs to be defined by a device config file (JSON file) and a register_config file (csv or JSON file).
- device configuration is a JSON file that requires these 3 fields "driver_config", "driver_type", "register_config"
    - driver_config: tells the driver how to access the device that is going to be read e.g. BACnet : ip address, device id, CSV: the csv file path etc.
    - driver_type: specifies the python file that is the driver
    - registry_config: a path specifying the **register_config** file that explains metadata from the device.

- in the <your-driver-name>.py file you have two classes, Interface and Register.
- the interface class you have 4 methods that is compulsory:
    - configure - in the configure method, you read the device configuration json file "driver config" field to find the device to read. You also read the "register_config" file and pass all the necessary information to the Register class with self.insert_register.
        - once the register object is populated you can find the object and use the properties to access the device. e.g. register.point_name etc.
    - get_point - using the "self.get_register_by_name(point_name)" method get the point name of interest from the device
    - _set_point - using the "self.get_register_by_name(point_name)" method get the point name of interest from the device and set a new value
    - _scrape_all - using the self.get_registers_by_type("byte") and get all the points on the device

### csvdriver example
- developing a csvdriver eg: https://volttron.readthedocs.io/en/develop/developing-volttron/developing-drivers/driver-development.html


1. Read through the example above. You can find a complete interface file in the example folder. Put the csvdriver.py in volttron -> services -> core -> PlatformDriverAgent -> platform_driver -> interfaces

2. Startup a volttron instance. cd to the volttron directory, by default volltron will create a .volttron folder in you home directory to store all your config files for this instance. You can change the folder by specifying it with the following:
    ```
    export VOLTTRON_HOME=~/.volttron1
    ```

3. execute the following to spin up volttron. 
    ```
    ./start-volttron  
    ```

4. install the listener agent with this command.
    ```
    vctl install examples/ListenerAgent/ --tag listener
    ```

5. install the platform driver 
    ```
    vctl install services/core/PlatformDriverAgent --tag driver
    ```

6. store all your config files in configs folder
    ```
    mkdir configs
    ``` 

7. setup the device driver.
    ```
    vctl config store platform.driver devices/csv_driver configs/csv_driver.config
    ```

8. setup the registry config
    ```
    vctl config store platform.driver registry_configs/csv_registers.csv configs/csv_registers.csv --csv
    ```

9. you can check the platform.driver config using this 
    ```
    vctl config list platform.driver
    ```

10. start the platform driver 
    ```
    vctl start --tag driver
    ```

11. You can look at the log with this command
    ```
    tail -f volttron.log
    ```

### csv agent example
- https://volttron.readthedocs.io/en/develop/developing-volttron/developing-agents/agent-development.html

1. activate the volttron environment
2. start the agent creation wizard
    ```
    vpkg init csvdriver_agent csvdriver_agent
    ```
3. Make the necessary changes to the agent.py file and config file. Take a look at the an example from volttron/examples/CSVDriver/CsvDriverAgent/CsvDriverAgent/agent.py and volttron/examples/CSVDriver/CsvDriverAgent/config

4. Once you are done we can install the agent
    ```
    python -m scripts.install-agent -s csvdriver_agent/ -c csvdriver_agent/config -t csvagent
    ```
    ```
    vctl install csvdriver_agent/ -c csvdriver_agent/config --tag csvagent
    ```

5. start the agent with 
    ```
    vctl start --tag csvagent
    ```
    
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
    vctl install services/core/PlatformDriverAgent --tag driver
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

## Resources
- the driver framework: 
    - video: https://www.youtube.com/watch?v=bPE_-6nHuSY
    - docs: 
        - driver framework: https://volttron.readthedocs.io/en/develop/agent-framework/driver-framework/platform-driver/platform-driver.html#platform-driver
        s
- bacnet driver: 
    - video: https://www.youtube.com/watch?v=qQNL2n936AU

- modbus driver:
    - video: https://volttron.org/videos/office-hours-%E2%80%93-august-16-2019
    - docs: https://volttron.readthedocs.io/en/develop/agent-framework/driver-framework/modbus/modbus-tk-driver.html?highlight=modbus

- Historian: https://volttron.readthedocs.io/en/develop/agent-framework/historian-agents/historian-framework.html#historian-video-tutorial

- Developing an agent: https://volttron.readthedocs.io/en/develop/developing-volttron/developing-agents/agent-development.html

- single machine deployment: https://volttron.readthedocs.io/en/develop/deploying-volttron/single-machine.html