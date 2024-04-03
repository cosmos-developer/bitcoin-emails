#Summary 66 -\> 81

###Summary:
This email block focuses on a problem Satoshi and Mmalmi encountered, which related to an user got their Bitcoin lost.

###Context:

*Email \#66
Date: Sun, 08 Nov 2009 17:39:39 +0000
From: Satoshi Nakamoto <satoshin@gmx.com>
Subject: Re: Linux build ready for testing
To: Liberty Standard <newlibertystandard@gmail.com>
Cc: Martti Malmi <mmalmi@cc.hut.fi>
In the debug.log, it requests the block list, receives the block list, 
then begins uploading the list of blocks requested.  It doesn't receive 
the blocks, but it didn't run long enough for me to be sure it would 
have had time yet.  Everything else looks normal.

How long did you run it?  It could take a few minutes to start 
downloading the blocks.  Especially if you're on a cable modem, the 
uplink can be much lower bandwidth so it would take some time to upload 
the block request list.

If you run it again and it still doesn't download blocks, keep it 
running for several hours at least and then send me the debug.log.  That 
should give it time for my node to connect to you and I could see what 
it says on my side and correlate it with your debug.log.

You're right about the minimize on close option, there's no reason that 
can't be separate.  Martti originally had it separate and I made it a 
sub-option, my bad.  I'll change it back.

Liberty Standard wrote:
> That is what I meant. The blocks displayed in the status bar did not 
> increase at all while i ran the program. I have attached my debug.log.
> 
> A good way for you to test the tray icon in Gnome is to remove the 
> notification area and then add it back. If the icon is still displayed 
> after adding the notification back, then it's working correctly.
> 
> I generally set application preferences to not minimize to the tray, but 
> to close to the tray. And I keep the application minimized. That way I 
> don't accidentally close the program and still have the convenience of 
> being able to open the application from the tray. (I don't display open 
> windows in the 'task bar' but I have an icon that if clicked displays 
> open windows as sub-menu items.) Then if the tray icon disappears, I go 
> into the settings disable and re-enable the tray icon setting to get it 
> to reappear. That's currently not possible with the bitcoin preferences 
> because the close to tray check mark can not be enabled without the 
> minimize to tray check box being enabled.
> 
> 
> On Sun, Nov 8, 2009 at 9:08 AM, Satoshi Nakamoto <satoshin@gmx.com 
> <mailto:satoshin@gmx.com>> wrote:
> 
>     Liberty Standard wrote:
> 
>         I downloaded it and it runs. It and it is using plenty of CPU,
>         so I think it's working properly. It has not downloaded
>         previously generated blocks. Is that a bug or a new feature?
> 
> 
>     If you mean the blocks count in the status bar isn't working its way
>     up to around 26600, then that's a bug, you should send me your
>     debug.log. (which is at ~/.bitcoin/debug.log)
> 
> 
>         The system tray in Gnome is not very reliable. Sometimes an icon
>         will disappear leaving no way to get back to the program. I have
>         verified that this can happen with bitcoin. It would be nice if
>         starting bitcoin while it's already running would just bring up
>         the GUI of the already running bitcoin process.
> 
> 
>     We haven't figured out how to find and bring up the existing running
>     program yet on Linux like it does on Windows.  Given what you say, I
>     should at least turn off the minimize to tray option initially by
>     default.

*Email \#68
Date: Mon, 09 Nov 2009 01:23:59 +0000
From: Satoshi Nakamoto <satoshin@gmx.com>
Subject: Re: Linux build ready for testing
To: Liberty Standard <newlibertystandard@gmail.com>
Cc: Martti Malmi <mmalmi@cc.hut.fi>
Liberty Standard wrote:
> Ok, blocks have now started to increase. It definitely takes longer for 
> them to start increasing than with the Windows version. Also, I think 
> they might be increasing at a slower rate than in with the Windows 
> version. Is there perhaps debugging enabled in the Linux build that you 
> sent me? Block are increasing at about 15 blocks per second (eyeball 
> estimate while looking at a clock). I didn't time how fast they 
> increased in the Windows version, but it seems like it was much faster.

About how long did it take to start?  It could be the node that you 
happened to request from is slow.  The slow start is consistent with the 
slow download speed.

I'd like to look at your current debug.log file and try to understand 
what's going.  It might just be a really slow connection on the other 
side, or maybe something's wrong and failed and retried.  Taking too 
long could confuse other users.

Martti, how long did it take to start downloading blocks when you ran 
it, and how fast did it download?

