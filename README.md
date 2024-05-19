## 👩🏻‍💻 VoIP Lab

This lab is part of the course Networks & Communications 2 of Universidad San Jorge of Zaragoza. All contents are original.

### 🧐 Intro

The objective of this lab is to make VoIP calls on a local network. The call will occur between a physical VoIP phone and a virtual SIP client running on a laptop. We will analyze the traces of these calls with Wireshark to see the details of the SIP and RTP protocols.

### 📦 Delivery

The deliverable for this lab will be a PDF document answering the questions indicated at the end of this document. Only one document per group should be submitted through the designated PDU submission. It is recommended to answer in class, as the questions are brief, simple, and directly related to what happens on the local network.

### 🗺️ Steps to Follow

1. **Physical Connection.** Connect the VoIP phone and the computers where the SIP client will run using the switch. This will require connecting the elements with Ethernet cables, as well as powering the switch and the phone. Phones are connected using the “LAN” port.
2. **Local Network Configuration.** Both the phone and the computers will be part of a simple local area network. Since there is no router, DHCP service, DNS, or any of the services usually available on a conventional network, we will have to configure some fields manually.
    1. On the computers we will:
        1. Temporarily disable the firewall (remember to enable it again afterward).
        2. Set a private IP address for our computer on the network we are configuring, of the type 192.168.X.Y. The address is arbitrary but must be used consistently.
        3. Set the subnet mask on the network we are configuring, in the format 255.255.W.Z. Again, it is arbitrary but must be configured consistently.
        4. If you have problems, try disabling other networks, such as WiFi.
    2. On the VoIP phone we will:
        1. From the menu (circular button), go to System > Network > Internet protocols and select IPv4 only.
        2. Press Back and navigate to IPv4 Settings and select Static IP.
        3. Press Back and navigate to Static IP Settings > Static IP. There we will set an IP address in the same range as the previous step, but of course different.
        4. Press Back and navigate to Netmask, where we will set the same subnet mask as in the previous step.
        5. Press Back and when asked, restart the device.
    3. Finally, we must check network visibility. To do this, from our computers we will ping the IP address we just assigned to the phone, which should respond, as well as to the other computers on the network.
3. **Configure the SIP Account on the phone.** To do this, we will access the phone’s web configuration panel by entering the IP address we assigned in the previous step from a browser. The credentials are admin/admin.
    1. From there, we will go to Accounts > Account 1 > General settings. There, ensure that “Account Active” is set to “Yes” and enter an “Account Name,” for example, “gs.” All other fields should be blank. Click “Save and apply.”
    2. Now go to “SIP settings” > “Basic settings.” Select “No” for “SIP Registration” and ensure that the SIP protocol uses port `5060` and UDP.
    3. Take a look at the other options offered by the phone, but do not change anything.
4. **Configure the Virtual SIP Client.** To do this, on each of the computers:
    1. Download the appropriate version of (linphone)[https://www.linphone.org] for our system.
    2. Once installed, open the program and select to create a SIP account with the default parameters. Once created, click the gear at the bottom left and click “Preferences.”
    3. In the “Network” section, make the following adjustments:
        1. Disable the IPv6 option.
        2. Enable “SIP UDP Port” and select `5060`.
        3. Enable “RTP UDP Audio Port” and select `25123`.
    4. In the “SIP Accounts” > “Proxy Accounts” section, click “Add account” and create one with the following parameters:
        1. In “SIP Address” enter: `"<our-name>" sip:<our-name>@<local-network-ip-address>`
        2. In “SIP Server Address” enter: `sip:<local-network-ip-address>;transport=udp`
        3. In “Registration Duration”: `3600`
        4. In “Transport” select UDP.
        5. All other fields should be empty and/or disabled.
        6. Save the account and select it in the dropdown at the top right.
5. **Test the Call.** If everything has gone well, we will be able to call between the phone and the computers.
    1. To call from linphone, type in the top bar `sip:<name>@<ip-address>`.
    2. To call from the phone, access the menu and select “Direct IP call.” Type the IP address you want to call and press “OK.”
    3. Try calling in both directions and transferring calls.
6. **Trace Analysis.** To analyze the traces during the calls, open Wireshark on a computer and capture the traffic on the Ethernet network interface. Try filtering by `sip`, `rtp`, or the IP addresses of the other computers and/or phone with `ip.src=...`.
7. When finished, reset the phone to factory settings. To do this, go to “System” > “Factory reset.”

### ❓Questions to Answer

- Which are the protocols in use? What are they used for?
- What command is sent to establish a conversation?
- Where can we find the name of the caller?
- What happens if the receiver does not pick up the phone?
- What happens if the receiver rejects the call?
- What command is sent for closing a connection?
- Can the second PC see the traffic generated by the conversation?

#### 🎬 Credits
The setup would not have been possible without the help of Ariel Naval Alcalá ([@aeri](https://github.com/aeri)).
