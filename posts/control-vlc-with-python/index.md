---
title: Control VLC with Python
description: One of a few possible ways to control VLC with a Python script
first-published: 2016-10-23
last-modified: 2016-10-23
tags:
- Python
- VLC
exports:
- ellak
- planet-sysadmin
---

[**VLC**](http://www.videolan.org/vlc/) is an amazing media player, or rather a
media-related Swiss army knife. It has a surprising number of different control
[interfaces](https://wiki.videolan.org/Interfaces/), some of which allow 
access over a network, such as the HTTP, Telnet and RC interfaces. It also has
a [Python API](https://www.olivieraubert.net/vlc/python-ctypes/doc/) but just
looking at the documentation is terrifying...

<!-- read more -->

I experimented with controlling VLC from Python using VLC's [Remote Control Interface][RCI], and wrote a small
proof-of-concept code snippet. Here's how to use it:

  [RCI]: https://wiki.videolan.org/documentation:modules/rc/

```python
from vlc import VLC

vlc = VLC()
vlc.add('/path/to/some/file.avi')
vlc.pause()
vlc.play()
vlc.stop()
```

The code is not production-grade. I will not be developing this any more, so I
am putting it here for anyone to grap and hopefully do something useful with. 
I've only tested this on my Ubuntu machine, and it requires both VLC and 
`screen` to be installed. It also uses `subprocess.run()` to start VLC, which
is not available in any version of Python earlier than 3.5.

Here's the library/wrapper, drop it anywhere in your Python `PATH`:

```python
import socket
import subprocess


class VLC:
    def __init__(self):
        self.SCREEN_NAME = 'vlc'
        self.HOST = 'localhost'
        self.PORT = 8888

        cmd = subprocess.run(
            ['screen', '-ls', self.SCREEN_NAME,],
            stdout=subprocess.DEVNULL)
        if cmd.returncode:
            subprocess.run([
                'screen',
                '-dmS',
                self.SCREEN_NAME,
                'vlc',
                '-I',
                'rc',
                '--rc-host',
                '%s:%s' % (self.HOST, self.PORT)
            ])

        self.SOCK = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.SOCK.connect((self.HOST, self.PORT))

    def x(self, cmd):
        '''Prepare a command and send it to VLC'''
        if not cmd.endswith('\n'):
            cmd = cmd + '\n'
        cmd = cmd.encode()
        self.SOCK.sendall(cmd)

    def pause(self):
        self.x('pause')

    def play(self):
        self.x('play')

    def stop(self):
        self.x('stop')

    def prev(self):
        self.x('prev')

    def next(self):
        self.x('next')

    def add(self, path):
        self.x('add %s'  % (path,))

    def enqueue(self, path):
        self.x('enqueue %s' % (path,))

    def clear(self):
        self.x('clear')

    def shutdown(self):
        self.x('shutdown')
```

Good luck using this for your scripts :)

