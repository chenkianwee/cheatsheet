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

check the size of top level directories 
```
sudo du -sh /*
```

List a directory
```
ls
```

List with details, or show all hidden files
```
ls -la file/foldername
```
```
ls -l file/foldername
```

Will be able to see size in human readable way
```
ls -lh file/foldername
```

Only see the hidden file
```
ls -ld .?* 
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

zip a folder but exclude certain folder
```
zip -r ../archive.zip folder2zip -x "folder2zip/folder2/*"
```

zip a folder but include certain folder
```
zip -r ../archive.zip folder2zip folder2zip/folder2include
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

### FreeCAD default app for .fcstd extension
- https://forum.freecad.org/viewtopic.php?style=5&f=4&t=63536
- https://askubuntu.com/questions/1180012/how-to-change-default-program-for-files-ending-in-one-extension

- This is for freecad installed with snap.
1. Go to /usr/share/mime/packages, cut and paste the following xml (freecad.xml) file into it.
```
<?xml version="1.0" encoding="UTF-8"?>
<mime-info xmlns='http://www.freedesktop.org/standards/shared-mime-info'>
	<mime-type type="application/x-extension-fcstd">
		<sub-class-of type="application/zip"/>
		<comment>FreeCAD document files</comment>
		<glob pattern="*.fcstd"/>
	</mime-type>
</mime-info>
```
```
sudo cp path/to/freecad.xml /usr/share/mime/packages/
```
2. Once you have copied the file. Go to /var/lib/snapd/desktop/applications/freecad_freecad.desktop and you can see the desktop file. Make sure the following is in the file.
```
MimeType=application/x-extension-fcstd
```
3. Update your mimetype database with the following command.
```
sudo update-mime-database /usr/share/mime
```
4. Now you should be able to double click fcstd files and open it with freecad.

### Openstudio default app for .osm extension
1. Go to /usr/share/applications and go to the openstudioapp.desktop file. Make sure the following is in the file
```
MimeType=application/x-openstudio;
```
2. Go to /usr/share/mime/packages, cut and paste the following xml (openstudioapp.xml) file into it.
```
<?xml version="1.0" encoding="UTF-8"?>
<mime-info xmlns='http://www.freedesktop.org/standards/shared-mime-info'>
	<mime-type type="application/x-openstudio">
		<comment>Openstudio Application document files</comment>
		<glob pattern="*.osm"/>
	</mime-type>
</mime-info>
```
```
sudo cp path/to/openstudioapp.xml /usr/share/mime/packages/
```
3. Update your mimetype database with the following command.
```
sudo update-mime-database /usr/share/mime
```

### LibreOffice
Zotero plugin for libreoffice
https://www.libreofficehelp.com/how-to-fix-libreoffice-requires-a-java-runtime-environment-error/

#### Install new fonts
- https://www.libreofficehelp.com/install-fonts-libreoffice-openoffice/#GNULinux_Ubuntu_Linux_Mint_Fedora_etc
- download the font file ttf or otf here (https://www.dafont.com/search.php?q=linux+libertine) 
- extract and put it in the share font folder in ubuntu, /usr/share/fonts.

### Install Inkscape
- with ubuntu 24.04 you can just install it using the snap store. It is the latest of inkscape.
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
- ffmpeg and youtube-dl: https://linuxconfig.org/how-to-rip-songs-from-youtube-videos
- ytdlp: https://www.unixmen.com/yt-dlp-download-youtube-videos/

split mp3 into equal segments
```
ffmpeg -i input.mp3 -f segment -segment_time 2 output_%03d.mp3
```

split mp3 according to start end time
```
ffmpeg -i input.mp3 -ss 00:01:00 -to 00:02:00 -c copy output.mp3
```

convert .webm to .mp4
```
ffmpeg -i input.webm' -c:v libx264 -c:a aac output.mp4
```
```
if you get similar error [libx264 @ 0x55de9ce50a40] height not divisible by 2 (1850x977) add the -vf command
ffmpeg -i input.webm' -c:v libx264 -c:a aac -vf "pad=ceil(iw/2)*2:ceil(ih/2)*2" output.mp4
```

compress mp4 good quality
```
ffmpeg -i input.mp4 -vcodec h264 -acodec mp2 out.mp4
```

compress mp4 ok quality but smaller
```
ffmpeg -i input.mp4 -s 1280x720 -acodec copy -y output.mp4
```

compress mp4 lowest quality
```
ffmpeg -i input.mp4 -vcodec h264 -b:v 1000k -acodec mp3 output.mp4
```

get only the audio, -ab can be 128k or 256k
```
ffmpeg -i "something.mkv" -ab 320k 'something.mp3'
```

download video with url
```
yt-dlp -o new_name.mp4 "something.com"
```

download audio-only with url 
```
yt-dlp -x --extract-audio --audio-format mp3 --audio-quality 128k -o something.m4a something.com
```
## RaspberryPi
- change wifi cmd
```
sudo raspi-config
```

## Fix window time syncing issue with ubuntu dual boot
- change window time to utc - https://www.youtube.com/watch?v=xO0lPxrtFCw
    - https://askubuntu.com/questions/169376/clock-time-is-off-on-dual-boot#:~:text=If%20you%20were%20to%20just,default/rcS%20and%20press%20Enter%20.&text=If%20you%20do%2C%20change%20UTC,sure%20it%20says%20UTC=no%20.

1. In Windows, go to Settings > Time & language > Date & time.
2. Toggle Set time automatically off.
3. Click Sync now under "Sync your clock" to manually set the time.
4. You will need to perform a registry edit to make Windows use UTC.
5. Press Windows Key + R, type regedit, and press Enter.
6. Navigate to HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation.
7. Right-click in the right pane, choose New > DWORD (32-bit) Value, and name it RealTimeIsUniversal.
8. Double-click the new value, set its Value data to 1, and click OK.
9. Restart your computer. 

## GlobalProtect installation with ubuntu 24.04
- princeton university guide - https://princeton.service-now.com/service?id=kb_article&sys_id=125a7bb11b6b4850435885d56b4bcb05
- northwestern university guide - https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1420

1. Download the global protect installation file using the princeton university guide
2. install it with the following command 
    ```
    sudo apt-get install libqt5webkit5
    sudo dpkg -i GlobalProtect_UI_deb-6.2.1.1-276.deb
    ```
3. Once installed go to the installed directory
    ```
     cd /opt/paloaltonetworks/globalprotect
    ```
4. Add  <default-browser>yes</default-browser> under <Settings> (Note: Do not add quotations.)
    ```
    sudo nano pangps.xml
    ```
5. Go to the startup application program and untick globalprotect so that it does not auto start everytime.

## Princeton University Printer
1. Go to the ubuntu software center and download the printer settings application from the debian packages channel
2. Go to Add -> Network Printer -> LPD/LPR Host or Printer
    ```
    (refer to the printer config instruction from the university)
    Host: username@ss226w.princeton.edu
    queue: TigerPrint 
    ```
3. For the printer driver Generic -> PostScript -> Generic PostScript Printer Foomatic 
4. Might need to be connected to vpn to print, currently doesnt work for direct print from browser.