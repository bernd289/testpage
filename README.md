# testpage

this is an alternative to http://test.schrimpe.de/

You can test it here:

https://test.matthiashille.com 
(IPv4 only: https://test4.matthiashille.com IPv6 only: https://test6.matthiashille.com)

https://test.mazdermind.de 
(IPv4 only: https://test4.mazdermind.de IPv6 only: https://test6.mazdermind.de)

Thanks to [MaZderMind](https://github.com/MaZderMind) and [matzex](https://github.com/matzex) for hosting it!


## Setup

1. Activate SSI in Apache: https://httpd.apache.org/docs/current/mod/mod_include.html


2. Activate Hostname Lookup in Apache.

Therefore change the following line from "Off" to "On" in the main apache2 conf, e.g. on Ubuntu

Original:
`HostnameLookups Off`

Change to:
`HostnameLookups On`
