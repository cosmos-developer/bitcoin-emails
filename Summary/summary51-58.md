# Summary 51 -\> 58  
  
### Context:  
* Email \#52:   
I couldn't get connected using the Tor SOCKS proxy. That might be  
because of the Freenode Tor policy which requires connecting to their  
hidden service: http://freenode.net/irc_servers.shtml#tor.
* Email \#53:  
The message discusses several issues related to connecting to Freenode's IRC server via TOR, as well as the challenges faced in establishing connections to Bitcoin nodes
Freenode's IRC connection via TOR: Freenode's hidden service bans TOR due to abuse, making it challenging to connect. The idea of sharing one account among multiple users is considered, but it's uncertain if it would work due to potential limitations or administrative issues
Proxy Test for Bitcoin Node Connection: Testing proxy connection for Bitcoin nodes involved difficulties in finding online nodes without IRC. It takes time to establish connections without known node addresses. The success of outbound TOR connection needs to be confirmed by checking the debug log for "connected" entries
Finding alternative IRC servers: suggests to inquire on forums or mailing lists for information
TOR users can't accept incoming connections: emphasizing the importance of setting up port forwarding on routers is suggested.
* Email \#56:   
Enabling the proxy setting and restarting Bitcoin I got the first  
connections in less than a minute and ultimately even 8 connections. I  
wonder if they're all really through TOR. Netstat shows only 2  
connections to localhost:9050 and 7 connections from local port 8333  
to elsewhere. (Some of the shown connections may be already  
disconnected ones.) For some reason there's no debug.log in the folder  
where I'm running it.
The wiki page sounds like a good and quickly applicable solution. I  
could keep my ip updated there and we could ask others to do the same.  
When the Linux build works, it's easier to set up nodes on servers  
that are online most of the time and have a static IP. A static ip  
list shipped with Bitcoin and a peer exchange protocol would be cool.  
That way there'd be no need for an IRC server.
* Email \#57:  
I merged the linux changes into the main trunk on SVN.  It compiles and 
runs now.  I think all the problems are in the UI.  The menus quickly 
quit working and it doesn't repaint when it's supposed to unless I 
resize it, and the UI is getting some segfaults.  Shouldn't be too hard 
to debug with gdb.  I haven't tested if it plays nice with other nodes 
yet so keep it off-net.
* Email \#58:   
That would be great.  It's only TOR users that need it, so in the 
instructions saying "bitcoin -proxy=127.0.0.1:9050 -addnode=<someip>", 
someip could be an actual static IP, with the wiki free-for-all 
add-your-ip list nearby or a link to it.  There should be a link to that optional step, add your IP to this list now that you can accept incoming 
if you're static.
Do you think anonymous people are looking to be completely stealth, as 
in never connect once without TOR so nobody knows they use bitcoin, or 
just want to switch to TOR before doing any transactions?  It's just if 
you want to be completely stealth that you'd have to go through the 
-proxy -addnode manual seeding.  It would be very easy to fumble that 
up; if you run bitcoin normally to begin with it immediately 
automatically starts connecting.
  
***  
### Summary:    
(Email #52, #53, #56, #58): Connect via TOR.   
(Email #54, #55, #57): Update the Linux version of Bitcoin.   

1:(Email #52, #53, #56, #58): Connect via TOR:   
[Date: Tue, 03 Nov 2009 09:31:41 +0200] [Email #52]:  
Mmalmi said couldn't get connected using the Tor SOCKS proxy. 
That might be because of the Freenode Tor policy which requires connecting to their hidden service: http://freenode.net/irc_servers.shtml#tor   
[Date: Tue, 03 Nov 2009 15:53:25 +0000] [Email #53]:    
Satoshi said if you connect to Freenode they will also ban TOR due to abuse. Satoshi wondered if it was possible to register an account and then multiple people could use it?
The idea of sharing one account among multiple users was considered but it was uncertain whether it would work due to potential limitations or administration issues.
Check proxy for Bitcoin node connection: Check proxy connection for Bitcoin nodes has difficulty finding online nodes without IRC. It takes time to establish a connection without knowing the node address. The success of the outgoing TOR connection must be confirmed by checking the debug log for "connected" entries.
Find an alternative IRC server: suggestion request information on forum or mailing list TOR users cannot accept incoming connections: suggestion emphasizes the importance of setting up port forwarding on the router gland.  
[Date: Wed, 04 Nov 2009 23:42:44 +0200] [Email #56]:    
"Besides the IRC part, how did your test of proxy go?"   
=>answer to Satoshi's question above:"Enable proxy settings and restart 8 connections 2 connections to localhost:9050 7 connections from local port 8333 to elsewhere."
I wonder if they're all really through TOR.
And Mmalmi asks "why is there no debug.log in the directory I'm running in?"  
It is recommended to set up a static IP list to connect directly without needing an IRC server.   
[Date: Thu, 05 Nov 2009 15:25:27 +0000] [Email #58]:  
Satoshi answers the question "why is there no debug.log in the directory I'm running in?" by Mmalmi    
=> debug.log is moved to data directory "%appdata%/bitcoin/debug.log"    
For the topic:    
"If some nodes that accept incoming connections were willing to have theirIP coded into the program, it could seed automatically. Or some IP seed addresses posted on a Wiki page with the instructions. The wiki page sounds like a good and quickly applicable solution. I could keep my ip updated there and we could ask others to do thesame. When the Linux build works, it's easier to set up nodes on servers that are online most of the time and have a static IP. A static ip list shipped with Bitcoin and a peer exchange protocol would be cool. That way there'd be no need for an IRC server."    
=> Satoshi thinks that would be great. It's only TOR users that need it, so in the instructions saying "bitcoin -proxy=127.0.0.1:9050 -addnode=<someip>", someip could be an actual static IP, with the wiki free-for-all add-your -ip list nearby or a link to it. There should be a link to that optional step, add your IP to this list now that you can accept incoming if you're static.    
-Satoshi asked Mmalmi if he thinks that anonymous people are finding ways to be completely anonymous, such as never connecting once without TOR so no one knows if they use bitcoin or just want to transfer to TOR before making any transactions? Only if you want to completely hide then you have to do manual seeding -proxy -addnode. It's easy to figure that out; If you run bitcoin normally to start then it will automatically start connecting immediately.   
 
[Date: Thu, 05 Nov 2009 05:31:03 +0000] [Email #57]:  
Satoshi merged the linux changes into the main trunk on SVN.
It compiles and runs now.  Satoshi think all the problems are in the UI.
The menus quickly quit working and it doesn't repaint when it's supposed to unless Satoshi resize it, and the UI is getting some segfaults.  Shouldn't be too hard to debug with gdb. Satoshi haven't tested if it plays nice with other nodes yet so keep it off-net.  
***    
### Conclusion:  
In summary, these emails revolve around dealing with issues related to connecting to the Freenode IRC server via TOR and initiating connections to Bitcoin nodes.
