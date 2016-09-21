---
layout: post
title: Log iOS method arguments with Frida
category: Security
tags: iosse
---

### What is Frida?

[Frida](http://www.frida.re/) is a reverse engineering tool that can be used for dynamic code instrumentation. Frida works by injecting a JavaScript engine and console into a live process. With Frida, it’s possible to dynamically expose and change behavior in memory, functions, and the [recently added Objective-C runtime](http://www.frida.re/docs/javascript-api/#objc) using JavaScript. Frida is available for debugging jailbroken iOS devices, along with native Linux, Windows, Mac, and Android processes.
This is a short guide to hook an Objective-C method with Frida, logging method arguments to the console.

### Frida setup

The first step is to follow [Frida’s iOS tutorial](http://www.frida.re/docs/ios/) to ensure that Frida can communicate with your iOS device. The major steps are to:
1. Install Frida on your computer
2. Install Frida on your jailbroken iOS device through Cydia
3. Test that Frida can communicate with your iOS device over USB

```bash
$ frida-ps -U
PID Name
--- ----------------
597 Audible
480 Camera632 Downcast
563 LINE133 Mail
601 Messenger
666 Settings...
```

### Identify Objective-C method

For this example I’m injecting Frida into an app called [LINE](http://line.me/en/), an instant-messaging app from Japan. I used the [Hopper](http://www.hopperapp.com/) dissasembler to find a method in LINE I was interested in examining,
[MessageViewController sendMessageWithText:]

![Hopper disassembly](http://upload-images.jianshu.io/upload_images/22730-d7fc2e6775a45481.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Use Frida to identify the underlying function in-memory:

![indetify LIne Application](/public/img/frida-Line.png)

### Write Interceptor script to attach to method
Create a file called interceptSendMessage.js to hold the method-injecting data:

```bash
    var sendMessage = ObjC.classes.MessageViewController["- sendMessageWithText:"];

    Interceptor.attach(sendMessage.implementation, {
        onEnter: function(args) {
        // args[0] is self
        // args[1] is selector (SEL "sendMessageWithText:")
        // args[2] holds the first function argument, an NSString
        var message = ObjC.Object(args[2]);
        console.log("\n[MessageViewController sendMessageWithText:@\"" + message.toString() + "\"]");}
    });
```



### Run the script
To tell Frida to connect to the LINE app on an iOS device over USB and run the script ininterceptSendMessage.js
, run this command:

```bash
    $ frida -U -l interceptSendMessage.js LINE
```

After Frida has attached to the process, try sending a message:

![resutl of run script](/public/img/frida-message-demonstration.png)

### Additional Information__Modifying arguments in Interceptor

It’s easy to dynamically modify arguments with Frida too. Here’s an example script that will append the string: " :)"
 to the first method argument to MessageViewController’s sendMessageWithText:

    var sendMessage = ObjC.classes.MessageViewController["- sendMessageWithText:"];

    Interceptor.attach(sendMessage.implementation, {    
      onEnter: function(args) {
      var message = ObjC.Object(args[2]);
      var modifiedMessage = message["- stringByAppendingString:"](" :)");
      args[2] = modifiedMessage;}
    });

origin: (http://www.mopsled.com/2015/log-ios-method-arguments-with-frida/)
