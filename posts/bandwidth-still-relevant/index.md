---
title: Is bandwidth still relevant for web hosting?
first-published: 2014-02-28
---

The [Numix Project](http://numixproject.org/) received some web love 
lately, mentioned in last Sunday's [Linux Action Show](http://www.jupiterbroadcasting.com/52182/numix-your-linux-las-s31e01/) 
web episode, and later on [It's F.O.S.S](http://itsfoss.com/numix-linux-distribution/) 
and probably elsewhere, for their announcement to release their own 
Linux distribution. As a result, their website is now down, with the 
server returning 500:

```bash
mariosz@super-mario:~$ curl -I numixproject.org
HTTP/1.0 500 Internal Server Error
```

...and displaying the message "Bandwidth Limit Exceeded". I would have 
guessed that with everything-as-a-service and the whole Cloud <abbr title='shebang'>#!</abbr>, 
bandwidth consumtion would have already become irrelevant. 

I mean, there's enough BW to feed our amplification attacks (
[400Gbps on CloudFlare](http://blog.cloudflare.com/technical-details-behind-a-400gbps-ntp-amplification-ddos-attack), 
[350Gbps on OVH](https://twitter.com/olesovhcom/statuses/433631778620702721)), 
but not enough to host our websites? Does that not call for a revision 
of the model?
