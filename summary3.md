# Summary 1->52

### Summary:
This email explains the vision of Satoshi about the existance of Bitcoin and its mission.

### Context

* Email \#3
Date: Sun, 03 May 2009 23:32:26 +0100
From: Satoshi Nakamoto <satoshin@gmx.com>
Subject: Re: Bitcoin
To: mmalmi@cc.hut.fi
1.
 > Since they can be created for free (or at the cost
 > of computer power people have anyway for other reasons),
 > monetizing them means simply giving away money.

You're still thinking as if the difficulty level will be so easy that 
people will be able to generate all the bitcoins they want.

Imagine you have to run your computer 24/7 for a month to generate 1 
cent.  After a year, you could generate 12 cents.  That's not going to 
make it so people can just generate all the bitcoin they want for spending.

The value of bitcoins would be relative to the electricity consumed to 
produce them.  All modern CPUs save power when they're idle.  If you run 
a computational task 24/7, not letting it idle, it uses significantly 
more power, and you'll notice it generates more heat.  The extra wattage 
consumed goes straight to your power bill, and the value of the bitcoins 
you produce would be something less than that."

2.
 > For some reason your transfer to me shows up as "From: unknown" even
 > though I added you to my address book.
 >
 > I have a "Generated (not accepted)" line in my transaction list, it
 > seems like an attempt to generate a coin went wrong somehow. Not sure
 > what happened here - presumably my node successfully solved a block
 > but then I went offline before it was sent to the network?

Transactions sent to a bitcoin address will always say "from: unknown". 

3.
> ...understand correctly, there is only one (or maybe a few) global
 > chain[s] into which all transactions are hashed. If there is only one
 > chain recording "the story of the economy" so to speak, how does this
 > scale? In an imaginary planet-wide deployment there would be millions
 > of even billions of transactions per hour being hashed into the chain...

The existing Visa credit card network processes about 15 million 
Internet purchases per day worldwide.  Bitcoin can already scale much 
larger than that with existing hardware for a fraction of the cost.  It 
never really hits a scale ceiling.  If you're interested, I can go over 
the ways it would cope with extreme size.

By Moore's Law, we can expect hardware speed to be 10 times faster in 5 
years and 100 times faster in 10.  Even if Bitcoin grows at crazy 
adoption rates, I think computer speeds will stay ahead of the number of 
transactions.

4.
 > ...How did you decide on the inflation schedule for v1? Where did 21
 > million coins come from? What denominations are these coins? You
 > mention a way to combine and split value but I'm not clear on how this
 > works. For instance are bitcoins always denominated by an integer or
 > can you have fractional bitcoins?...
By Moore's Law, we can expect hardware speed to be 10 times faster in 5 
years and 100 times faster in 10.  Even if Bitcoin grows at crazy 
adoption rates, I think computer speeds will stay ahead of the number of 
transactions.
> Won't there be massive inflation as computers get faster and are able 
to solve the proof-of-work problem faster?
The difficulty is controlled by a moving average that compensates for 
the total effort being expended to keep the total production constant. 
As computers get more powerful, the difficulty increases to compensate.

5.
> One other question I had... What prevents the single node with the most
 > CPU power from generating and retaining the majority of the BitCoins?
 > If every node is working independently of all others, if one is
 > significantly more powerful than the others, isn't it probable that this
 > node will reach the proper conclusion before other nodes?
It's not like a race where if one car is twice as fast, it'll always
win.  It's an SHA-256 that takes less than a microsecond, and each guess
has an independent chance of success.  Each computer's chance of finding
a hash collision is linearly proportional to it's CPU power.  A computer
that's half as fast would get half as many coins.

**About Satoshi's ideals**

1. The existing Visa credit card network processes about 15 million 
Internet purchases per day worldwide.  Bitcoin can already scale much 
larger than that with existing hardware for a fraction of the cost.

2. We have to trust them with
our privacy, trust them not to let identity thieves drain our accounts.
Their massive overhead costs make micropayments impossible.

