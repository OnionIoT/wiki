# Setup Omega for the Onion Cloud

## What is the Onion Cloud?

The Onion Cloud is an online service that maps an IoT device (in our case the Omega) to the Web as a set of RESTful APIs. These APIs are no different from the Restful APIs of typical Web services such as Facebook or Twitter, and they allow these IoT devices to be accessed remotely via standard API calls. Now, you can get physical devices to interact with your software, other Web services, or to get the devices to interact with each other.

## Method 1: Setup using the Console

### Step 1: Upgrade the Omega to the Newest Firmware

The first step is to perform an upgrade of the Omega firmware. This installs all the required packages to communicate with the Onion Cloud, and also ensures that these packages have all the latest bug fixes and security patches.

First, make sure that your Omega is connected to the Internet. Then, to perform the upgrade, log into the Linux commandline using the **Terminal** app on the Console, and run the following command:

```bash
oupgrade -l
```

The Omega will download the newest firmware and start the firmware upgrade. **Please DO NOT roboot or uplug the Omega when the upgrade is taking place, doing so will risk bricking the device.**

### Step 2: Log into the Onion Cloud

Once the firmware upgrade has completed, open up a new window on your browser and navigate to `https://cloud.onion.io`. Login with your Onion Account (the same login you use for the Community and the Store).

![Cloud UI](https://i.imgur.com/gNWWwVy.png)

Once you are logged in, click on the **Device Manager** app. This will be the app that you will be using to manage all your devices as well as devices that other people have shared with you.

![Device Manager](https://i.imgur.com/Rbc55Cm.png)

### Step 3: Create a New Device on the Onion Cloud

In the **Device Manager** app, click on the **New Device** button on the toolbar. A window will pop up and prompt you for the Name and Description of your device, then press **Create Device**.

![Create New Device](https://i.imgur.com/uvE5hCw.png)

### Step 4: Generate the Setup Code

You should see you device being added under a table called **My Devices**. Now click on the device that you have just created to enter the detailed view.

![Device Detail](https://i.imgur.com/WqcrF0y.png)

Click on the **Setup Code** button on the toolbar, a window will pop up and a display an one-time setup code.

![Setup Code](https://i.imgur.com/t2iJgZo.png)

Highlight the code and copy it to your clipboard.

### Step 5: Setup the Omega

Finally, return to the console on the Omega you want to connect to the Onion Cloud, and open the **Settings** app.

![Console Settings](https://i.imgur.com/aq6r19A.png)

To the left, you should see a tab called **Cloud**, click on it and enter the setup code you have just copied and click on **Save**.

And that's it! Your Omega is now connected to the Onion Cloud!

For more information on how to use the Onion Cloud, please take a look at the tutorial here.

## Method 2: Setup using the Command Line

### Step 1: Upgrade the Omega to the Newest Firmware

The first step is to perform an upgrade of the Omega firmware. This installs all the required packages to communicate with the Onion Cloud, and also ensures that these packages have all the latest bug fixes and security patches.

First, make sure that your Omega is connected to the Internet. Then, to perform the upgrade, log into the Linux commandline via `ssh` or the serial terminal, and run the following command:

```bash
oupgrade -l
```

The Omega will download the newest firmware and start the firmware upgrade. **Please DO NOT roboot or uplug the Omega when the upgrade is taking place, doing so will risk bricking the device.**

### Step 2: Log into the Onion Cloud

Once the firmware upgrade has completed, open up a new window on your browser and navigate to `https://cloud.onion.io`. Login with your Onion Account (the same login you use for the Community and the Store).

![Cloud UI](https://i.imgur.com/gNWWwVy.png)

Once you are logged in, click on the **Device Manager** app. This will be the app that you will be using to manage all your devices as well as devices that other people have shared with you.

![Device Manager](https://i.imgur.com/Rbc55Cm.png)

### Step 3: Create a New Device on the Onion Cloud

In the **Device Manager** app, click on the **New Device** button on the toolbar. A window will pop up and prompt you for the Name and Description of your device, then press **Create Device**.

![Create New Device](https://i.imgur.com/uvE5hCw.png)

### Step 4: Generate the Setup Code

You should see you device being added under a table called **My Devices**. Now click on the device that you have just created to enter the detailed view.

![Device Detail](https://i.imgur.com/WqcrF0y.png)

Click on the **Setup Code** button on the toolbar, a window will pop up and a display an one-time setup code.

![Setup Code](https://i.imgur.com/t2iJgZo.png)

Highlight the code and copy it to your clipboard.

### Step 5: Setup the Omega

Finally, return to the Linux commandline on the Omega you want to connect to the Onion Cloud, and run the following command:

```bash
onion-cloud setup <setup code>
```

where `<setup code>` is the code that you just copied from the Onion Cloud.

And that's it! Your Omega is now connected to the Onion Cloud!

For more information on how to use the Onion Cloud, please take a look at the tutorial here.