>     When I launch bitcoin and the bitcoin port is not available, I get
>     the following messages to the command line. I don't get those
>     messages when the bitcoin port is available. Would it be possible
>     for bitcoin to pick another port if the default port is taken? The
>     same think sometimes happens to me with my BitTorrent client. When I
>     restart it, my previously open port is closed. All I have to do is
>     change the port and it starts working again.
> 
>     /usr/lib/gio/modules/libgvfsdbus.so: wrong ELF class: ELFCLASS64
>     Failed to load module: /usr/lib/gio/modules/libgvfsdbus.so
>     /usr/lib/gio/modules/libgioremote-volume-monitor.so: wrong ELF
>     class: ELFCLASS64
>     Failed to load module:
>     /usr/lib/gio/modules/libgioremote-volume-monitor.so
>     /usr/lib/gio/modules/libgiogconf.so: wrong ELF class: ELFCLASS64
>     Failed to load module: /usr/lib/gio/modules/libgiogconf.so

It already uses SO_REUSEADDR so it can bind to the port if it's in 
TIME_WAIT state after being closed.  The only time it should fail to 
bind is when the program really is already running.  It's important that 
two copies of Bitcoin not run on the same machine at once because they 
would be modifying the database at the same time.  There is never any 
need to run two on one machine as coin generation will now use multiple 
processors automatically.

I'm not sure what those lib errors are, I'll do some searching.

Email #69
Date: Mon, 09 Nov 2009 05:42:59 +0000
From: Satoshi Nakamoto <satoshin@gmx.com>
Subject: Re: Linux build ready for testing
To: Liberty Standard <newlibertystandard@gmail.com>
Cc: Martti Malmi <mmalmi@cc.hut.fi>
Thanks for that, I see what happened.  Because the first one was slow, 
it ended up requesting the blocks from everybody else, which only bogged 
everything down.  I can fix this, I just need to think a while about the 
right way.

There's no risk in shutting down while there are unconfirmed.  When you 
make a transaction or new block, it immediately broadcasts it to the 
network.  After that, the increasing #/confirmed number is just 
monitoring the outcome.  There's nothing your node does during that time 
to promote the acceptance.

Now that I think about it, when you close Bitcoin, it closes the main 
window immediately but in the background continues running to finish an 
orderly flush and shutdown of the database.  Before I implemented that, 
it was annoying having a dead hung unresponsive window hanging around. 
Until it finishes the orderly shutdown in the background, the port would 
be locked, and this is an important protection to make sure another copy 
can't touch the database until it's done.  I haven't seen the shutdown 
take more than a few seconds.

In Wine, there's no way for the Windows version to do SO_REUSEADDR, so 
that would add 60 seconds (on my system) of TIME_WAIT after the port is 
closed.

If you need to transfer between two copies, you could send it to the 
other's bitcoin address.  The receiving copy doesn't have to be online 
at the time.

The command line to use a different data directory is
bitcoin -datadir=<directory>

