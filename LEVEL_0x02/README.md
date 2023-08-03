
# Wireshark, it was a long time ago

We have a **`.pcap`** file here, these files contain packet data of a network and are used to analyze the 
network characteristics. This file extension stands for _**packet capture**_.

If you want to know more about ***packets*** and ***networking*** in general, i strongly recommend that you 
watch the playlist of Youtuber Ben eater on this subject.

We will analyze the content with the Wireshark software https://www.wireshark.org/.

**Wireshark** is a network protocol analyzer used to capture and inspect data traffic on a computer network. 
It helps in analyzing network issues, monitoring data exchanges, and identifying security vulnerabilities by 
providing a detailed view of network packets and their contents.

Let's see how to dissect all this :

![enter image description here](https://i.imgur.com/N9PwFK4.png)

Each line represents a packet, when you click on it, you have a window at the bottom left, it gives all the 
characteristics of the package, visually it looks like this :

![enter image description here](https://media.geeksforgeeks.org/wp-content/uploads/TCPSegmentHeader-1.png)

In the bottom left, you can see the same informations but in a HEX + ASCII format, by clicking on it you can 
see the corresponding parts on the left.

On the No. 43, we can see a suspect information, that indicates maybe a password prompt :

    0000   00 24 1d 0f 00 ad 08 00 27 cc 8a 1e 08 00 45 00   .$......'.....E.
    0010   00 41 d4 b3 40 00 40 06 16 77 3b e9 eb df 3b e9   .A..@.@..w;...;.
    0020   eb da 2f 59 99 4f ba a8 fb 18 9d 18 15 7b 80 18   ../Y.O.......{..
    0030   01 c5 27 9d 00 00 01 01 08 0a 02 c2 3c 62 01 1b   ..'.........<b..
    0040   b9 87 00 0d 0a 50 61 73 73 77 6f 72 64 3a 20      .....Password: 

The following lines are not truly consistent, but we do have an option to rectify this.
Right click on the first packet, ***Follow → TCP Stream***.

https://www.wireshark.org/docs/wsug_html_chunked/ChWorkDisplayPopUpSection.html#PacketDetailsPopupMenuTable
https://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowStreamSection.html

![enter image description here](https://i.imgur.com/KCuMAbT.png)

> Token : `ft_waNDReL0L`

> Flag : `kooda2puivaav1idi4f57q8iq`

