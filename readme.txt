server auto start ngrok raspberry

after instaling ngrok on the raspberry and
supposing that the pass is :    /home/sam

First step is editing crontab file :

in the      /tmp/crontab.Qz85jC folder   there is a file named crontab . this file is executed line by line by the operating system when booting or at sheduled times . we will add a line at the end of this file that launch a python file called booting.py

open terminal and type : crontab -e 
to edit crontab file and add in it as last line: 

@reboot /usr/bin/python3 /home/sam/startup/booting.py >> /home/sam/startup/booting.log 2>&1

then press control+o then enter to save
then control+x to exit

second step is creating the file booting.py :

we will create a python file that will be executed once each time the system boot . for this open thonny editor on the raspberry pi then new file : 


import subprocess
import time

# Function to launch the .sh file
def launch_sh_file():
    subprocess.Popen(["/bin/bash", "/home/sam/startup1.sh"])
    time.sleep(5)
    subprocess.Popen(["/bin/bash", "/home/sam/startup2.sh"])
    time.sleep(3)
    
time.sleep(3)
launch_sh_file()

then save it in : /home/sam/startup/booting.py
this script will delay 3 seconds for the system to start then launch in the terminal background startup1.sh , wait 5 second , launch startup2.sh 
startup1.sh is an executable bach file that launch ngrok
startup2.sh is alsan executable bach to launch the sever

third step is to creating startup1.sh :

open terminal and type : sudo nano startup1.sh

then type the folowing bach command : 



#!/bin/bash

# Create a marker file to indicate the script ran
touch /home/sam/ngrok_started.txt

# Ensure the correct PATH for ngrok
export PATH=$PATH:/usr/local/bin

# Kill any existing ngrok or Python HTTP server processes
pkill ngrok
pkill -f 'python3 -m http.server'

# Start ngrok with the specified URL
/usr/local/bin/ngrok http 8000 --url perrier-tatient-rongoose.ngrok-free.app


fourth step is creating startup2.sh :

open terminal and type : sudo nano startup2.sh

then type the folowing bach command : 



#!/bin/bash

touch /home/sam/server_started.txt

python3 -m http.server 8000

fifth step is making these files executables : 

after creating :      
     /home/sam/startup1.sh      and  
     /home/sam/startup2.sh.    in these exact locations
according to  our booting.py , we should now making them executables by :  

open terminal and type : 

sudo chmod +x startup1.sh     enter
sudo chmod +x startup2.sh    enter

*****************************************************************

installing ngrok on raspber


open terminal : 

1-
 wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-stable-linux-arm.tgz

2-
tar -xvzf ngrok-stable-linux-arm.tgz

3-
sudo mv ngrok /usr/local/bin

4-
ngrok --version

5-
ngrok config add-authtoken 3jhgjhgkjgp8lA9YbImKz0iOhjgjhUfNE1H1PkghgjhgjhgjhN


