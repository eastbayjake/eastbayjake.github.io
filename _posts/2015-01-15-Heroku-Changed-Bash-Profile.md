---
layout: post
title: Heroku Changed My .bash_profile
date: January 15, 2015
intro: I recently installed the Heroku toolbelt and was surprised to see my bash settings changed the next time I opened a terminal window. Three hash marks had been added to my command line prompt without permission!
---

I recently installed the Heroku toolbelt and was surprised to see my bash settings changed the next time I opened a terminal window. Three hash marks had been added to my command line prompt without permission! (I'm guessing because Heroku changes PATH in .bash_profile they just added a comment to the line before it, without considering users who have customized their settings and already have something defined on that line.)

But it's not a big deal. Here's how I fixed it:

(1) Open a terminal window
(2) Enter `cd` to go to your home directory
(3) Use your editor of choice to open .bash_profile. If you like vim, the command is `vi .bash_profile`

![]({{ site.url }}/images/heroku-bash-hash-before.png)

(4) Remove the offending "### Added by Heroku Toolbelt" comment. (You may find these [vim commands](http://fprintf.net/vimCheatSheet.html) useful.)
(5) Save and exit. The vi command is `ZZ` (Shift+Z Shift+Z).
(6) Close and reopen your terminal window.

If you `cd` into your home directory and enter `cat .bash_profile` you should see your restored bash settings

![]({{ site.url }}/images/heroku-bash-hash-after.png)

In the examples above you'll see some of my customized bash settings that probably differ from yours. If you'd like more information about customizing your terminal command prompts, there are [several](http://code.tutsplus.com/tutorials/how-to-customize-the-command-prompt--net-20586) [tutorials](http://blog.taylormcgann.com/2012/06/13/customize-your-shell-command-prompt/) [online](http://blog.twistedcode.org/2008/03/customizing-your-bash-prompt.html).