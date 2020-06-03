---
layout: post
title:  "How to create a virtual serial port"
date:   2020-06-03 10:17:00 +0310
categories: C++20 Coroutines
---
## Table of content

- [Table of content](#table-of-content)
- [Introduction](#introduction)
- [What is `socat`](#what-is-socat)
- [Create Virtual Serial Port](#create-virtual-serial-port)

## Introduction

Hi everybody:)

In this short post, I’m going to show you how we can create a virtual serial 
port and feed data to it. It can be used in debugging and testing of your 
software which communicates with serial port devices. You may not have any 
serial port devices or serial port on your computer at the time of software 
development and it is very essential if you can create a virtual serial port 
and feed mock data to it and test your software.
We’re going to use `socat` application to create a virtual serial port.

## What is `socat`

Based on it’s documentation, Socat is a command line based utility that 
establishes two bidirectional byte streams and transfers data between them.
Because the streams can be constructed from a large set of different types of 
data sinks and sources (see address types), and because lots of address options 
may be applied to the streams, socat can be used for many different purposes.
I don’t wanna bore you with all the details, we’re going to create a virtual 
serial port and feed data to it like this:

![VirtualSerialPort](/assets/images/image8.png "VirtualSerialPort")

As you see in this figure, the virtual serial port has two connectors. 
Our software can connect to one of them. And also we can implement another 
test application which can connect to the second connector. Virtual serial port 
transfers data between these two connectors, so the test application can feed 
mock data and also reads data. In this way we can test our software.

## Create Virtual Serial Port

First of all install socat through running following command in the terminal:

```sh
sudo apt install socat 
```

In the previous section we’ve said virtual serial port has two connectors, this
two connectors must bind to two files. So create two files as follows:

```sh
touch interface_1
touch interface_2
```

and finally we can create virtual serial port by running the following command:

```sh
socat pty,link=interface_1,raw,echo=0 pty,link=interface_2,raw,echo=0
```

These two files behave like serial file port file descriptors for example: 
`/dev/ttyUSB0`

And you can test virtual serial ports as follows.
Open two terminals and run following commands:

first terminal:

```sh
cat interface_1
```

second terminal:

```sh
echo "this is test data" >> interface_2
```

And you see the text which you’ve written in the interface_1 appears in the 
first terminal. 
Congratulations you did it:) 
Now your software can get the interface_1 file as a serial port file descriptor 
and you can feed data to the virtual serial port through writing data to 
interface_2 and also you can read the responses from interface_2.   
