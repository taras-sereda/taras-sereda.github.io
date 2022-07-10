---
layout: post
title: i3 for multilinguals
---

When I heard about  i3 window manager for Manjaro ArchLinux, I thought, I want to try it and use it as an alternative to macOS, which I like a lot!

*Disclaimer:* using i3 is an adventure. You need to configure many things. The learning curve is steep, though along the way you are building an understanding that you can do whatever you can imagine with Linux and i3 as a window manager.

In this post I want to share my experience on how to add second input language. In my case I was interested in adding Ukrainian.

1. **Adding a new language.** 

   i3 customization can be done via editing `~/.i3/config` . My native language is Ukrainian, so to add it as one more language for input the following line should be added to i3 config. 
   
   ```
   exec --no-startup-id "setxkbmap -model pc105 -layout us,ua -option grp:alt_shift_toggle"
   ```
   
   After rereading config,  `alt+shift` will toggle between US and UA layouts. Basically that's it! though there is more to say on this topic...

2. **Handling unexpected issue.**

   Now when you have more than one language to choose from, another problem sooner or later will arise. :) By default, your computer will be locked in 10 min, if it's in an idle state.  This can be viewed and configured in the same  `~/.i3/config`.

   ```
   exec --no-startup-id xautolock -time 10 -locker blurlock
   ```
   When your screen is locked, unfortunately, there is no way to change the keyboard layout. It was a disappointment for me, luckily it can be fixed. As stated above `xautolock` will use `blurlock` as a locker.

   ```bash
   [taras@archibald ~]$ cat `which blurlock`
   #!/bin/bash
   set -eu
   
   RESOLUTION=$(xrandr -q|sed -n 's/.*current[ ]\([0-9]*\) x \([0-9]*\),.*/\1x\2/p')
   
   # lock the screen
   import -silent -window root jpeg:- | convert - -scale 20% -blur 0x2.5 -resize 500% RGB:- | \
       i3lock --raw $RESOLUTION:rgb -i /dev/stdin -e $@
   
   # sleep 1 adds a small delay to prevent possible race conditions with suspend
   sleep 1
   
   exit 0
   ```

   Since `blurlock` is essentially a bash script nothing prevents us from modifying it a little bit. I found a handy package `xkb-swith`  that allows to set and query layouts. So before locking the screen, we should check current layout, and if it's not US we forcefully set it to US. This will guarantee that we'll be on the correct layout on a locked screen.  That's it!
   ```bash
   # set US layout if it's not US
   current_layout=$(xkb-switch)
   if [[ $current_layout != 'us' ]]
   then
       xkb-switch -s us
   fi
   ```

3. **Adding language indicator to i3bar.** 

   By default, `i3bar` displays useful data about your system, like CPU load, amount of free space, battery charge level, etc.  This data is supplied by `i3status` . But it doesn't show a language indicator. So to added it to the `i3bar` I used same `xkb-swith` and wrote a custom wrapper over `i3status` as suggested in its man page.

   ```bash
   [taras@archibald ~]$ cat ~/scripts/my_i3status.sh 
   #!/bin/sh
   # shell script to prepend i3status with more stuff
   
   i3status | while :
   do
       read line
       current_layout=$(xkb-switch)
       echo "$current_layout | $line" || exit 1
   done
   ```

   Lastly it's necessary to modify `~/.i3/config`  to pipe output of `my_i3status.sh` to the `i3bar`
   ```
   # Start i3bar to display a workspace bar (plus the system information i3status if available)
   bar {
   	i3bar_command i3bar
   	status_command ~/scripts/my_i3status.sh
   	position bottom
   	...
   ```

   

   After re reading config you should have language indicator displayed on your bar! :)

   ![rere](/assets/img/1657470528.png)

That's all! Now switching languages doesn't feel awkward.

