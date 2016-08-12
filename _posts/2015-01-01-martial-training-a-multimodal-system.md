---
id: 70
title: 'Martial Training &#8211; A multimodal system'
date: 2015-01-01T09:34:16+00:00
author: newbiettn
layout: post
---
Martial Training is a project I worked with 2 other guys &#8211; 1 Vietnamese and 1 Brazilian. The fundemental idea is to support precise training in martial art by using **Kinect** for action recognition, voice control for both commands and feedbacks, logging and vibration for notification of incorrect movements.

{% include image.html url="/images/Martial-Training-Architecture.png" description="The system architecture" %}

In the application, the template-based algorithm **Dynamic Time Warping** is used to compare actions, which can be recorded and stored in matrix vectors by using Kinect. During the implementation, DTW exposed contrainsts, including high time complexity O(N^2) and not being able to measure similarity parts of the body. Therefore, the application is limited in the sense that it can only notify the user if he performs the actions incorrectly but can not explicitely lets him know which part has problem.

Besides notified by the sound, the system should be able to notify the user via vibration. To handle the vibration functionality, I built a tiny Android app which can connect to the main application, which was developed in C#, by using **Socket**. However, the socket is not persistent in the sense that after a succesful response, the connection will be close just like Http 1.0. Unfortunately, I was not be able to figure out the problem at the moment and spent 2 hours to investigate my code. Later I realized the code logic was wrong at all. I exactly needed a full-duplex connection similar to WebSocket. So, lesson learned.

Regarding to the UI, I had to spent a couple of days in order to read the books **WP4 Unleased**. I had to admit that learning WPF requires a steep curve, which means that it appears to be difficult at the beginning but the deeper I digged, the easier it became. After having some experiences, I began to upgrade it with **Metro** style of Windows 8. I used the open source project **MahApps. Metro** to do that, then added logging display and other things to the UI.

{% include image.html url="/images/Overal-UI.png" description="Martial Training UI" %}

As you can see, it is me in the picture for action tutorial and the left screen is for the user which captured by Kinect. The application is very limited but if you asked me how would I upgrade the application, I think I will augment the video by adding realtime information in the video to inform the user and of course switch the algorithm to machine-learning one.
