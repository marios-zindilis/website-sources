---
title: Mutagen ID3 Python Library
Description: Notes and links on using Mutagen for accessing ID3 tags in Python
first-published: 2015-12-12
---

**Mutagen** is a Python module to handle audio metadata.

UFID: Unique File Identifier
----------------------------

The unique file identifier is a reference to an external database that may 
contain more information about the work tagged with it. Like many ID3 tags, it 
can have multiple values. For example, you can link the song "*Black Sabbath*" 
by "Black Sabbath" as `mt0030286828` in [AllMusic][], and as 
`153a3596-af03-36ed-976a-83fec8745732` in [MusicBrainz][]. 

  [AllMusic]: http://www.allmusic.com/ "AllMusic"
  [MusicBrainz]: https://musicbrainz.org/ "MusicBrainz"

Note that according to the [definition of the UFID ID3 frame][UFID], the URLs 
should not be queries themselves. I guess that it might be a feature of the
software that reads this tag to transform it to direct links. In the example
used, those links would be: 
<http://www.allmusic.com/song/black-sabbath-mt0030286828> and 
<https://musicbrainz.org/work/153a3596-af03-36ed-976a-83fec8745732>.

  [UFID]: http://id3.org/id3v2.4.0-frames

Let's see how to write UFID and read it back in Python, with Mutagen.

{% highlight python %}
>>> from mutagen import id3
>>> song = id3.ID3('test.mp3')
>>> # Note that there is no `UFID` tag in the beginning:
>>> song.keys()
dict_keys(['TPE1', 'TALB', 'APIC:', 'TDRC', 'TRCK', 'TPE2', 'TPOS', 'TIT2'])
>>> # Let's add two references:
>>> am_url = 'http://www.allmusic.com/'
>>> am_ref = 'mt0030286828'
>>> mb_url = 'https://musicbrainz.org/'
>>> mb_ref = '153a3596-af03-36ed-976a-83fec8745732'
>>> song.add(id3.UFID(owner=am_url, data=am_ref.encode()))
>>> song.add(id3.UFID(owner=mb_url, data=mb_ref.encode()))
>>> # There are now two UFIDs:
>>> song.keys()
dict_keys(['UFID:https://musicbrainz.org/', 'TRCK', 'TPE1', 'UFID:http://www.allmusic.com/', 'TPE2', 'TALB', 'TIT2', 'APIC:', 'TPOS', 'TDRC'])
>>> # Read them back:
>>> song['UFID:https://musicbrainz.org/']
UFID(owner='https://musicbrainz.org/', data=b'153a3596-af03-36ed-976a-83fec8745732')
>>> song['UFID:https://musicbrainz.org/'].owner
'https://musicbrainz.org/'
>>> song['UFID:https://musicbrainz.org/'].data
b'153a3596-af03-36ed-976a-83fec8745732'
>>> song['UFID:https://musicbrainz.org/'].data.decode()
'153a3596-af03-36ed-976a-83fec8745732'
{% endhighlight %}

Note the use of `encode()` and `decode()` methods of `unicode` used for the 
`data` part of the frame, as it expects its contents to be bytes.
