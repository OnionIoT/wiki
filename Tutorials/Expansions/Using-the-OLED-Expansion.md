# Using the OLED Expansion

The OLED Expansion allows you to display images and text on the small black and white OLED Display. It can be used to display project status, sensor info, or other information from the Omega, allowing you to have data from the digital world displayed in the real world!

![Omega+OLED Expansion](http://i.imgur.com/tqcRlgG.jpg)


[[_TOC_]]


[//]: # (Hardware)

## The Hardware
The OLED Expansion has a 0.96" 128x64 black and white OLED display. When displaying text, there are 8 rows, with 21 possible characters in each row.

[//]: # (LAZAR: EXPAND ON THIS WITH AN IMAGE)



[//]: # (Using oled-exp)

## Using the OLED Expansion

*Make sure that your Omega has the latest firmware!*

A tool called `oled-exp` will be your helper in all things related to the OLED Expansion.

Also available are a C library and Python module that will allow you to create your own programs that use the OLED Expansion.


### Command Usage

For a print-out of the command's usage, run it with just a -h argument

```
oled-exp -h
```

In general, the commands will be in this form:
```
oled-exp [OPTIONS] COMMAND PARAMETER
```


### Programming Flow

When it is first powered on, the OLED Expansion must first be programmed with an **initialization sequence** in order to accept further commands. 

After the initialization, it can accept a series of commands to do the following:
* clear the display
* turn it on and off (while preserving the image)
* invert the display colours
* dim the brightness
* set the text cursor to a particular position
* write a message
* enable horizontal or diagonal scrolling
* display an image

A single `oled-exp` call can have one or many cascaded commands:
```
// single command
oled-exp write "Hello!"

// multiple cascaded commands (execute in order from left to right)
oled-exp write "Onion Omega\nMy favourite" dim on cursor 4,0 write ":)"
```


#### Command and Option Table

To reconcile all of the above features to `oled-exp`, refer to the table below:

| Action                    | Option/Command            |
|---------------------------|---------------------------|
| initialization            | `-i`                      |
| clear the display         | `-c`                      |
| toggle display on or off  | `power <on|off>`          |
| invert colours            | `invert <on|off>`         |
| dim the screen            | `dim <on|off>`            |
| set the cursor            | `cursor <row>,<column>`   |
| write text                | `write <string>`          |
| enable scrolling          | `scroll <direction>`      |
| display an image          | `draw <lcd file>`         |



### Options

The option arguments can be run by themselves or in conjunction with any other command. They will always be executed first.


#### Initialization

To initialize the display to accept further commands:
```
// just initialize the display
oled-exp -i 	

// initialize the display and write a message
oled-exp -i write "Freshly initialized"
```


#### Clear the Screen

To clear the screen and set the cursor to the top left:
```
// just clear the screen
oled-exp -c

// clear the screen and draw an image
oled-exp -c draw /root/onion.lcd
```




### Commands
The commands are more complex and each requires one argument.


#### Power

Turn the display on or off. Can be used to toggle the display after it has been initialized.
```
oled-exp power <on|off>
```

Note that any text or images on the screen will be preserved.


#### Invert

Invert black and white on the display. Setting to `on` will enable the invert, setting to `off` will disable the inversion.
```
oled-exp invert <on|off>
```


#### Dim

Enable dimming the display. Setting to `on` will dim the display, setting to `off` will restore the default brightness.
```
oled-exp dim <on|off>
```


#### Write

Write a string to the display *at the current position of the cursor*:
```
oled-exp write <string>
```

If the string contains spaces or any special characters (newlines, !?(){}[] etc), it needs to be wrapped with double quotes:
```
oled-exp write "This is so exciting!\nIsn't it?!?!\n\n(I love the Omega)"
```

A list outlining the supported characters can be found in a [section below.](#supported-characters)




#### Set the Cursor Position

Set the cursor position on the display, any writes after this command will start at the specified row and column.

The `row` parameter represents each character row (8 pixel rows) on the display, so the range is **0 to 7**

The `column` parameter represents each character column, the range is **0 to 20**
```
oled-exp cursor <row>,<column>
```

**Examples**

To write `Hello` on the first line, and then `How are you?` on the 4th row:
```
oled-exp write Hello cursor 3,0 write "How are you?"
```

Set the cursor to the start of the last character row:
```
oled-exp cursor 7,0
```

Set the cursor to the middle of the 2nd row:
```
oled-exp cursor 2,10
```



#### Scrolling

The OLED can scroll whatever is on the screen horizontally or diagonally upwards. The text/image will wrap around the edges.
``` 
oled-exp scroll <direction>
```

Supported directions:
* left
* right
* diagonal-left
* diagonal-right
* stop
  * To disable scrolling



![left scroll](http://i.imgur.com/nytY4Xw.gif)

![diagonal-right scroll](http://i.imgur.com/9JcoKEj.gif)



#### Displaying Images

Display an image from an LCD file on the display. 

**For now use the OLED App on the Console!**
*More details on this coming soon, stay tuned!*



[//]: # (SUPPORTED CHARACTER LIST)

## Supported Characters

The OLED Display supports the following characters:

* SPACE
* !
* "
* #
* $
* %
* &
* '
* (
* )
* *
* +
* ,
* -
* .
* /
* 0
* 1
* 2
* 3
* 4
* 5
* 6
* 7
* 8
* 9
* a
* b
* c
* d
* e
* f
* g
* h
* i
* j
* k
* l
* m
* n
* o
* p
* q
* r
* s
* t
* u
* v
* w
* x
* y
* z
* A
* B
* C
* D
* E
* F
* G
* H
* I
* J
* K
* L
* M
* N
* O
* P
* Q
* R
* S
* T
* U
* V
* W
* X
* Y
* Z
* :
* ;
* <
* =
* >
* ?
* @
* [
* \
* ]
* ^
* _
* `
* {
* |
* }
* ~



[//]: # (Using the Libraries)

## Using the Libraries

The C library and Python module will give you the flexibility to use the OLED Expansion however you want in your own programs.

For more information, see [this guide](../Libraries/OLED-Expansion-Library).


