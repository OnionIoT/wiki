# Using PWM with the Expansion Dock

Pulse Width Modulation (PWM) is possible with the Omega plus just the Expansion Dock. It is software-based PWM and although it isn't quite as accurate as hardware-based PWM, it allows you to do most of the things you need PWM to accomplish.

## Software PWM vs Hardware PWM

Software PWM is implemented by a program that usually waits for a defined amount of time before toggling the GPIO output. This has the potential to be inaccurate since the CPU might be interrupted with other processes and tasks. Software PWM is generally good enough for dimming an LED but not for something requiring more accuracy, such as driving a servo.

Hardware PWM is done by programming a micro-controller that will output cycle-accurate PWM signals. The Servo Expansion has a micro-controller that can be programmed to provide cycle-accurate PWM.

## PWM on the Expansion Dock

If you require PWM using the expansion dock and don't need it to be 100% accurate, we've built a tool that can provide software-based PWM.

Usage:

```
fast-gpio pwm <gpio> <freq in Hz> <duty (out of 100)>
```

Example:

```
fast-gpio pwm 14 200 25
```

The command above will set GPIO 14 to do PWM at 200 Hz with a 25% duty cycle. 

This means that during the 5ms period (P = 1/f), 25% of the time the signal will be HIGH and LOW the remaining 75% of the time.

This image provides a good illustration:

![25% Duty Cycle](//i.imgur.com/A8W1FLw.gif)

Here are some examples of different duty cycles:

![Duty Cycles](//i.imgur.com/7Am0NDB.png)