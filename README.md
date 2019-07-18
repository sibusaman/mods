# Todo
- processes
    - three-axis rough cut
    - three-axis finish cut
- editing
    - subgraph copy paste
    - nested module graphs
- ui
    - collapse nodes
    - refactor for skinning
- Cross-Origin Resource Sharing (CORS)
- formats
    - HPGL input
    - SVG export
    - DXF export
- ...

# To install and run mods locally

You need to first install [node.js](https://docs.npmjs.com/getting-started/installing-node).

We install the [NVM](https://github.com/nvm-sh/nvm) (Node Version Manager) and install the node using NVM. This should work in all linux distributions.

get the install script and run it
```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash

bash install_nvm.sh # install
```

Add the NVM command to the shell

```bash
source ~/.profile
```

Now, install the version of NODE you want. The following will install the latest LTS version.

```bash
nvm install 10
```

Install 

Install the [http-server](https://www.npmjs.com/package/http-server) npm package. Including '-g' sets the installs the package globally, allowing you to use it as a command line tool:

```bash
sudo npm install http-server -g
```

Clone the mods repository:

```bash
git clone https://gitlab.cba.mit.edu/pub/mods.git
```

Use the command line to navigate to the root of the mods repository and start the server with

```bash
http-server
```
or
```bash
hs
```

Open a browser tab and go to ```127.0.0.1:8080``` which is the same as ```http://localhost:8080``` to view the server that you just started.

Depending on how to need to use mods you can start local servers located in ```mods/js```, for example, if you start from the root of the mods repository:

```bash
 cd js
```

and run servers

```bash
node serialserver.js ::ffff:127.0.0.1 1234
```

For these servers you will need additional npm packages. You could install them locally within the mods repo. Navigate to the ```mods``` or ``` mods/js ``` and run (without the ```-g``` for local installation)

```bash
npm install serialport ws
```

# Machine Problems

## Wrong SRM-20 Origin

#### Problem : SRM-20 not moving to the real, machine origin when we ask it to move to x,y = 0,0

#### Cause : The It appears that we are working in the user coordinate system of the machine. This is stored in the machine's EEPROM.

#### Solution : As of now, we know we can change this from the V-Panel in Windows. So, open the V-Panel, move to machine-coordinate's origin, and then set this as the user-coordinate's origin. We will see if we can do this in Linux.

##### Credits : [Ahmed-Khairy](https://github.com/ahmed-khairy)




# Mods Connection Debugging

set correct serial port permission (do this each time you reboot the machine): ```chmod a+rwx /dev/ttyUSB0```

start serialserver in the terminal so you can see the logs as it tries to connect.  navigate to the mods/js folder in the terminal (probably use ```cd ~/mods/js```) and type: ```node serialserver.js ::ffff:127.0.0.1 1234```

check serialserver is running with: ```ps aux | grep node```

# Common Issues

1. **_Help! My SRM-20 will only run a single job and then go dead!_** Chances are you are using printserver.js instead of deviceserver.js to connect to the machine.  For now, we need to treat the SRM-20 as a device instead of a printer.
2. **_Argg... why do I need to reset permissions on /dev/usb/lp0 every restart?_**  You can use `sudo add_user username lp` and `sudo add_user username lpadmin` to make persistent permissions.
3. **_Why is my web socket connection refused when the addresses are the same?_** This can happen due to a difference between IPV4 and IPV6 addresses.  In your start mods server script, try changing 127.0.0.1 to ::ffff:127.0.0.1 and see if it helps.



