# LCD I2C Libary  

This tutorial shows you how to control an I2C-based LCD Display with a custom library. Let's get started!


# The Setup


## Downloading Software

We first need to install some software that this library requires:
```
opkg update
opkg install git git-http python-light pyOnionI2C
```

Next, we will download the custom I2C LED library:
```
cd /root
git clone https://bitbucket.org/fires/fireonion_i2c_lcd
```

## Connecting the LCD Display

Make the following connections:

| Omega Pin | LCD Display Pin |
|-----------|-----------------|
| 5V        | 5V              |
| GND       | GND             |
| 20        | SCL             |
| 21        | SDA             |

![Omega->LCD Connection](http://davidstein.cz/wp-content/uploads/1450851881456-image-1010x1024.png)

## Running the LCD Library Code

Navigate to the `src` directory:
```
cd fireonion_i2c_lcd/src
```

Run the command:
```
python lcd.py
```


Now you should see sample text on your LCD. You can edit `lcd.py` to change the displayed text. 

![Sample Output](http://davidstein.cz/wp-content/uploads/IMG_20160313_133731.jpg)

You can also import the whole library into your own python project!



# More
More and latest info at : [http://davidstein.cz/2016/03/13/onion-io-i2c-lcd-16x220x4-backpack-library/
](http://davidstein.cz/2016/03/13/onion-io-i2c-lcd-16x220x4-backpack-library/)**


## To Do

* Add support to call this library from shell with arguments to make it more confortable use with other programming languages
* Test on 20x4 display. IF SOMEBODY GOT a 20x4 Display please test it and send feedback.



# Acknowledgements

Thanks to [Fires04](https://github.com/Fires04) for writing this tutorial! 
