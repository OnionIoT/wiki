Flashing your onion omega via serial
===================
Credits to @Zheng Han and @johannes-zellner for the images and content!

@Chris McCaslin for making the nice wiki post 

**Required Materials:**

 - ( **8** )  100Ω* *resistors* 
 - ( **5** ) 0.1µF* *capacitors* 
 
 *Note: There is no exact requirement for the capacitors and resistors values, however capacitor shouldn't be too big. (for example: 0.1µF and 100Ω should work)

The schematic
--
![Schematic by Zheng Han](https://onion-cdn.s3.amazonaws.com/community/images/transformerless_ethernet.jpg)

The Breadboard
--
![Image from johannes-zellner](https://community.onion.io/uploads/files/1448652521918-2015-11-27_omega_breadboard_network.jpg)

The process
--
Here are the necessary steps after wiring the network connection:

 1. boot the good omega -- let's call it omega-good
 
 2. connect the bricked omega (omega-bricked) with an USB cable with a linux pc (expansion switch still off)
 
 3. connect to omega-bricked with `screen /dev/ttyUSB0 115200`
 
 4. power on the switch of the expansion board of omega-bricked and immediately hit enter in the screen session to get into uboot on omega-bricked
 
 5. printenv in uboot on omega-bricked shows that the ethernet ip is configured to be `192.168.1.1`

 6. configure the ethernet connection of omega-good to use the static ip `192.168.1.100`, either by modifying /etc/config/network and restarting the network or by something like ifconfig eth0 `192.168.1.100` up
 
 7. While in uboot in omega-bricked verify with ping 192.168.1.100 that the network connection to omega-good works
 
 8. While in uboot in omega-bricked start httpd
 
 9. create an ssh tunnel from the linux pc to omega-bricked via omega-good with `ssh -L 8080:192.168.1.1:80` omega-good (ssh connection to omega-good is via wifi)
 
 10. open `http://localhost:8080/` in a browser on the linux pc. The ssh tunnel redirects this to the httpd of omega-bricked → you see the uboot httpd upgrade page in your browser and can proceed to upload a bin image.

