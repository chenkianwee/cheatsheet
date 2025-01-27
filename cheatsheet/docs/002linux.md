# Linux commands

## Ubuntu
### User

sudo root
```
sudo su - root
```
### Installation
Update the OS
```
sudo apt-get update
```
```
sudo apt-get upgrade
```

Remove unnecessary files after an OS upgrade.
```
sudo apt-get autoremove
```

Install an app
```
sudo apt-get app_name
```

Check if program is installed
```
sudo apt list --installed | grep program_name
```
```
sudo apt list --installed program_name
```
```
sudo apt list --installed program_name | less
```
```
sudo apt list --installed program_name | more
```

Remove program
```
sudo apt-get remove --auto-remove program_name
```

Purge program remove all config files
```
sudo apt purge program_name
```

### File System
Check size of a directory
```
$ du -sh /path/to/folder
```
Check volumes in system
```
$ df -h
```
List a directory
```
$ ls
```
List with details, or show all hidden files
```
$ ls -lh file/foldername (will be able to see size in human readable way)

$ ls -l file/foldername

$ ls -la file/foldername
```
Copy file
```
$ cp /path/to/file1 /path/to/file2

$ cp -r /path/to/folder /path/to/folder
```
Remove a file.
```
$ rm filename
```
Create a file
```
# touch filename
```
Remove a folder.
```
$ rm -r foldername
$ rm -rf foldername #delete all folders in folders
```
Change permission
```
$ chmod g=rw filename
```
```
u=owner
g=group users
o=other users
a=all users

r=read
w=write
-=no permission
```
Change owner
```
sudo chown -cR owner:group folder path

sudo chown -c owner:group file path
```
Add directory
```
$ sudo mkdir
```
Copy files from local machine to a remote machine
```
$ scp C:\local\machine\file\path username@chaosbox.com:/your/path/on/remote/machine
```
Zip a file
```
zip archivename.zip filename1
```
Zip a directory
```
zip -r archivename.zip directory_name
```
Unzip a file
```
unzip filename
```
Search for a file or directory with a name
```
grep -r -i "name2search" /directory/to/search
```
### Terminal
Ctrl+K: Cut the part of the line after the cursor, adding it to the clipboard.

Ctrl+U: Cut the part of the line before the cursor, adding it to the clipboard.

Ctrl+Y: Paste the last thing you cut from the clipboard. The y here stands for “yank”

### Shellscript
Execute a shellscript
```
$ sudo sh shellscript_name
```

### Internet
Download a file from website with curl
```
$ curl -L https://github.com/chenkianwee/masa3db/archive/0.02.zip > masa3db-0.02.zip
```

connect to wifi using cmd
```
nmcli d wifi list

nmcli d wifi connect "test wifi network" password "secret"
```

### Read and Write Text
Read a text file
```
cat filename.txt
```
Create a text file
```
cat > filename.txt
```

### System
Check running service
```
$ systemctl status process_name
```
Stop running service
```
$ systemctl stop process_name
```
Start running service
```
$ systemctl start process_name
```
Check distribution
```
$ cat /etc/os-release
```

### Network
- https://itsfoss.com/check-ip-address-ubuntu/

Get ip
```
hostname -i

hostname
```
```
ip addr show
```

### Path
Check the path list
```
echo $PATH
```
Add path to the back of the path list
```
export PATH=$PATH:/home/your/path
```
Add path to the front of the path list
```
export PATH=/home/your/path:$PATH
```
To permanently add the path for yourself (local user), you need to put the command in the .bashrc file or .profile file.
```
cd ~
gedit .bashrc
gedit .profile
```
To permanently add the path for everyone who uses the system, you need to put the command in the /etc/profile file.
```
sudo gedit /etc/profile
```

### SSH
- https://linuxconfig.org/quick-guide-to-enabling-ssh-on-ubuntu-24-04
- https://linux-tips.us/use-your-hostname-with-ssh-instead-of-your-ip-address-in-ubuntu/

- to ssh into a remote computer just need to add .local to the hostname of the computer

