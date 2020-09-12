# Keeping time on the Tworoads networks

I have always been facinated with internet time keeping practices, and ever since having a home lab, I used my own time servers to organise time keeping on our local network, mostly using ntpd, upgrading to the bleeding edge when necessary.

In 2018, I got rid of my very power hungry home lab servers, and moved in the direction of Raspberry Pi, devices to manage some generic services, and other mini computers running home automation systems. The first hurdle i came across was that many small computers do not have a Real Time Clock (RTC). Simply put, when they first boot, they dont know what time it is.  There are some tricks that can be used to make a general guess at time and date, but they are dependent on the internet connection for getting actual time.

## Real time clock ds1307 for thr Raspberry Pi 3B+

### Installation

I wanted a chip that was incorporated into a “hat”, but could still fit within a pi case, and duplicated unused pins, and after some searching, came up with the
[Raspberry Pi RTC Module Real Time Clock Module DS1307](https://www.amazon.com/gp/product/B00ZOXWHK4/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B00ZOXWHK4&linkCode=as2&tag=tworoads09-20&linkId=39cfabfe3e228ce20e37646f920a3166)

It’s very easy to install on the pi, just install the coin battery, and connect to the pin 1 end of the attached header.

### Configuration