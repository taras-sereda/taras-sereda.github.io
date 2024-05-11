---
layout: page
title: Fixing Kindle scribe pen
description: 
img: assets/img/IMG_0416.jpg
importance: 1
category: fun
published: true
---

### Preamble
I'm a big fan of e-paper devices, my kindle served me for more then 5 years with no objections. After watching a couple of e-notebook reviews on youtube I decided to give Kindle scribe a try.  I live in Ukraine and these uncertain days when country is in the war, traveling abroad is almost impossible for male part of the population, but I knew that my girlfriend is about to visit Canada, so I asked her to bring the scribe for me.

Side note: prices for scribe in Ukraine are unreasonably high.

### Initial Disappointment
First impressions after unpacking, tablet feels solid and well assembled, it reminded me of iPad. 
I was eager to try making some notes with pencil. Even without touching the surface of the tablet pen started to write, and the experience was just horrible. You have no control of the pen and it feels like it's overly sensitive!
I searched the web and found similar objections on [reddit](https://www.reddit.com/r/kindlescribe/comments/16ozabr/pen_writes_when_hovering_with_or_without_nibs/) 
Moreover, amazon [acknowledges this issue](https://www.amazon.com/gp/help/customer/display.html?nodeId=Tr9Q8zt2BBD4b6uynz) and suggests to replace a tip.  I tried replacing the tip, and that didn't help!
Here is an [issue tracker for this problem](https://www.amazonforum.com/s/question/0D56Q0000BiSaI8SQK/kindle-scribe-pen-writing-when-not-touching-the-screen), that has over 80 comments! Some people suggest to knock the pencil. For some it works, for some it doesn't. Folks are just leaving mad comments, and I understand why.

### Electrical Tape and piece of kitchen aluminum foil to the rescue!

Alright, pencil is overly sensitive, this is already informative. Now the question is how to harness its sensitivity? Before I had some experience improving [EMI shielding by means of the aluminum foil](https://xgrtec.com/blog/aluminum-emi-shielding-importance-and-challenges/). So I had a hypothesis, that maybe and IC inside the pencil and magnet or some other component interfere with each other? And this interference amplifies the sensitivity of the pencil?

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/IMG_0412.jpg" title="image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/IMG_0413.jpg" title="image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Here is the solution I came up with. Take a piece of aluminum foil and wrap around the bottom part of the pencil, it's important to find a good placement for the wrapper. If you place it too close to the tip - pen becomes unresponsive at all. If you move it too far - then there is no effect of the shielding. So once you found a correct placement for the foil wrapper, in my case it's 1.5 cm from the tip of the pencil, secure the foil with electrical tape. That's it, this fix is robust enough and now I can fully enjoy taking notes on my a bit-modified and personalized Kindle scribe. I hope the described solution will help you to fix your devices too!

<div class="row justify-content-sm-center">
	<div  style="width: 60%;">
		{% include figure.html path="assets/img/IMG_0416.jpg" title="image" class="img-fluid rounded z-depth-1" %}
	</div>
</div>
