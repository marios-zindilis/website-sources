---
title: tcpdump
Description: Notes and links on tcpdump
first-published: 2013-11-19
last-modified: 2014-05-24
---

Notes and links on `tcpdump`.

## Capture and output at the same time ##

This command will make `tcpdump` save captured packets in a `.pcap` file,
*and* output their printable format on `stdout`:

    sudo tcpdump -n -s0 -U -vvv tcp port 80 -w - | tee capture.pcap | tcpdump -vvv -n -r -

Options:

    -n  : numeric output, don't resolve IPs to names, don't resolve ports to service names
    -s0 : don't truncate packets unless they are bigger than 65535 bytes
    -U  : don't buffer output, dump to `.pcap` immediately
    -w  : write `.pcap`
    -r  : read from `.pcap`

## See Also ##

*   [A tcpdump Primer](http://www.danielmiessler.com/study/tcpdump/)
*   [BPF - the forgotten bytecode](http://blog.cloudflare.com/bpf-the-forgotten-bytecode)
