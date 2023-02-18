# Django
## Django Commands
django interactive shell
  ```
  py manage.py shell
  ```
## Deploying Django on Microsoft Azure
The instructions are based on these sources
- https://realpython.com/django-nginx-gunicorn/

1. ssh into your vm with powershell from windows.
    ```
    ssh directory\to\your\key.pem your:ip:add
    ```
2. Copying files into the VM
    ```
    scp -i your\directory\key.pem your\dir\file.zip username@you:ip:address:/home
    ```
3. cd into your Django project directory. Run your Django app.  
    ```
    python3.11 manage.py runserver
    ```
    - check if it is running with the GET command. You will need to install httpie.
        ```
        GET http://127.0.0.1:8000/
        ```
4. Get a DNS name from (https://ddns.freedombox.org/). Using the service associate your IP address to the domain name.
5. Setting up Gunicorn. Create the log and run file as specified in your config dev.py file.
    ```
    $ sudo mkdir -pv /var/{log,run}/gunicorn/
    $ sudo chown -cR ubuntu:ubuntu /var/{log,run}/gunicorn/
    ```
6. Run Gunicorn with this command.
    ```
    gunicorn -c config/gunicorn/dev.py
    ```
7. Check on the status of gunicorn with this command.
    ```
    tail -f /var/log/gunicorn/dev.log
    ```
### Nginx subdirectories routing
- https://www.digitalocean.com/community/questions/nginx-subdirectory-returning-404
- https://stackoverflow.com/questions/29587738/how-to-configure-nginx-to-pass-proxy-to-tomcat-on-centos

### Potential Issues
1. Changing write permission on windows power shell (https://stackoverflow.com/questions/48888365/openssh-using-private-key-on-windows-unprotected-private-key-file-error?rq=1)
    ```
    icacls .\private.key /inheritance:r
    icacls .\private.key /grant:r yourusername:"(R)"
    ```
2. Install Python3.11 on ubuntu (https://www.debugpoint.com/install-python-3-11-ubuntu/)
    ```
    sudo add-apt-repository ppa:deadsnakes/ppa
    ```
    ```
    sudo apt update 
    ```
    ```
    sudo apt install python3.11
    ```
3. Also install the venv on the ubuntu (https://stackoverflow.com/questions/69594088/error-when-creating-venv-error-command-im-ensurepip-upgrade-def)
    ```
    sudo apt install python3.11-venv
    ```
