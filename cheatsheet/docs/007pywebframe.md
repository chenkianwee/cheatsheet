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

- pyscript ltk is a wrap of jquery and jqueryUI, although jqueryUI is in maintenance mode, meaning there wont be any new features. The library is very stable for the existing basic UI features like sliders etc.

- in the widget.py file defines all the UI element. Users declare the UI elements using the widgets.py classes. The widgets.py module then calls jquery.py to translate all the python to html and css using the jquery library.

can you explain the project structure of ltk, from the github repository I see the following files

- __init__.py
- jquery.py
- logger.py
- ltk.css
- ltk.js
- pubsub.py
- widgets.py

This is an excellent question because looking at a library's file structure reveals its architectural intent. Since LTK operates as a hybrid library—using Python for logic but relying on web technologies for presentation—its files reflect this dual nature.

Here is a breakdown of the project structure, grouped by their function within the library:

🏗️ Core API & Structure

These files define what the user interacts with and how the package operates as a whole.

- __init__.py: This is the entry point for the Python package. It serves two primary purposes:
It defines the public interface of LTK (i.e., when a user writes from ltk import Button, this file makes that class available).
It initializes and imports other core components, ensuring the library is set up correctly when it's first loaded by PyScript.
🧩 UI Components & Logic

These files contain the actual building blocks of your application.

- widgets.py: This file holds the definitions for all the visual elements (the "Widgets"). It contains Python classes like Button, Input, Label, etc. These classes define what a widget is and its properties, but they do not handle drawing themselves; they merely hold the state and logic for that component.
- pubsub.py: This stands for Publish-Subscribe​. It is crucial for implementing LTK's reactive programming model. Instead of having widgets directly call functions on each other (which creates tight coupling), PubSub allows a widget to "publish" an event (e.g., "The button was clicked") and other interested components can "subscribe" to that event, reacting automatically when it occurs. This is how LTK manages state changes efficiently.

🌉 Infrastructure & Integration

These files are the bridges between pure Python logic and the messy world of the web browser.

- jquery.py: Since LTK leverages jQuery for DOM manipulation (the process of changing HTML elements), this file acts as a Python wrapper or interface layer​. It allows your high-level Python code to call low-level functions that ultimately translate into JavaScript/jQuery commands, enabling the widget to actually render itself on the page.
- logger.py: This is a standard utility module for logging messages. It helps developers track what the library is doing during runtime—such as reporting errors, debugging state transitions, or providing warnings about improper usage.

🎨 Assets (Presentation Layer)

These files are not Python code; they are static web assets that handle how LTK looks and functions in the browser environment.

- ltk.css: This file contains all the Cascading Style Sheets​. It dictates the appearance of your widgets—the colors, fonts, layout, padding, etc.—ensuring that a Button looks like a button.
- ltk.js: While LTK uses jQuery for core DOM operations, this file likely holds any specific JavaScript initialization scripts or helper functions required by the library to integrate smoothly with PyScript and the browser environment itself.

💡 Architectural Summary

In short, LTK is designed as a State Management System that relies on external tools for rendering:

You define your UI logic in Python (widgets.py).
The state changes are managed through decoupled events using the PubSub pattern (pubsub.py).
When it's time to show the widget, LTK uses its internal bridge (jquery.py) to tell JavaScript what HTML needs to be generated.
CSS and JS files handle the actual visual presentation in the browser.

in the jquery.py file, in line 44 jQuery = window.jQuery can you explain what is happening here

This line of code is a classic pattern used when integrating JavaScript libraries like jQuery into a system that bridges Python and the web (like PyScript).

Here is a detailed explanation of what is happening in jQuery = window.jQuery:

1. The Role of window

In any standard web browser environment, the global object accessible from all scripts running on the page is the window object. It represents the current browser window and its environment.

2. Accessing the Global Library (window.jQuery)

When a webpage loads jQuery (usually via a <script> tag), that library is made available to all scripts running on that page by attaching it as a property to the global window object. Therefore, window.jQuery retrieves the actual, loaded jQuery function/object from the browser's environment.

3. Assignment (jQuery = ...)

The code then takes that retrieved global object and assigns it to a local variable named jQuery.

Why is this assignment necessary?

Bridging Python and JavaScript: The core of LTK is written in Python, but the rendering engine (the part that actually changes the HTML) is powered by JavaScript/jQuery. This line serves as the bridge​. It takes the raw, browser-specific jQuery object and makes it accessible to the Python code running within PyScript.
Simplified Calling: Instead of having to write verbose calls like window.jQuery(...) every time you want to use a jQuery function in your Python logic, you can now simply call jQuery(...). This makes the library much cleaner and more idiomatic for developers working in a Python environment.
In Summary:

The line jQuery = window.jQuery is essentially saying:

"Go look in the browser's global scope (window) for the jQuery library that has been loaded, grab it, and give me a clean variable named jQuery so my Python code can easily use it to manipulate the web page."


## pyscript with threejs
- https://threejsdemos.com/demos/basics/points-cloud
- https://threejsdemos.com/tutorials/3d-point-cloud
- https://github.com/Python-Ninja-Hebi/pyscript-threejs?tab=readme-ov-file

## Tutorial
- https://docs.pyscript.net/2025.7.3/beginning-pyscript/
- uploading files - https://pyscript.recipes/latest/basic/file-upload/

## Examples
- python editor in the browser: https://docs.pyscript.net/2026.7.2/user-guide/editor/
- good pyscript examples: https://jeff.glass/post/7-guis-pyscript/#timer-header

# Pelican
- https://docs.getpelican.com/en/latest/quickstart.html

1. Kickstart your website with the following command
    ```
    pelican-quickstart
    ```

2. Add some content https://docs.getpelican.com/en/latest/content.html. Put a file in your 'content' folder
    ```
    Title: My super title
    Date: 2010-12-03 10:20
    Modified: 2010-12-05 19:30
    Category: Python
    Tags: pelican, publishing
    Slug: my-super-post
    Authors: Alexis Metaireau, Conan Doyle
    Summary: Short version for index and feeds

    This is the content of my super blog post.
    ```

3. Generate the content for your static pages. The -t command let you specify the theme. If the theme is installed you can just specify the name, you can also specify the directory of themes you downloaded in your computer.
    ```
    pelican content -t notmyidea
    ```

4. Preview your site with the following command
    ```
    pelican -l
    ```

## Installing a Pelican theme
- https://docs.getpelican.com/en/latest/pelican-themes.html

1. list themes installed with the following command
    ```
    pelican-themes -v -l
    ```

2. Install a pelican theme with the following command
    ```
    pelican-themes --install ~/Dev/Python/pelican-themes/notmyidea-cms --verbose
    ```


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