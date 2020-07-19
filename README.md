# competitive-programming-practice
Notes written while practicing coding problems

## Environment Setup
Follow the instructions at [Using C++ and WSL in VS Code](https://code.visualstudio.com/docs/cpp/config-wsl) to setup WSL in windows for C++ development. Use the files inside .vscode folder of the repo to add snippets relevant to competitive programming.
Create these additional files: 
```
touch input.txt output.txt CP.cpp
```
Use layout options to arrange the files in the view according to convenience.
Open CP.cpp and type precode to load the template code to start coding.

## Execution Instructions
```
To complile: Press Ctrl + Shift + B
Then go to the console: Ctrl + `
Press any key and execute the executable: ./CP
```
## Using Hightail for automated testing
Follow the instructions at https://www.youtube.com/watch?v=IL7Jd9rjgrM to setup GUI and wsl2. This is needed as Hightail uses gui environment. 
### To setup WSL2
~~~
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
wsl --set-default-version 2
~~~

### To setup Ubuntu GUI
~~~
sudo apt update && sudo apt -y upgrade
sudo apt-get purge xrdp
sudo apt install -y xrdp
sudo apt install -y xfce4
sudo apt install -y xfce4-goodies

sudo cp /etc/xrdp/xrdp.ini /etc/xrdp/xrdp.ini.bak
sudo sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini
sudo sed -i 's/max_bpp=32/#max_bpp=32\nmax_bpp=128/g' /etc/xrdp/xrdp.ini
sudo sed -i 's/xserverbpp=24/#xserverbpp=24\nxserverbpp=128/g' /etc/xrdp/xrdp.ini
echo xfce4-session > ~/.xsession

sudo nano /etc/xrdp/startwm.sh
comment these lines to:
#test -x /etc/X11/Xsession && exec /etc/X11/Xsession
#exec /bin/sh /etc/X11/Xsession

add these lines:
# xfce
startxfce4
~~~
Till now it was one time setup. The following lines have to be run everytime, wsl gets restarted.
~~~
sudo /etc/init.d/xrdp start

Now in Windows, use Remote Desktop Connection
localhost:3390
then login using your Ubuntu username and password
~~~
### Testing with Hightail
 - Download hightail jar from https://github.com/dj3500/hightail.
 - Make it executable and launch from the RDP session.
 - Use Ctrl + Alt + Shift + B to run the test task instead of the build task. This one has the -DONLINE_JUDGE flag set which skips the freopen commands. This allows hightail to send the input.

