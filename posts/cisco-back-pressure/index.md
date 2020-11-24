---
title: "Back-Pressure on Cisco Switches"
Description: The back-pressure feature on Cisco switches
first-published: 2013-11-08
Last Updated: 2013-12-29
Author: Marios Zindilis
---

The `back-pressure` feature on Cisco switches is a workaround to the 
inability to apply proper flow control on half-duplex links. It causes 
the Cisco switch to send fake packets on the link, thus controlling the 
transmission from the remote side, since only one side can send at a 
time on a half-duplex link.
