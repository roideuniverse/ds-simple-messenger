# ds-simple-messenger
Distributed System - Assignment 1
Link : https://docs.google.com/document/d/1JqBqZChFWzbnTXgbYB8Z6l9IRlzt8c0loIwa6ymkPps/pub

PA 1 Specification
CSE 486/586 Distributed Systems

Programming Assignment 1

#Simple Messenger on Android

## Introduction

In this assignment, you will design a simple messenger app on Android. 

The goal of this app is simple: enabling two Android devices to send messages to each other. The purpose of this assignment is to help you understand some of the basic mechanisms necessary to write networked apps on Android.
Please start this assignment as soon as possible. 

There will be a significant learning curve in terms of socket programming as well as Android programming. If you have not done these before, this assignment might quickly become daunting to you. We will cover some of the basics in class; however, it is still necessary for you to learn what is not covered in class yourself. So again, please start this assignment as soon as possible.


As far as we can see, there are three high-level challenges in this assignment:
There will be a lot of reading involved in this assignment. This is because you need to read many tutorials/articles online.
There will be many trials and errors to make Android do what you want to do. This is true not just for Android but also for any platform/framework. Luckily, Android provides excellent official documentation. You will get used to reading the official developer website.


There are networking restrictions that the Android emulator environment imposes. You need to get used to it for the assignment.
So here we go!

### Step 1: Getting Started

Unless you are already familiar with Android programming, the first thing you want to do is to follow the tutorials.


Please follow the first tutorial “Building Your First App” which will guide you through the process of installing necessary software and creating a simple app.


Important Note: There is one part of this tutorial where it instructs you to create an Android emulator instance (or AVD, Android Virtual Device). 

Please follow this to create an x86-based AVD, not an ARM-based AVD. x86-based AVDs run a lot faster with less resources, so it’s going to be easier to run on your laptop. However, it is not a requirement for you to use x86-based AVDs. If your machine doesn’t support VT-x, you can still use ARM-based AVDs.


If you are a Mac user, there is a bug in the original HAXM. Please download and install this “hotfix” instead.
Consequently, we will not use the API level 17 because x86-based AVDs are not supported by it. Instead, please download the API level 16 using Android SDK Manager in Eclipse.


You do not need to do other tutorials at this point unless you like to do it.
Please read the following pages because it will give you necessary background.
Application Fundamentals 
Activities
Processes and Threads
For the success of all the programming assignments, it is critical that you know how to use the Android emulator and debug your app because you will spend lots of time using the emulator and debugging.
Using the Android Emulator
Debugging, especially with Eclipse using the Log class.

### Step 2: Setting up a Testing Environment

You will need to run two AVDs in order to test your app. Unfortunately, Android does not provide a flexible networking environment for AVDs, so there are some hurdles to jump over in order to set up the right environment. 

The following are the instructions. Although the instructions should work on Windows platforms, we strongly suggest you use a UNIX-based platform, e.g., Linux or Mac, because the teaching staff may not use Windows and the support will be limited.


You need to have the Android SDK and python 2.x (not 3.x). If you have not installed these, please do it first and proceed to the next step.


Add <your sdk directory>/tools to your $PATH so you can run Android’s development tools anywhere.
E.g., in bash, enter: export PATH=<your sdk directory>/tools:$PATH
If you use bash, you probably want to add it in your ~/.profile or ~/.bash_profile so you don’t have to repeat it every time.


For Windows platforms, just google and see how you can set PATH ;-)
Add <your sdk directory>/platform-tools to your $PATH so you can run Android’s platform tools anywhere.
E.g., in bash, enter: export PATH=<your sdk directory>/platform-tools:$PATH


Again if you use bash, you probably want to add it in your ~/.profile or ~/.bash_profile so you don’t have to repeat it every time.


Save create_avd.py, run_avd.py, and set_redir.py.


To create AVDs, enter: python create_avd.py 5


The above command should be run only once because you do not need to create AVDs multiple times.
When asked the question “Do you wish to create a custom hardware profile [no]”, just press enter.
5 AVDs should have been created by the above command. The names are avd0, avd1, avd2, avd3, & avd4. You can manipulate them in Eclipse, but please do not edit or delete them because they are necessary for our scripts to work.


If you can’t create x86-based AVDs, please enter: python create_avd.py 5 arm instead; this will create ARM-based AVDs.


For this assignment you need two AVDs, so enter: python run_avd.py 2
The above command will start two AVDs, avd0 & avd1.


In general, to run the AVDs created by create_avd.py, enter: python run_avd.py <number of AVDs>
After all AVDs finish launching, create a network that connects the AVDs by entering: python set_redir.py 10000


The above command will set up port redirections, but there are some restrictions in terms of socket programming. We will detail the restrictions in Step 3 below.

### Step 3: Writing the Messenger App

The actual implementation is writing the messenger app. The requirements are below. You must follow everything below exactly. Otherwise, you will get no point on this assignment.

There should be only one app that you develop and need to install for grading.


When creating your new Android application project in Eclipse, use the following names:

Application Name: SimpleMessenger
Project Name: SimpleMessenger
Package Name: edu.buffalo.cse.cse486586.simplemessenger


API level 16 as the minimum & target SDK.


There should be only one text box on screen where a user of the device can write a text message to the other device.


The other device should be able to display on screen what was received.
And vice versa.


You need to use the Java Socket API.


All communication should be over TCP.


You can assume that the size of a message will never exceed 128 bytes (characters).


As mentioned above, the Android emulator environment is not flexible for networking among multiple AVDs. Although set_redir.py enables networking among multiple AVDs, it is very different from a typical networking setup. When you write your socket code, you have the following restrictions:


In your app, you can open only one server socket that listens on port 10000 regardless of which AVD your app runs on.


The app on avd0 can connect to the listening server socket of the app on avd1 by connecting to <ip>:<port> == 10.0.2.2:11112.


The app on avd1 can connect to the listening server socket of the app on avd0 by connecting to <ip>:<port> == 10.0.2.2:11108.


Your app knows which AVD it is running on via the following code snippet. If portStr is “5554”, then it is avd0. If portStr is “5556”, then it is avd1:
```
TelephonyManager tel =
        (TelephonyManager) this.getSystemService(Context.TELEPHONY_SERVICE);
String portStr = tel.getLine1Number().substring(tel.getLine1Number().length() - 4);
```
This document(http://developer.android.com/tools/devices/emulator.html#emulatornetworking) explains the Android emulator networking environment in more detail.
In general, set_redir.py creates an emulated, port-redirected network like this (VR stands for Virtual Router):

Design Document
