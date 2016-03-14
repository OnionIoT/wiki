# FireOnion I2C LCD  #

Hi, guys. I write a little library for you.

Now you can control your I2C LCD backpack easy way.

**Easy setup:**

1. setup your Onion Omega and connect to your wifi
2. connect your onion to I2C backpack 
	1. 5V to 5V pin
	2. GND to GND pin
	3. Onion pin 20 to SCL
	4. Onion pin 21 to SDA
	5. ![](http://davidstein.cz/wp-content/uploads/1450851881456-image-1010x1024.png =300x300)
3. connect to your Onion Omega via SSH or COM port
4. run commands
	1. **opkg update**
	2. **opkg install python-light pyOnionI2C**
5. switch you to folder where you want download library (for example: cd /usr/)
6. run command **git clone https://bitbucket.org/fires/fireonion_i2c_lcd**
7. move to src directory **cd fireonion_i2c_lcd/src**
8. run command **python lcd.py**

**!! ALL DONE !!**

now you should see testing text on your LCD. You can edit lcd.py to edit text. Also you can import whole library to your python project.

![](http://davidstein.cz/wp-content/uploads/IMG_20160313_133731.jpg =300x300)


**More and latest info at : [http://davidstein.cz/2016/03/13/onion-io-i2c-lcd-16x220x4-backpack-library/
](http://davidstein.cz/2016/03/13/onion-io-i2c-lcd-16x220x4-backpack-library/)**

**ToDO**

- add support to call this library from shell with arguments to make it more confortable use with other programming languages
- test on 20x4 display. IF SOMEBODY GOT a 20x4 Display plase test it and send feedback.