### Nvidia CUDA
Configure ubuntu to detect nvidia gpu
- https://www.linuxbabe.com/desktop-linux/switch-intel-nvidia-graphics-card-ubuntu

- use this command to check which is the recommended driver
    ```
    sudo ubuntu-drivers devices
    ```

- before installing the driver. Check if you already have any nvidia driver installed. If so, remove them, so that you can install a fresh new driver
    ```
    sudo apt purge nvidia-*
    ```

- install the recommended device. My screen blackout when installing it. I had to force restart my computer. Everything seems to be fine after my hard restart. 
    ```
    sudo apt-get install nvidia-driver-535
    ```

- You can check if your system detects the graphic card by going to Settings -> about
- you can change the primary graphic card to use with the nvidia-settings application

### Thunderbird outlook sync
- https://support.mozilla.org/en-US/questions/1363441

1. Install these 2 add-on
    - tbsync
    - provider for exchange active sync

2. once installed go to tbsync -> account action -> add account -> microsoft office 365
    - key in the necessary credentials

### Make appimage as an ubuntu desktop app
- https://askubuntu.com/questions/902672/registering-appimage-files-as-a-desktop-app?rq=1
- https://askubuntu.com/questions/162612/how-can-i-add-an-application-to-the-list-of-open-with-applications

1. Make sure the appimage is executable
    ```
    chmod u+x path/to/appimage
    ```
2. You might need to install the libfuse library for the appimage to work.
    - https://github.com/OpenShot/openshot-qt/issues/4789
    ```
    sudo apt install libfuse2
    ```

3. for librecad
    ```
    [Desktop Entry]
    Type=Application
    Name=Librecad
    Comment=Librecad
    Exec=/home/usr/Applications/LibreCAD-2.2.0.2-x86_64.AppImage %F
    Terminal=false
    Categories=Librecad,cad
    ```
3. put the file in ~/.local/share/applications 

### LibreOffice
Zotero plugin for libreoffice
https://www.libreofficehelp.com/how-to-fix-libreoffice-requires-a-java-runtime-environment-error/

#### Install new fonts
- https://www.libreofficehelp.com/install-fonts-libreoffice-openoffice/#GNULinux_Ubuntu_Linux_Mint_Fedora_etc
- download the font file ttf or otf here (https://www.dafont.com/search.php?q=linux+libertine) 
- extract and put it in the share font folder in ubuntu, /usr/share/fonts.

### Install Inkscape
- https://www.linuxcapable.com/how-to-install-inkscape-on-ubuntu-linux/

1. Install dependencies
    ```
    sudo apt install software-properties-common apt-transport-https
    ```
2. Import the PPA
    ```
    sudo add-apt-repository ppa:inkscape.dev/stable -y
    ```
3. Install inkscape
    ```
    sudo apt install inkscape
    ```

### Install .run files
- https://www.wikihow.com/Execute-.RUN-Files-in-Linux

1. Make your file executable
    ```
    sudo chmod +x yourfilename.run
    ```

2. execute your file with this command
    ```
    sudo ./yourfilename.run
    ```

### 7zip
- https://www.e2encrypted.com/howtos/how-to-securely-encrypt-decrypt-with-7zip-via-command-line/

Install 7zip
```
sudo apt install 7zip
```
zip a file with encryption
```
7zz a -mhe=on nameofyourfile.7z path/2/zip -p
```
extract a file
```
7zz x nameofyourfile.7z
```

### ffmpeg
- https://linuxconfig.org/recording-live-streams-on-linux-with-ffmpeg-examples-included
- https://stackoverflow.com/questions/67320808/splitting-an-audio-file-into-equal-lenght-segments-using-ffmpeg
- ffmpeg to record audio output - https://trac.ffmpeg.org/wiki/Capture/PulseAudio
- 
split mp3 into equal segments
```
ffmpeg -i input.mp3 -f segment -segment_time 2 output_%03d.mp3
```

split mp3 according to start end time
```
ffmpeg -i input.mp3 -ss 00:01:00 -to 00:02:00 -c copy output.mp3
```