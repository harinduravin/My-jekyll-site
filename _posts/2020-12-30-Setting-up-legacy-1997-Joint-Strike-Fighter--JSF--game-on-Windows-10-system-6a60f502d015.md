---
layout: post
type: other
image: https://cdn-images-1.medium.com/max/800/0*4hRbzd_aLxHymj3K.jpg
comments: true
title: Setting up legacy 1997 Joint Strike Fighter (JSF) game on Windows 10 system
description: >-
  Joint Strike Fighter is a 1997 combat flight simulator designed by Innerloop Studios and published by Eidos Interactive. Innerloop produced the game chiefly to be a showcase for their cutting edge game engine, which they planned to license to other game developers.
date: '2020-12-30T08:25:55.550Z'
categories: []
keywords: []
slug: >-
  /@harinduravin/setting-up-legacy-1997-joint-strike-fighter-jsf-game-on-windows-10-system-6a60f502d015
---

![](https://cdn-images-1.medium.com/max/800/0*4hRbzd_aLxHymj3K.jpg)

**_Joint Strike Fighter_** is a 1997 combat flight simulator designed by Innerloop Studios and published by Eidos Interactive. Innerloop produced the game chiefly to be a showcase for their cutting edge game engine, which they planned to license to other game developers. Innerloop Studios was the creator of famous combat game series called IGI (I’m Going In). Here is how I setup this old game on windows 10 computer after facing a lot of issues.

First I downloaded the game through this link. There is a mountable disk image of size 604MB. Additionally, there is a 160 page manual for the game play.

{% linkpreview "https://www.myabandonware.com/game/jsf-dh2#download" %}

After downloading the 604MB file, there will be two files available in the download folder. Double click on the jsfv10.mds file.

![](https://cdn-images-1.medium.com/max/800/1*04VS8EjQd_GL3Z8tVDqQOQ.jpeg)

Make sure to keep this mounted as a drive, whenever running the game. This has been done for the purpose of copyright protection.

![](https://cdn-images-1.medium.com/max/800/1*OkSQRt73XCAjLHgj0PAPBw.jpeg)

Execute launch.exe.

![](https://cdn-images-1.medium.com/max/800/1*-S2cre8HAVLPGcpKeZhFhA.jpeg)

For the first time, ‘Run Joint Strike Fighter’ button will be ‘Install Joint Strike Fighter’. Go through simple installation workflow to get the JSF game installed in the computer. The total installation size will not exceed 150MBs.

In the next step, when you try to Run Joint Strike Fighter, Windows 10 will require you to download and install _Direct Play_. Direct play is very important for the execution of the game. If your computer has not enabled Direct Play, try to enable Direct Play in the windows system. This can be easily found through google search.

> After installing Direct Play make sure to Restart the computer to get the exe file working. Otherwise 0Xc0000022 error will pop up.

![](https://cdn-images-1.medium.com/max/800/1*NRY3csUM9Hb1VEOtPWrtXw.jpeg)

When you click Install Microsoft DirectX5.0 button, something like this will pop up. This installation might not work properly, but for the purpose of running the game, having half of the stuff is enough. The above is a screenshot I took after getting JSF working on my computer. It seems installing Direct Play is the most important part here.

The game will not run on Windows 10 machine even after following above steps for the following interesting reason.

This game was released in 1997. This means this game was intended to run in Windows 95 computers. If you look carefully, there is another button ‘Install 3Dfx Drivers’. This button will not work in Windows 10 anyway. I tried going through the process in Windows 98 in virtual box.

This is what I got, when I clicked ‘Install 3Dfx Drivers’,

![](https://cdn-images-1.medium.com/max/800/1*ktlwR62wGwsruE9XqoOgTQ.jpeg)

From this one can see that these are very old drivers. Apparently installation failed because there is no reason for my computer to have a very old graphics card from 1990s. Running the game in Windows 98 virtual box is not going to work. In Windows 10 also 3D graphic rendering part needs to be handled.

> **3dfx Interactive** was a company headquartered in San Jose, California, founded in 1994, that specialized in the manufacturing of 3D graphics processing units, and later, graphics cards. It was a pioneer in the field from the late 1990s until 2000.

> The company’s original product was the Voodoo Graphics, an add-in card that implemented hardware acceleration of 3D graphics. The hardware accelerated only 3D rendering, relying on the PC’s current video card for 2D support. Despite this limitation, the Voodoo Graphics product and its follow-up, Voodoo2, were popular. It became standard for 3D games to offer support for the company’s Glide API.

In order to get this part done, install nGlide 2.0 from the below site.

{% linkpreview "https://www.zeus-software.com/downloads/nglide" %}

After downloading go through normal installation steps. Then go into this file path and find nglide\_config.exe file.

> C:\\Windows\\SysWOW64

Create a shortcut of this file in the desktop. Add following settings in the nGlide\_config and Apply.

![](https://cdn-images-1.medium.com/max/800/1*glsKPX-eI_tbkFRk5J5_rg.jpeg)

The next step is setting up Reshade on the JSF folder in the computer. This part is optional because, the game worked fine without Reshade in my computer. For this download Reshade through this link.

{% linkpreview "https://reshade.me" %}

Open the Reshade setup and select the game file as,

C:\\Program Files (x86)\\Eidos Interactive\\Joint Strike Fighter\\jsf.exe

Select Direct3D 9 and go through below installation. After going through this step make sure there are additional files added by Reshade in the JSF folder.

![](https://cdn-images-1.medium.com/max/800/1*FgYfA69_jtSbqatpYq3B8w.png)
![](https://cdn-images-1.medium.com/max/800/1*UWqxCB8UTPqRhO_iBlg4cA.png)

Finally, you are ready to run Joint Strike Fighter in your Windows 10 computer. Before running jsf.exe. Right click and go to properties. Select Reduced color mode in the settings.

![](https://cdn-images-1.medium.com/max/800/1*klggKUTcxHaD-SkqMt1QTA.png)


Here is a screenshot of the game I obtained after successfully running the game in my Windows 10 system.

![](https://cdn-images-1.medium.com/max/800/1*dEEIkyvLD-vfvhoqd0i7UQ.jpeg)

Good luck setting up!