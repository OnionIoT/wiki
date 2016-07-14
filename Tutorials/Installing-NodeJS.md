# Installing NodeJS

The Omega supports NodeJS 4.3.1!



# Installation

Connect to the Omega's terminal using either SSH or Serial.

Run the following commands on the terminal:
```
opkg update
opkg install nodejs
```

This will take about 8MB of space on the Omega. 

Due to the 16MB size of the Omega's flash storage, this installation will take up a sizeable chunk of the Omega's memory. We recommend running the [Omega's OS from USB storage](./Using-USB-Storage-as-Rootfs) to have additional space for other programs and files.



## Installing npm

The Omega also supports npm:
```
opkg update
opkg install npm
```

The installation will take about 9MB of space. 

Since npm requires node to be installed, it cannot fit on the flash storage on the Omega. You **must** run the [Omega's OS from USB storage](./Using-USB-Storage-as-Rootfs) in order to have enough space for this installation. 