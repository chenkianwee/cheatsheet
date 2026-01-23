# Python Web

# Pyscript

## Python in the browser
- https://jupyter.org/
- https://pyodide.org/en/stable/
- https://realpython.com/pyscript-python-in-browser/
- https://medium.com/andrewdass/how-to-start-using-pyscript-11036f998cef

## Developing webapp with pyscript
- to develop your webapp locally on your computer you need to create a simple server with python as follows
    - https://github.com/pyscript/pyscript/issues/257#issuecomment-1119595062

1. first go to the directory where your webapp is located
    ```
    cd <path_of_your_directory>
    ```
2. use the following command to serve the directory 
    ```
    python3 -m http.server
    ```
3. go to 'localhost:8000' or 'http://127.0.0.1:8000/' and see your webapp.

## pyscript using LTK
- example
    - pyalgoviz: https://github.com/laffra/pyalgoviz-pyscript/tree/main
## pyscript with threejs
- https://threejsdemos.com/demos/basics/points-cloud
- https://threejsdemos.com/tutorials/3d-point-cloud
- https://github.com/Python-Ninja-Hebi/pyscript-threejs?tab=readme-ov-file

## Tutorial
- https://docs.pyscript.net/2025.7.3/beginning-pyscript/
- uploading files - https://pyscript.recipes/latest/basic/file-upload/

## Examples
- good pyscript examples: https://jeff.glass/post/7-guis-pyscript/#timer-header


# Django
- good django tutorial: https://realpython.com/django-nginx-gunicorn/
- packaging django app (https://docs.djangoproject.com/en/4.2/intro/reusable-apps/)

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
- Configure your nginx to serve applications with subdomaims [here](https://stackoverflow.com/questions/9905378/nginx-subdomain-configuration)
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
## Django Resources
- Django Tutorial
    - https://www.w3schools.com/django/django_add_members.php 
    - Upload file
        - https://pythonguides.com/django-app-upload-files/
    - Download file
        - https://fedingo.com/how-to-download-file-in-django/
    - Deployment with Gunicorn and Nginx
        - https://realpython.com/django-nginx-gunicorn/
        - subdomain and multiple locations
            - https://stackoverflow.com/questions/9905378/nginx-subdomain-configuration
    - Django + CesiumJS
        - https://github.com/All4Gis/django-docker-cesium
    - Django Desktop App
        - https://github.com/ClimenteA/flaskwebgui
    - Django REST API
        - https://www.django-rest-framework.org/tutorial/quickstart/
    - Django REACT
        - https://blog.logrocket.com/using-react-django-create-app-tutorial/#how-send-data-django-react
    - Data from Django to Javascript
        - https://adamj.eu/tech/2022/10/06/how-to-safely-pass-data-to-javascript-in-a-django-template/