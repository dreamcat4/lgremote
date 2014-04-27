# Lgremote cmdline program

A command line program to remotely control 2011 LG "Smart" TVs.

Supported models:

    Any 2011 LG TVs labelled "Smart TV"*

    For example:
    LV5500,LW5500,LW6500,LW7700,LW9800
    LV550x,LW550x,LW650x,LW770x,LW980x
    LV550T,LW550T,LW650T,LW770T,LW980T

This program is strictly only meant for LG "Smart TV" devices and not compatible with any other LG products (blue-ray players, phones, etc).*

This script may no longer work with newer "LG SMART TV" models released after 2011.

## Networking Requirements

Be sure that:

    * Both computer + TV are on the same LAN segment.
    * uPNP is enabled on the local router.
    * You have assigned a FIXED ip address to your LG Smart TV.

## Platform Requirements

Platform support: Mac OS X - good, Linux - ok, Windows - poor.

This is a Ruby command line program. So it requires Ruby, and several rubygems installed in order to operate. The Ruby interpreter needs to have been installed with READLINE support for the small part of the program which handles interactive keyboard input.

The Gem dependencies are listed in Gemfile. Some gem dependencies will fail to compile without their required C libraries. Apple Macs already have everything installed and this script was tested exclusively on Mac OS X. If you DO have an Apple Mac, skip this section of the README. If you have some other operating system, then the remaining hints here may form essential reading. However your mileage may vary considerably.

Curl (`libcurl`) is required for the patron gem, which does all the HTTP portion of communications. On Linux you may need to install the optional libcurl-dev package to get the patron gem extensions to compile. For Windows, unfortunately Patron and Curl are pretty difficult to install. See [here](https://github.com/toland/patron/issues/2) for ideas. If all else fails, consider running this script in a VmWare virtual machine (linux guest, network bridge mode).

Another simple approach to get Windows working is to replace the patron API with your own Windows-compatible functions and use a different underlying mechanism for sending the actual HTTP get and HTTP post requests. There are only a couple functions to implement.

TV auto-discovery is provided by the `dnssd` gem (multicast DNS). It requires `dns_sd.h`, which can be provided by the Bonjour library (Apple's Bonjour for Windows), or Avahi (Linux). Mac OS X already comes with the necessary `dns_sd.h` API and runtimes (aka `mdnsResponder` daemon).

Again, here Windows is problematic and does not meet the requirements easily. After installing Apple's Bonjour for Windows, you apparently need to go away and find the location of the `dns_sd` runtime library. There are some discussion elsewhere on the net about this. Then somehow, point it to the 'dns_sd' gem extensions so that they can find the Bonjour library + headers, and link against it. I have no idea how you are going to do that.

A simpler solution might be to just disable and get rid of this portion of the program entirely. You must get in there and remove (comment out) the line that says `require dnssd`. Then proceed to remove all those functions which perform the TV auto-discovery command(s). That of course also means that you can't auto-discover the TV during pairing in the usual wayanymore. Therefore (as part of the same pair command) an alternative method of pairing has also been provided where you can just manually input the TV's IP address.

## Installation

Bundler can try to install all of the necessary gem dependencies for you.

    $ gem install bundler
    $ cd lgremote
    $ bundle install

If you have bundler 1.1 onwards, you can stage all the dependencies directly within the lgremote folder.

    $ bundle install --standalone

Finally, add the directory containing the `lgremote` program to your `$PATH`.

    $ echo "export PATH=$PATH:$PWD" >> ~/.profile

## Usage

    Interactive pairing
       lgremote pair

    Display pairing key
       lgremote pair 192.168.1.2

    Enter pairing key
       lgremote pair 192.168.1.2 AAABBB

    Show all buttons
       lgremote press

    Show all buttons in group "Menus"
       lgremote press menus

    Press button
       lgremote press volume_up
       lgremote press volume_down

    Move mouse by 1 increment
       lgremote mouse up
       lgremote mouse down
       lgremote mouse left
       lgremote mouse right

    Move mouse by +- {x,y} pixels
       lgremote mouse -25 0

    Interactive text entry (tab updates)
       lgremote keyboard

    Non-interactive text entry
       lgremote keyboard text_string

## Copyright

lgremote is provided Copyright © 2011 under MIT License.


