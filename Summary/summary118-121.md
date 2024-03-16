# Summary 118 -\> 121  
  
### Context:  
  
* Email \#118:  
I just made a 10,000bc transaction from one account to another, but it  
ended up sending 10,000.20bc. Any idea why that could be?  
  
* Email \#119:  
There's a transaction fee of 0.01 per KB after the first 1KB for
oversized transactions. The first 1KB is free, small transactions are
typically 250 bytes. Doubleclick on the transaction. Think of it like
postage by weight.  
The solution is an extra dialog when sending, something like "This is an
oversized transaction and requires a transaction fee of 0.20bc. Is this
OK?" (is that text good enough or any improvements?) I have the code
already, I'll put it in.  
Then we wouldn't have to explain the 10,000.20bc transaction, but may
still have to explain who the transaction fee goes to.  
  
* Email \#120:  
Is there no transaction fee then, if you send the same amount in
multiple small packages?  
Sounds fine.  
Where should it go btw? Here it went to the receiver along with all the
other coins. Transaction screenshot attached.  
  
* Email \#121:  
True. I suppose the dialog could make it worse by giving people a chance
to experiment with breaking it up.  
I'm making some changes. The largest free transaction will be 60KB, or
about 27,000bc if made of 50bc inputs. I hope that's high enough that
the transaction fee should rarely ever come up. v0.2 nodes will take
free transactions until the block size is over 200K, with priority
given to smaller transactions.  
It's best if you don't talk about this transaction fee stuff in
public.  
It's there for flood control. We don't want to give anyone any ideas.  
You found an infrequent bug in CreateTransaction. It wrote the
transaction for 10000.20 with a fee of 0.22. If you look at the
transaction on the sender's side, it'll be a debit 10000.42 with
transaction fee 0.22. The bug was that it had to make a rare third pass
on calculating the fee, and incorrectly added the first pass' fee to the
amount being sent. Will fix.  
***  
### Summary:  
[Wed, 02 Dec 2009 16:26:42 +0200] [Email #118]:  
Malmi sent 10,000bc to another account but it actually sent
10,000.20bc?? and Malmi wondered why?  
[Wed, 02 Dec 2009 17:47:48 +0000] [Email #119]:  
Satoshi explains that 0.20bc is the transaction gas fee but still has to
explain who will this gas fee go to?  
[Thu, 03 Dec 2009 09:46:50 +0200] [Email #120]:  
Malmi wondered where it should go? Here it went to the recipient along
with all the other coins. And Malmi wondered how to send the same amount
of money but divided into many small packages, would there be a fee?  
[Fri, 04 Dec 2009 04:24:41 +0000] [Email #121]:  
Satoshi's response to the issue of split transactions: Satoshi is making
some changes  
=\> The largest free transaction is 60KB or 27,000bc if done from 50bc
input.  
=\> Perform free transactions until block size \> 200K and will give
priority to smaller transactions.  
Satoshi replied: It's best not to talk publicly about this transaction
fee issue.  
It is there for flood control. We don't want to give anyone any ideas.(Flood control ensures that systems and services are not overloaded or attacked).  
***    
### Conclusion:  
In summary, this email focuses on resolving some technical issues such as transaction costs and directions to fix errors related to transaction fees.
