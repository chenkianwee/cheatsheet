# BACpypes
- installation: https://bacpypes3.readthedocs.io/en/latest/gettingstarted/index.html

1. pip install bacpypes3

2. git clone the repo
    ```
    git clone https://github.com/JoelBender/BACpypes3.git
    ```

3. go to the bacpypes directory
    ```
    cd bacpypes
    ```

4. execute the sample scripts in samples. Script to discover devices.
    ```
    python -m samples.discover-devices 0 10 --address 192.168.1.1/24
    ```
    - to look for the address you can use ip addr show and look for inet based on your internet connection.

5. script to extract information from specific device. In this case device 8
    ```
    python -m samples.discover-objects 8 --address 192.168.1.1/24
    ```