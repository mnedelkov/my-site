---
title: "ubuntu_firefox_kiosk_guide"
date: 2023-01-29T11:03:55-07:00
draft: false
toc: false
images:
tags:
  - documentation
  - linux
---

**Description:**

This is a guide on how to set up a Firefox Kiosk. The kiosk may only be used for browsing the web on a configured machine. Basically, once setup, the machine will boot directly into a specific webpage. The machine will be unable to access other applications, such as: terminal, settings, other browsers.

*This guide utilized Ubuntu 22.04 LTS*

---

**Process:**

To get it setup, we need to create a brand new desktop file to be executed upon selection from the Ubuntu login menu (formatted like file.desktop).

1. First, open a terminal (*Ctrl + Alt + t*) and execute the following command:

`sudo nano /usr/share/xsessions/kiosk.desktop`

2. This will create the .desktop file and open it in the nano text editor. Next we will enter in the required text. Copy the following text into our new file:

```
[Desktop Entry]
Encoding=UTF-8
Name=Kiosk Mode
Comment=Firefox Kiosk
Exec=/usr/share/xsessions/firefoxKiosk.sh
Type=Application
```

Breaking down what is going on, we are setting up a desktop launch option from the Ubuntu account login screen. 

As for the “Exec” line, we are listing the path to the shell script that will launch our Firefox kiosk. We will create that file in just a moment.

3. Now we will save the .desktop file. Press *Ctrl + o* then *Enter* to save the file and *Ctrl + x* to exit the file once saved.

---

Next, we will create and populate the shell script with the required bash commands that the .desktop file will call in the “Exec” line.

1. Execute the following command to create the shell script file and open it in nano:

`sudo nano /usr/share/xsessions/firefoxKiosk.sh`

2. Enter the following commands in our new script:

```
#!/bin/bash
xset s off
xset s noblank
sleep 10s
while true; do
	firefox -kiosk <URL> & -width XXXX -height XXXX -P 
<profile>;
	sleep 10s;
done
```

Here's a breakdown of what we have set up now.

*xset*: disables the screensaver. 

*firefox*: launches firefox

*-kiosk*: opens firefox in kiosk mode

*<URL> &*: allows us to specify a page to open the browser on

*-width XXXX -height XXXX*: sets the window dimensions for the browser to avoid any blank space around the window. 

*-P <profile_name>*: allows us to specify a profile to use. It is required for setting screen dimensions. 

3. Once again, save and exit the file using the hotkeys above.

---

Now we need to make sure that the shell script has executing permissions.

1. Run the command:

`sudo chmod -x /usr/share/xsessions/firefoxKiosk.sh`

2. Double check the file is executable by:

`sudo ls -la /usr.share/xsessions/firefoxKiosk.sh`

---

Now we are going to create a separate user without admin creds for using the kiosk.

Go to Settings > Users, enter the admin password and create a new user. Later we will need to come back and edit this user.

Now log out. You should see both users listed on the login screen. We need to check and make sure that we now have a new “Kiosk Mode” option for our desktop. To do that, select the new kiosk user. In the bottom right corner, you should see a small gear icon. When you click that, the “Kiosk Mode” option should be populated alongside the others. 

If it is not there, you will need to go back over the kiosk.desktop file to make sure it was created in the right directory with the correct file name and contained text.

---

Now login to the new kiosk account. We will need to get our firefox profile created and configured.

1. Open a terminal and launch Firefox with the following command:

`firefox -P kiosk`

2. This will pop open a box for Firefox allowing us to create a new profile. We need to make sure that the profile name we used in our shell script above matches the one we create.

3. Next we will setup private browsing for our new profile:

```
In Firefox, select the Application Menu > Settings > Privacy and Security

In the History section, select Firefox will: “Use custom settings for history”

Then check “Always use private browsing mode”
```

---

Now everything should be good to go. First, we will test everything to make sure it is working. Logout of the kiosk account. Select the kiosk account from the sign-in screen and select the gear in the bottom right. Select our new “Kiosk Mode” desktop option and enter the password to sign in.

With any luck we should see Firefox launch in full screen to the URL we specified.

If everything launched the way it was supposed to, we are good to finish our configuration by enabling auto-login for the kiosk user.

Before we enable auto-login, make sure the kiosk user has the “Kiosk Mode” selected in the login screen so it boots into the right desktop.

1. Got back to Settings > Users, enter the admin password

2. Select the kiosk user and  toggle Auto-login

Now restart the computer. It should now boot directly into the Firefox browser in full screen.

---

Considerations:

*Browser*

When configured this way, there will be no menu bar, application button, search bar, etc. It will only display the current webpage with no additional access. You will be unable to navigate tabs, open a new window, or close the browser.

There is a bit of a way around this however. The tab hotkeys for Firefox will still work.

*Ctrl + t*: Opens a new tab

*Ctrl + w*: Closes the current tab

*Ctrl +r*: Refreshes the current tab

Using tabs this way though is difficult as you are unable to see what is open where.

*Desktop Environment*

When you use the “Kiosk Mode” desktop, there will be no access to any other apps, icons, toolbars, or the like. It will only open Firefox on boot. That is it. You will not even be able to access the terminal through the regular hotkey. There is however a way around this.

*Ctrl + Alt + F3*: This hotkey will open tty and allow you to enter commands.

You can use it to power down the machine or navigate the system with commands. It will be handy in case you need to do troubleshooting of your script or other configurations, as the device will now always boot up into Firefox only.

*SSH*

I would recommend installing SSH on the machine as well if it will be used in an enterprise environment. It will help with remote troubleshooting issues with the configuration. However, I would only do this if the machine will have no PHI or PII if you are required to be HIPAA compliant.
