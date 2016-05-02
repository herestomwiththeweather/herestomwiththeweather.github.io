---
layout: post
title:  "Ethereum Contract Update"
date:   2016-02-27 12:22:00 -0500
tags: ethereum
---
Getting up and running with [Ethereum](https://www.ethereum.org) on Ubuntu Trusty has been fairly painless thanks to the [truffle](https://github.com/ConsenSys/truffle) development environment. The [HOWTO: Building Ethereum Apps With Truffle](https://www.youtube.com/watch?v=GPP6uAq15d8) is a good introduction but a few details, like directory layout, have changed since then.


At [the most recent Austin Ethereum meetup](http://www.meetup.com/Austin-Ethereum-Meetup/events/228452105) we had a conversation about how to handle the contingency of when you need to update your “[contract](https://medium.com/@ConsenSys/unpacking-the-term-smart-contract-e63238f7db65#.q6szg6xr5)” which has already been deployed to the blockchain. Using “contract” as the term for the main class makes the discussion of updating such code more difficult than it should be. Code often needs to be updated for new features or fixing bugs.


For instance, it is important to check (as mentioned in [How to Write Safe Smart Contracts](https://chriseth.github.io/notes/talks/safe_solidity/#/7) and [Address types](http://solidity.readthedocs.org/en/latest/types.html#address)) the return value of send().


The “King of the Ether Throne” [post-mortem](http://www.kingoftheether.com/postmortem.html) shares the unenviable experience of not doing this check. The offending solidity code was

	currentMonarch.etherAddress.send(compensation);

In this particular case, send() was failing because the destination address was a contract address (an Ethereum Mist “contract-based wallet”) and the default 2300 gas supplied by send() was insufficient for the destination contract to run. So, their [fix](https://github.com/kieranelby/KingOfTheEtherThrone/commit/120f5a20b17600154ecb11ca120ec0dc296f66d5#diff-05a7e7e015bed68b1fcc05cd0b5c680dR209) was to define a new sendWithGas() function which allows extra gas to be sent and returns a boolean like send():

	//Unfortunately destination.send() only includes a stipend of 2300 gas, which
	//isn't enough to send ether to some wallet contracts - use this to add more.

	function sendWithGas(address destination, uint256 value, uint256 extraGasAmt) internal returns (bool)
	{
	  return destination.call.vaule(value).gas(extraGasAmt)();
	}

Now, the sendWithGas() return value is stored in a payment status and if that status is failure, the operation is cancelled with prior state restored.

Now, they just need to update their contract!