3. A generation ago, multi-user time-sharing computer systems had a similar
problem.  Before strong encryption, users had to rely on password
protection to secure their files, placing trust in the system
administrator to keep their information private.  Privacy could always
be overridden by the admin based on his judgment call weighing the
principle of privacy against other concerns, or at the behest of his
superiors.  Then strong encryption became available to the masses, and
trust was no longer required.  Data could be secured in a way that was
physically impossible for others to access, no matter for what reason,
no matter how good the excuse, no matter what.

It's time we had the same thing for money.  With e-currency based on
cryptographic proof, without the need to trust a third party middleman,
money can be secure and transactions effortless.

4. One of the fundamental building blocks for such a system is digital
signatures.  A digital coin contains the public key of its owner.  To
transfer it, the owner signs the coin together with the public key of
the next owner.  Anyone can check the signatures to verify the chain of
ownership.  It works well to secure ownership, but leaves one big
problem unsolved: double-spending.  Any owner could try to re-spend an
already spent coin by signing it again to another owner.  The usual
solution is for a trusted company with a central database to check for
double-spending, but that just gets back to the trust model.  In its
central position, the company can override the users, and the fees
needed to support the company make micropayments impractical.

5. Of course, the biggest difference is the lack of a central server.  That
was the Achilles heel of Chaumian systems; when the central company shut
down, so did the currency.

### Section summary
[Date: Sun, 03 May 2009 23:32:26 +0100] [Email #3]
Malmi wanted to write Bitcoin's FAQ, Satoshi provided him with some
questions he previously answered.

Q: A user asked about how many coins are there and how did Satoshi manage
inflation schedule?
A: Satoshi said it was an educated guess, he only distributed 21 mil coins
just so the bitcoin keeps its value as the chain scales.

Q: A user asked why all transfer from Satoshi showed up as "From: unknown"
A: All transactions in Bitcoin are always from "unknown".

Q: A user asked what if Bitcoin blow up and there are millions of
transactions, how does the chain scale?
A: Bitcoin's blockchain can scale much more than Visa with existed hardware
for a fraction of the cost. By Moore's Law, this problem can be solved 
eventually. Satoshi believed the power of computers would go ahead of the scalability
of Bitcoin's chain, this means there will be any problems like computers
couldn't catch up with the scaling of the chain.

Q: What about inflation? Computers will get stronger, hence it creates 
more Bitcoins eventually?
A: The inflation of Bitcoin was already handled, as the computer gets
stronger, the difficulty to solve the puzzles increases to compensate.

Q: Every nodes work independently, how to prevent one node from generating
and retaining the majority of the Bitcoins?
A: Each computer's chance of finding a hash collision is linearly 
proportional to it's CPU power.  A computer that's half as fast would get 
half as many coins. 

**About Satoshi's ideals**

1. He believed even when we use a lot of energy to operate the chain, it
wouldn't be as much as traditional banking systems.

2. Current massive overhead cost of banking systems make micropayments impposible.

3. Satoshi believed encryption protection is the future. No rely on
centralized system where administrator can overidden your data. Data can 
be secured in a way that is impossible for others to access.

4. To detect double-speding, users have to entrust a TTP to check it for them,
which leads back to the trust model. Bitcoin uses P2P network which works
like a distrubuted time stamp server, stamping the first transaction spend
a coin, so double spender couldn't double-spend later on.

5. DigiCash once published a cryptocurrency in 1989, but the fatal weakness 
was that they used a centralized system, when the company shutdown, so 
did the currency.

***  
### Glossary:
- DigiCash: Digicash was an early form of electronic money created by David Chaum in the 1980s, allowing users to make secure and anonymous transactions over the internet. It utilized cryptographic techniques to provide privacy and authenticity in digital payments.
- Fiat money: Fiat money is currency that a government has declared to be legal tender, not backed by a physical commodity like gold or silver. Its value is based on the trust and confidence of the people using it and the stability of the issuing government.
