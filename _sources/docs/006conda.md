# Python
## Python
install python for ubuntu https://computingforgeeks.com/how-to-install-python-on-ubuntu-linux/?expand_article=1
```
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt install python3.11
sudo apt install python3-pip
sudo apt install python3.11-venv
python3.11 -m venv /home/chenkianwee/venv/geomie3d
```

if your spyder give this error msg 
```
'Could not load the Qt platform plugin "xcb" in "" even though it was found.
This application failed to start because no Qt platform plugin could be initialized. Reinstalling the application may fix this problem.'
```

install these libraries to solve the problem https://stackoverflow.com/questions/62391587/qt-could-not-find-the-qt-platform-plugin-xcb 
```
sudo apt install make g++ pkg-config libgl1-mesa-dev libxcb*-dev libfontconfig1-dev libxkbcommon-x11-dev python libgtk-3-dev
```
create a virtual environment with Python
```
py -m venv c:\path\to\new\virtual\environment
```
activate environment for ubuntu
```
source bin/dir/activate
```
## Conda
create environment
```
conda create --name myenv python=3.9
```
remove an environment
```
conda remove --name myenv --all
```
update conda
```
conda update -n base -c defaults conda
```
remove cache and unused cache packages
```
conda clean -a
```