For example, on Linux, the default directory is (don't use ~)
bitcoin -datadir=/home/yourusername/.bitcoin

[OYou shouldn't normally have any need to use this switch.  It still won't 
let you run two instances at once.

Liberty Standard wrote:
> On Mon, Nov 9, 2009 at 3:23 AM, Satoshi Nakamoto <satoshin@gmx.com 
> <mailto:satoshin@gmx.com>> wrote:
> 
>     Liberty Standard wrote:
> 
>         Ok, blocks have now started to increase. It definitely takes
>         longer for them to start increasing than with the Windows
>         version. Also, I think they might be increasing at a slower rate
>         than in with the Windows version. Is there perhaps debugging
>         enabled in the Linux build that you sent me? Block are
>         increasing at about 15 blocks per second (eyeball estimate while
>         looking at a clock). I didn't time how fast they increased in
>         the Windows version, but it seems like it was much faster.
> 
> 
>     About how long did it take to start?  It could be the node that you
>     happened to request from is slow.  The slow start is consistent with
>     the slow download speed.
> 
> 
> It took about a half hour for it to start incrementing quickly. 
> Interestingly, the CPU usage increased before it started to increment 
> steadily and then lowered when it started to increment steadily. 
> Although this time the block incremented to 2 within the first few 
> minutes. I have not yet generated any bitcoins. I'll wait for as long as 
> I have patience to generate a bitcoin, but if none are created by the 
> time I lose patience, I'm going to move back to the wine version.
> 
>     I'd like to look at your current debug.log file and try to
>     understand what's going.  It might just be a really slow connection
>     on the other side, or maybe something's wrong and failed and
>     retried.  Taking too long could confuse other users.
> 
> 
> I've included my current debug.log.
>  
> 
>     Martti, how long did it take to start downloading blocks when you
>     ran it, and how fast did it download?
> 
> 
>            When I launch bitcoin and the bitcoin port is not available,
>         I get
>            the following messages to the command line. I don't get those
>            messages when the bitcoin port is available. Would it be possible
>            for bitcoin to pick another port if the default port is
>         taken? The
>            same think sometimes happens to me with my BitTorrent client.
>         When I
>            restart it, my previously open port is closed. All I have to
>         do is
>            change the port and it starts working again.
> 
>            /usr/lib/gio/modules/libgvfsdbus.so: wrong ELF class: ELFCLASS64
>            Failed to load module: /usr/lib/gio/modules/libgvfsdbus.so
>            /usr/lib/gio/modules/libgioremote-volume-monitor.so: wrong ELF
>            class: ELFCLASS64
>            Failed to load module:
>            /usr/lib/gio/modules/libgioremote-volume-monitor.so
>            /usr/lib/gio/modules/libgiogconf.so: wrong ELF class: ELFCLASS64
>            Failed to load module: /usr/lib/gio/modules/libgiogconf.so
> 
> 
>     It already uses SO_REUSEADDR so it can bind to the port if it's in
>     TIME_WAIT state after being closed.  The only time it should fail to
>     bind is when the program really is already running.  It's important
>     that two copies of Bitcoin not run on the same machine at once
>     because they would be modifying the database at the same time.
>      There is never any need to run two on one machine as coin
>     generation will now use multiple processors automatically.
> 
> 
> The reason I run two instances at the same time is to transfer bitcoins 
> from one bitcoin instance to another. They of course would need to be 
> accessing different data directories. Perhaps that could be specified as 
> a command line argument. I currently have to move my bitcoin data folder 
> to a virtual machine to do this. Shutting down bitcoin and restarting it 
> with a different data directory is a poor solution because shutting down 
> bitcoin while there are unconfirmed bitcoins risks losing those bitcoins.
> 
> Bitcoin was definitely not running when i get the busy port error. The 
> process closes quickly and reliably from my experience, but it takes 
> anywhere from 30 seconds to 3 minutes (estimation from memory) for the 
> port to become available again. It occurred while switching from bitcoin 
> 0.1.5 in Wine to the Linux build and again while switching from the 
> Linux build to bitcoin 0.1.5 in Wine.
> 
> Another thing that I noticed is that the about dialog text does not fit 
> correctly and it cannot be resized. 
> 
>     I'm not sure what those lib errors are, I'll do some searching.

*Email \#70
Date: Mon, 09 Nov 2009 10:32:08 +0200
From: mmalmi@cc.hut.fi
To: Satoshi Nakamoto <satoshin@gmx.com>
Cc: Liberty Standard <newlibertystandard@gmail.com>
Subject: Re: Linux build ready for testing
> Martti, how long did it take to start downloading blocks when you ran
> it, and how fast did it download?

Started very quickly when I got connected and downloaded quicker than  
my Windows PC, which has a slower CPU.

I'll have to focus on a school project (coincidentally C++ coding) for  
about a month now, so I don't have that much time for active  
developing until December. Let's keep contact anyway.

> Liberty Standard wrote:
>> Ok, blocks have now started to increase. It definitely takes longer  
>>  for them to start increasing than with the Windows version. Also,  
>> I  think they might be increasing at a slower rate than in with the  
>>  Windows version. Is there perhaps debugging enabled in the Linux   
>> build that you sent me? Block are increasing at about 15 blocks per  
>>  second (eyeball estimate while looking at a clock). I didn't time   
>> how fast they increased in the Windows version, but it seems like   
>> it was much faster.
>
> About how long did it take to start?  It could be the node that you
> happened to request from is slow.  The slow start is consistent with
> the slow download speed.
>
> I'd like to look at your current debug.log file and try to understand
> what's going.  It might just be a really slow connection on the other
> side, or maybe something's wrong and failed and retried.  Taking too
> long could confuse other users.
>
> Martti, how long did it take to start downloading blocks when you ran
> it, and how fast did it download?
>
>>    When I launch bitcoin and the bitcoin port is not available, I get
>>    the following messages to the command line. I don't get those
>>    messages when the bitcoin port is available. Would it be possible
>>    for bitcoin to pick another port if the default port is taken? The
>>    same think sometimes happens to me with my BitTorrent client. When I
>>    restart it, my previously open port is closed. All I have to do is
>>    change the port and it starts working again.
>>
>>    /usr/lib/gio/modules/libgvfsdbus.so: wrong ELF class: ELFCLASS64
>>    Failed to load module: /usr/lib/gio/modules/libgvfsdbus.so
>>    /usr/lib/gio/modules/libgioremote-volume-monitor.so: wrong ELF
>>    class: ELFCLASS64
>>    Failed to load module:
>>    /usr/lib/gio/modules/libgioremote-volume-monitor.so
>>    /usr/lib/gio/modules/libgiogconf.so: wrong ELF class: ELFCLASS64
>>    Failed to load module: /usr/lib/gio/modules/libgiogconf.so
>
> It already uses SO_REUSEADDR so it can bind to the port if it's in
> TIME_WAIT state after being closed.  The only time it should fail to
> bind is when the program really is already running.  It's important
> that two copies of Bitcoin not run on the same machine at once because
> they would be modifying the database at the same time.  There is never
> any need to run two on one machine as coin generation will now use
> multiple processors automatically.
>
> I'm not sure what those lib errors are, I'll do some searching.

*Email \#71
Date: Mon, 09 Nov 2009 19:30:53 +0000
From: Satoshi Nakamoto <satoshin@gmx.com>
Subject: Re: Linux build ready for testing
To: Liberty Standard <newlibertystandard@gmail.com>
Cc: Martti Malmi <mmalmi@cc.hut.fi>
You really don't want to keep running in Wine, you're getting database 
errors (db.log).  You probably developed these rituals of transferring 
to a fresh install to cope with database corruption.  If there is a way 
to lose unconfirmed blocks, it would have to be the database errors. 
Any problems you find in the Linux build can be fixed.  The Wine 
incompatibility deep inside Berkeley DB is unfixable.

I think GCC 4.3.3 on the Linux build optimized the SHA-256 code better 
than the old GCC 3.4.5 on Windows.  When I was looking for the best 
SHA-256 code, there was a lot of hand tuned highly optimized SHA1 code 
available, but not so much for SHA-256 yet.  I should see if I can 
upgrade MinGW to 4.3.x to get them on a level playing field.

Liberty Standard wrote:
> Everyone that contributed to making this Linux build really did a great 
> job! Thanks for the hard work. It has started maturing some bitcoins, so 
> I'm going to continue to run the Linux client for the time being until I 
> decide whether it's at least as good or better at generating coins than 
> the Windows version running in Wine.
> 
> 
> On Mon, Nov 9, 2009 at 8:59 AM, Liberty Standard 
> <newlibertystandard@gmail.com <mailto:newlibertystandard@gmail.com>> wrote:
> 
>     Another instance when I would like to run multiple instances is when
>     I upgrade bitcoin. I will uncheck the generate coin check box in the
>     outdated bitcoin, launch and start generating coins in the new
>     bitcoin using a separate data directory, then when the old
>     application's coins have matured I will send them to the new
>     application and then close the old application. I prefer do do clean
>     installs rather than upgrading while maintaining old data.
> 
> 
> 
>     On Mon, Nov 9, 2009 at 7:42 AM, Satoshi Nakamoto <satoshin@gmx.com
>     <mailto:satoshin@gmx.com>> wrote:
> 
>         Thanks for that, I see what happened.  Because the first one was
>         slow, it ended up requesting the blocks from everybody else,
>         which only bogged everything down.  I can fix this, I just need
>         to think a while about the right way.
> 
>         There's no risk in shutting down while there are unconfirmed.
>          When you make a transaction or new block, it immediately
>         broadcasts it to the network.  After that, the increasing
>         #/confirmed number is just monitoring the outcome.  There's
>         nothing your node does during that time to promote the acceptance.

*Email \#73
Date: Tue, 10 Nov 2009 16:46:04 +0000
From: Satoshi Nakamoto <satoshin@gmx.com>
Subject: Re: Linux - dead sockets problem
To: Liberty Standard <newlibertystandard@gmail.com>
Cc: Martti Malmi <mmalmi@cc.hut.fi>
I see what happened.  All your sockets went dead somehow.  You had no 
communication with the network, but because you had 8 zombie 
connections, it thought it was still online and kept generating blocks. 
  You can tell this is happening when your blocks are numbered 
sequentially, without other people's blocks interspersed, like:
2/unconfirmed
3/unconfirmed
4/unconfirmed
5/unconfirmed
6 blocks
7 blocks

It's implausible that you would be the only one to find blocks for 6 
blocks in a row like that.

When you exited and restarted, it connected and downloaded 45 blocks 
that the network found in your absence.  Since your blocks were not 
broadcast to the network immediately, the network went on without them.

It sounds like you had exactly the same problem on Wine.  There's 
clearly something about socket handling on Linux that's effecting it 
either way.

I'll start researching this.  Ultimately if I can't find the root of the 
problem, I'll have to make some kind of mechanism to watch for an 
absence of messages and disconnect.  The only workaround for you right 
now would be to exit and restart more often.

All but one of your node connections went dead at the same time, one 
shortly after.  IRC was still working, so it wasn't that you were 
offline from the internet.

I wonder if the status of blocks should say "#/unconfirmed" all the way 
up to maturity (119/unconfirmed then 120 blocks) instead.  The meaning 
of the number isn't as strong for blocks as for transactions.

I think it would be an improvement not to count one's own blocks as 
confirmations.  A drawback would be that the status numbers shown by 
different nodes would not match.  The status number would no longer be 
coordinated with the maturity countdown on blocks either.  A lighter 
option would be a special case only if all confirmations are your own.

Liberty Standard wrote:
> I just lost 6 sets of maturing coins! I had 10 sets of bitcoins 
> maturing. The last set was generated at about 0:22. It got to 
> 2/unconfirmed before bitcoin got stuck. At 10:10, the bitcoin which was 
> generated at 0:22 was still only at 2/unconfirmed. Since you had told me 
> that I wasn't going to lose coins, I shutdown and restarted bitcoin. On 
> the bright side, it shutdown and started up very smoothly. But 
> unfortunately, when the blocks updated, I lost 6 sets of bitcoins. Four 
> sets were still unconfirmed, but two sets were confirmed. And there's no 
> trace of them now. Perhaps now that you have the 'Show Generated Coins' 
> option available, you can put back in failed bitcoin generations. I just 
> don't like that those bitcoins just disappeared into thin air. I'm still 
> running the Linux build at the moment, but the Wine version is suddenly 
> looking much more attractive now that 6 out of the 10 sets of bitcoins I 
> generated in the past 24 hours just vanished. I've included my debug.log.
> 
> 
> On Tue, Nov 10, 2009 at 1:45 AM, Liberty Standard 
> <newlibertystandard@gmail.com <mailto:newlibertystandard@gmail.com>> wrote:
> 
>     The Linux build has generated a decent amount of bitcoins within the
>     past 20 hours and I trust what you're telling me about database
>     errors, so all signs point toward me running the Linux build from
>     now on. The only half annoying thing about the Linux build is that
>     my computer's fan has gone from 50% to 100%. :-P I know I can limit
>     the CPU, so if it gets on my nerves too much and if I can live with
>     less bitcoins being generated, perhaps I'll do that. Or maybe I just
>     need to start listening to more music...
> 
...
> 
>                    There's no risk in shutting down while there are
>             unconfirmed.
>                     When you make a transaction or new block, it immediately
>                    broadcasts it to the network.  After that, the increasing
>                    #/confirmed number is just monitoring the outcome.
>              There's
>                    nothing your node does during that time to promote
>             the acceptance.

*Email \#74
Date: Wed, 11 Nov 2009 00:39:19 +0000
From: Satoshi Nakamoto <satoshin@gmx.com>
Subject: Re: Linux - linux-0.1.6-test2
To: Liberty Standard <newlibertystandard@gmail.com>
Cc: Martti Malmi <mmalmi@cc.hut.fi>
I fixed a few places I found where it was possible for a socket to get 
an error and not get disconnected.  If your connections go dead again, 
it should disconnect and reconnect them.  I also implemented an 
inactivity timeout as a fallback.

This also includes a partial fix for the slow initial block download.

You should run with the "-debug" switch to get some additional debug.log 
information I added that'll help if there are more problems.

linux-0.1.6-test2.tar.bz2  12,134,012 bytes
Download:
http://rapidshare.com/files/305231818/linux-0.1.6-test2.tar.bz2.html


Satoshi Nakamoto wrote:
> I see what happened.  All your sockets went dead somehow.  You had no 
> communication with the network, but because you had 8 zombie 
> connections, it thought it was still online and kept generating blocks. 
>  You can tell this is happening when your blocks are numbered 
> sequentially, without other people's blocks interspersed, like:
> 2/unconfirmed
> 3/unconfirmed
> 4/unconfirmed
> 5/unconfirmed
> 6 blocks
> 7 blocks
> 
> It's implausible that you would be the only one to find blocks for 6 
> blocks in a row like that.
> 
> When you exited and restarted, it connected and downloaded 45 blocks 
> that the network found in your absence.  Since your blocks were not 
> broadcast to the network immediately, the network went on without them.
> 
> It sounds like you had exactly the same problem on Wine.  There's 
> clearly something about socket handling on Linux that's effecting it 
> either way.
> 
> I'll start researching this.  Ultimately if I can't find the root of the 
> problem, I'll have to make some kind of mechanism to watch for an 
> absence of messages and disconnect.  The only workaround for you right 
> now would be to exit and restart more often.
> 
> All but one of your node connections went dead at the same time, one 
> shortly after.  IRC was still working, so it wasn't that you were 
> offline from the internet.
> 
> I wonder if the status of blocks should say "#/unconfirmed" all the way 
> up to maturity (119/unconfirmed then 120 blocks) instead.  The meaning 
> of the number isn't as strong for blocks as for transactions.
> 
> I think it would be an improvement not to count one's own blocks as 
> confirmations.  A drawback would be that the status numbers shown by 
> different nodes would not match.  The status number would no longer be 
> coordinated with the maturity countdown on blocks either.  A lighter 
> option would be a special case only if all confirmations are your own.
> 
> Liberty Standard wrote:
>> I just lost 6 sets of maturing coins! I had 10 sets of bitcoins 
>> maturing. The last set was generated at about 0:22. It got to 
>> 2/unconfirmed before bitcoin got stuck. At 10:10, the bitcoin which 
>> was generated at 0:22 was still only at 2/unconfirmed. Since you had 
>> told me that I wasn't going to lose coins, I shutdown and restarted 
>> bitcoin. On the bright side, it shutdown and started up very smoothly. 
>> But unfortunately, when the blocks updated, I lost 6 sets of bitcoins. 
>> Four sets were still unconfirmed, but two sets were confirmed. And 
>> there's no trace of them now. Perhaps now that you have the 'Show 
>> Generated Coins' option available, you can put back in failed bitcoin 
>> generations. I just don't like that those bitcoins just disappeared 
>> into thin air. I'm still running the Linux build at the moment, but 
>> the Wine version is suddenly looking much more attractive now that 6 
>> out of the 10 sets of bitcoins I generated in the past 24 hours just 
>> vanished. I've included my debug.log.
>>
>>
>> On Tue, Nov 10, 2009 at 1:45 AM, Liberty Standard 
>> <newlibertystandard@gmail.com <mailto:newlibertystandard@gmail.com>> 
>> wrote:
>>
>>     The Linux build has generated a decent amount of bitcoins within the
>>     past 20 hours and I trust what you're telling me about database
>>     errors, so all signs point toward me running the Linux build from
>>     now on. The only half annoying thing about the Linux build is that
>>     my computer's fan has gone from 50% to 100%. :-P I know I can limit
>>     the CPU, so if it gets on my nerves too much and if I can live with
>>     less bitcoins being generated, perhaps I'll do that. Or maybe I just
>>     need to start listening to more music...
>>
> ...
>>
>>                    There's no risk in shutting down while there are
>>             unconfirmed.
>>                     When you make a transaction or new block, it 
>> immediately
>>                    broadcasts it to the network.  After that, the 
>> increasing
>>                    #/confirmed number is just monitoring the outcome.
>>              There's
>>                    nothing your node does during that time to promote
>>             the acceptance.

*Email \#76
Date: Thu, 12 Nov 2009 05:36:06 +0000
From: Satoshi Nakamoto <satoshin@gmx.com>
Subject: Linux - linux-0.1.6-test3
To: Liberty Standard <newlibertystandard@gmail.com>
Cc: Martti Malmi <mmalmi@cc.hut.fi>
Right now (04:50 GMT) my node is connecting to yours and getting zombie 
connections each time.  The socket isn't returning an error, just zombie 
without notice.  If you're running the linux build right now, it would 
be interesting to see what the log says on your side.

test3:

I've added specific code to detect zombie sockets.  It'll detect if the 
socket hasn't sent or received any data within 60 seconds of connecting, 
and detect if data is queued to send and hasn't sent for 3 minutes.

I think I may have weakened the reconnect speed in test2.  In test3 I'm 
making it more determined to reconnect quickly.

I added checking to track whether other nodes received your generated 
blocks.  If none did, it'll warn you in the description:
"Generated - Warning: This block was not received by any other nodes and 
will probably not be accepted!"

The status can go to "#/offline?" for blocks or transactions you create 
if they don't get out to any other nodes.

With all this, it should be impossible not to notice as soon as it 
screws up.  It should hopefully disconnect all the zombie sockets. 
After that, whether it's able to make some good connections, or sockets 
is completely hosed and it stays at 0 connections, I don't know.

If this doesn't work, I guess I'll look at the sourcecode of some other 
P2P apps like BitTorrent and see how they deal with this stuff.  Maybe 
there's some magic flag or procedure to bash the sockets system back to 
life.

File linux-0.1.6-test3.tar.bz2 attached in the next message.


Liberty Standard wrote:
> On Wed, Nov 11, 2009 at 8:08 AM, Liberty Standard 
> <newlibertystandard@gmail.com <mailto:newlibertystandard@gmail.com>> wrote:
> 
>     My network connection is direct to my computer. My ISP requires that
>     I run VPN to connect to the Internet. I then have a second NIC that
>     shares my Internet with other devices. My IP address while using my
>     computer is my actual IP address, but the devices connected through
>     my second NIC use NAT. When I connect through a virtual machine,
>     that also uses NAT. All this requires very little configuration.
>     NetworkManager in Ubuntu has an option to share my Internet
>     connection through the second NIC and VirtualBox has the option to
>     use NAT.
> 
>     I lost a couple packs of bitcoins again, so that problem is not yet
>     fixed. It's a bit more bearable now that I have an idea of what is
>     going on. I figure for now I'll just restart bitcoin whenever I see
>     a pack of bitcoins starting to mature. I may go back and forth a bit
>     between Linux and Wine, but I'll definitely test every new version
>     that comes out. At the moment I'm still running the Linux build.
> 
> 
> 
>     On Wed, Nov 11, 2009 at 7:49 AM, Satoshi Nakamoto <satoshin@gmx.com
>     <mailto:satoshin@gmx.com>> wrote:
> 
>         Thanks.  The log didn't stop on anything special, just simple
>         message passing.  Chances are it's UI related.  Most of the
>         initial bugs were all UI.
> 
>         What brand/model of firewall do you have?  It's possible for
>         BitTorrent to overwhelm the number of connections some models
>         can handle.  Most are underpowered and flaky under load.
> 
>         NewLibertyStandard wrote:
> 
>             I have been getting your attachments just fine. I just
>             thought I'd spare Martti the large attachment.
> 
>             I am not able to reproduce the bug. I don't know whether the
>             paste, the blocks finishing, a combination of the two or
>             something else entirely caused the fault.
> 
>         ...
> 
>                     But after they started
>                    downloading, I took a look a look at my BitTorrent
>             client, and
>                    sure enough, I had forgotten about a torrent and my
>             upload was
>                    quite high, at the limit I had set for it.

Email #78
Date: Thu, 12 Nov 2009 23:39:44 +0000
From: Satoshi Nakamoto <satoshin@gmx.com>
Subject: linux-0.1.6-test5 fix for zombie sockets
To: Liberty Standard <newlibertystandard@gmail.com>
Cc: Martti Malmi <mmalmi@cc.hut.fi>
test 5:

I added MSG_DONTWAIT to the send and recv calls in case they forgot the 
socket is non-blocking.  If that doesn't work, there's now the catch-all 
solution: another thread monitors the send/recv thread and terminates 
and restarts it if it stops.  It prints "*** Restarting 
ThreadSocketHandler ***" in debug.log, and an error message displays on 
the status bar for a while.

Before terminating, it tries closing the socket that's hung.  If that 
works, it doesn't have to resort to terminating.

I ran a test where it terminated the thread about 1000 times without 
trouble, so it should be safe.  The terminate on linux is 
pthread_cancel, which throws it into C++'s exception handler.

The thread calls we were using didn't have terminate, so I created our 
own wrappers in util.h to use CreateThread on windows and pthread_create 
on linux, instead of:
   _beginthread is windows only and lacks terminate
   boost::thread is really attractive, but lacks terminate
   wxThread requires you to create a class for every function you might 
call (yuck)

File attached in the next e-mail

Email #81
Date: Sun, 15 Nov 2009 15:40:29 +0000
From: Satoshi Nakamoto <satoshin@gmx.com>
Subject: Linux update
To: Martti Malmi <mmalmi@cc.hut.fi>
linux-0.1.6-test5 solved Liberty's zombie socket problem.  The 
MSG_DONTWAIT fixed the root cause, it's not having to terminate and 
restart the thread.  The sockets are marked non-blocking already, so I 
don't understand why.  Maybe it forgot.  I suppose if a socket fails and 
the OS closes it then there's nothing left to remember it was 
non-blocking, but then accessing a closed handle should return 
immediately with an error.  There's no MSG_DONTWAIT on Windows, marking 
the socket as nonblocking is the only way, so if anyone runs the Windows 
version in Wine it will have to rely on terminating the thread.

The only problem now is the DB exceptions he's getting.
************************
EXCEPTION: 11DbException
Db::open: Bad file descriptor
bitcoin in ThreadMessageHandler()
************************
EXCEPTION: 11DbException
Db::close: Bad file descriptor
bitcoin in ThreadMessageHandler()

I had expected those to be a Wine problem, but he's getting them on 
Linux just the same.  He tried moving the datadir to a different drive, 
no help.  I've never gotten them.  I'm running a stress test that 
continuously generates a lot of activity and DB access and never got it.

He has Ubuntu 64-bit and I have 32-bit, so I'm assuming that's the 
difference.  Is your Linux machine 64-bit or 32-bit?  Have you ever had 
a DB exception? (see db.log also)  Now that the zombie problem is fixed 
in test5, could you start running it on your Linux machine?  We could 
use a 3rd vote to get a better idea of what we're dealing with here. 
The DB exception is uncaught, so it'll stop the program if you get it.

BTW, zetaboards insists on displaying "Member #", so you better sign up 
soon and grab a good account number.

Email #82
Date: Sun, 15 Nov 2009 19:55:35 +0200
From: mmalmi@cc.hut.fi
To: Satoshi Nakamoto <satoshin@gmx.com>
Subject: Re: Linux update
The program terminated a few times with the same error in debug.log  
close: Bad file descriptor
blkindex.dat: Bad file descriptor

I'm running a 64-bit Ubuntu distribution.

> The only problem now is the DB exceptions he's getting.
> ************************
> EXCEPTION: 11DbException
> Db::open: Bad file descriptor
> bitcoin in ThreadMessageHandler()
> ************************
> EXCEPTION: 11DbException
> Db::close: Bad file descriptor
> bitcoin in ThreadMessageHandler()
>
> I had expected those to be a Wine problem, but he's getting them on
> Linux just the same.  He tried moving the datadir to a different drive,
> no help.  I've never gotten them.  I'm running a stress test that
> continuously generates a lot of activity and DB access and never got it.
>
> He has Ubuntu 64-bit and I have 32-bit, so I'm assuming that's the
> difference.  Is your Linux machine 64-bit or 32-bit?  Have you ever had
> a DB exception? (see db.log also)  Now that the zombie problem is fixed
> in test5, could you start running it on your Linux machine?  We could
> use a 3rd vote to get a better idea of what we're dealing with here.
> The DB exception is uncaught, so it'll stop the program if you get it.
>
> BTW, zetaboards insists on displaying "Member #", so you better sign up
> soon and grab a good account number.

### Section summary

*[Sun, 08 Nov 2009 17:39:39 +0000] [Email #66]:
LibertyStandard wrote to complain why his blocks count didn't get updated 
as intended? he wondered if this was a UI bug or is this a new feature?
he thought is this is could be an error where the blocks didn't get
downloaded correctly? 

Satoshi said this could be caused by low internet speed, he suggested
Liberty to let the program running for a few hours and then re-send him
the debug.log file.

Liberty also mentioned a UI feature needed to be alternated and Satoshi
said he didn't manage to make it possible at the moment.

*[Mon, 09 Nov 2009 01:23:59 +0000] [Email #68]

Liberty started to see the blocks got downloaded at a rate of 15 
blocks/sec and it seemed to be slower than the Windows version? and he 
asked why? maybe because Satoshi enabled debugging in Linux build which
caused this problem?

Satoshi said it could be slow because the node Liberty requested from
was slow. He asked to see the current debug.log

Liberty said that he got some messages of library errors in command line 
when he launched bitcoin and he didn't know why? maybe because his default 
bitcoin port was taken? and then he asked would it be possible for bitcoin 
picking up another port and start using it?

Satoshi was not sure about the lib errors, he would research further.
Satoshi said bitcoin already used SO_REUSEADDR so if it failed to bind
is when the program is really already running. He said if two copies of 
bitcoin run on the same machine at once they would modify the database
at the same time. He thought Liberty was trying to double the performance
of bitcoin mining by running two instances.

[Mon, 09 Nov 2009 05:42:59 +0000] [Email #69]
Everything was so slow, Libery said he may leave Linux build to switch back 
to Wine version in  what scenario? if he couldn't mine any bitcoins? he 
needed to transfer bitcoins between his two bitcoin instances so he used
virtual machine to do so, but why? because he thought two nodes had to be 
online to start transfering bitcoins? and he complaint why the didn't the
program release the port instantly after turning it off? and are there any
risks if he turns it off in the middle of something?

Satoshi said it was slow because when Liberty requested from node A and that 
was slow, his node started to request from every other nodes which slowed
everything down. He said after closing the program, it still runs in the 
background to finish and clean things up. The port is locked during that
proccess, just so another bitcoin instance can't touch the database until
it's done. To transfer bitcoins, Liberty should just transfer to the exact
address, the receiving copy doesn't have to be online at the time. Satoshi
said turning the program off is risk-free.

[Mon, 09 Nov 2009 10:32:08 +0200] [Email #70]
Malmi said his download rate was normal.

[Mon, 09 Nov 2009 19:30:53 +0000] [Email #71]
Satoshi said database erros is the only way to lose blocks.

[Tue, 10 Nov 2009 16:46:04 +0000] [#Email 73]
Liberty reported that he lost 6 out of 10 sets of maturing coins but how? 
maybe it was caused by shutting down the program mid-confirmation? he 
considered to switching back to Wine even though he once said he would stick 
to the Linux, why? because all of the blocks he lost? or because Linux build
made his computer overload?

Satoshi said the lost blocks had never been broadcasted to the network because
Liberty's sockets went dead somehow, Liberty is still on the internet because
IRC was still working. A block is made doesn't mean it will be broadcasted.

[Wed, 11 Nov 2009 00:39:19 +0000] [#Email 74]
Satoshi fixed a few places which may cause errors for a socket. If connetions
go dead, it should disconnect and reconnect again.

[Date: Thu, 12 Nov 2009 05:36:06 +0000] [#Email 76]
Liberty acknowledged the problems, but they didn't get fixed? the convos here
got pretty vague because there messages we can't approch from the POV of Malmi.

Satoshi connected to Liberty's, the socket didnt return errors even though
they are zombie ones. He attached a test for Liberty.

About Satoshi test3:
1. code to detect zombie sockets
2. improved the reconnect speed compares to test2
3. a warning if other nodes don't receive any blocks by you
4. the blocks status is more clear, from #/unconfirmed to #/offline if they
are not broadcasted

[Thu, 12 Nov 2009 23:39:44 +0000] [#Email 78]
About Satoshi's test5:
He added MSG_DONTWAIT to solve the sockets problem, this marks zombie sockets as
non-blockingg sockets. Operations related to these sockets will retutrn immediately
if they cannot complete requested operation at the moment.

There is messages from Lirberty in the next few days, I assume Satoshi's solution
worked.
