---
title: Mining - Threat Model and Equilibrium Analysis
show_author: true
comments: true
---

> In our future world of Payment Channels, there is really no way that even a 100% pool can attack users. We will always need to be ready to change Bitcoin's PoW algorithm.

This is a written elaboration of [this video](https://www.youtube.com/watch?v=91TufmffIDg&list=PLw8-6ARlyVciNjgS_NFhAu-qt7HPf_dtg&index=2) from last summer.

### Motivation

#### New Knowledge

Previously, [I argued](http://www.truthcoin.info/blog/mirage-miner-centralization/) that the concept of "miner centralization" is unhelpful (and possibly meaningless).

With this post, I'm going to try to *provide* something that I think is more useful. In other words, the previous post attempted to *destruct* an idea of "miner centralization", this post attempts to *construct* something useful (on the same topic).

#### Sidechain Criticism

Secondly, there are some people who complain about my sidechains work (not to my face of course, only in whispers to their already-friends). After slowly and steadfastly eliminating all possible realms of concern, category by category, I asked my critics to reveal their  retreated


#### "Effectively Centralized"

I have a third distinct goal -- to demonstrate that "Bitcoin works".

Most of Bitcoin "just works" already, as is obvious. Signatures work, hash functions work, the data is displayed to the user, transactions are processed, et cetera.

However, "mining" can't easily be shown to "work".

I believe that this is due to the "slow" life-cycle of mining. For digital signatures, the life-cycle (generating the keys, signing a transaction, checking the signature) is "fast". We can perform the experiment many times. Each time, the signature works as advertised. Same goes for hash functions, database calls, CPU usage, the font, et cetera. These subsystems can all be tested a hundred times, and the results will always generalize to the future (because nothing about them changes over time). Mining, however, appears to be is constantly changing. We can not demonstrate that it will work in the future, merely because it works today.

It is true that "development" is in the same boat. However, any audience familiar with [1] open source, [2] soft forks, and [3] this [intuitive upgrade strategy](http://www.truthcoin.info/blog/against-the-hard-fork/#the-game-theory-of-bitcoin-upgrades), will probably be immune to nagging of this kind. Plus, the software has *already* been written...so what are you complaining about? That someone, somewhere, could at some future point write some evil software and trick you into installing it? Welcome to the internet?

So "mining" is where the criticism lands (excluding goal-related criticism, such as [the fixed money supply](http://www.truthcoin.info/blog/deflation-the-last-word/)). And this criticism fuels interest in proof-of-stake and other mining "alternatives".

My point of view [is that this criticism is unfounded](https://bitcoinmagazine.com/articles/problems-associated-with-bitcoin-mining-centralization-may-be-overstated-1474917259). When it comes from the ignorant, we can simply ignore it. However, it comes also, on occasion, from our most senior core developers. My opinion is that, at best, this is overzealous paranoia on their part, as they are unduly distressed over something which is out of their control. At worse it represents a childish need to be noticed and recognized as an authority or expert -- a kind of subconscious racketeering.


## Mining Threat Model

> What can miners do to attack users?

Threat models are important. Dr. Back linked to [this thread by Mike Hearn](https://groups.google.com/forum/m/#!topic/bitcoin-xt/zbPwfDf7UoQ), cited it as a "useful thread" and [reproduced it in full](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2015-September/010978.html) on the bitcoin-dev mailing list so that it "may reach a wider audience".

Excerpt: "Establishing a threat model is an important part of any security engineering project. In the early days of secure computing, ...usability, performance ...were sacrificed to try and defend against absurd or very unlikely threats just because someone identified one ...the resulting product sucked and nobody used it, thus protecting people from no threats at all ...PGP is a good example of this problem."

I would like to **apply the "threat model" concept to Bitcoin mining** specifically, instead of Bitcoin as a whole. While Bitcoin tends to be many things to many people, mining is an area where there is wide agreement over what should be accomplished: miners should [1] include transactions in blocks (only excluding them if they are invalid or if they pay a sufficient fee/kb), [2] pursue their long-term self-interest in a docile and transparent way (ie, 'no surprises'), [3] compete honestly (ie, on merit), and [4] maintain their own existence (so that the Bitcoin network doesn't stall out).

The threat model will consider two classes of miner behavior:

Class I: What miners are capable of doing.
Class II: What miners are motivated to do.

( Motivation is a reliable force -- the laws of incentive are comparable, in strength, to the laws of physics. Just look at any highway, and you'll see that the cars are snapped into their lanes, as if the thin lines of white paint were walls of concrete a mile high. )

### 0. Nodes and Miners

> The *validation* of blocks is the network's top priority, and should concern all Bitcoiners. However, the *mining* of blocks is none of the network's business, and should concern no Bitcoiners.

The threat model is **independent of full node costs** (ie, [decentralization](http://www.truthcoin.info/blog/measuring-decentralization/)). Specifically, the model assumes that every user is running a node (or else, each user is able to start one up). That certainly may not be the case, but those variations belong to a different threat model and a different day. This model defines "the network" as whoever is running a full node, and works from there.

#### Are They Really Separable?

Let me clarify my view on the connection between [1] an ASIC chip (ie, "the thing that miners") and [2] the software which administrates this chip (ie, the software that determines "what is mined"). Many believe that each miner must run a full node, else something terrible will happen. But I think that this belief cannot be correct.

The security model for Bitcoin is, unambiguously, that *users* run full nodes in order to validate payments. The full node of User A, protects him against the fraudulent activity of Users B through Z (including the fraudulent activities of miners "M" and "N"). Because of A's full node, A is robust to any mistakes made by B-Z.

Then, the requirement that miners run full nodes is at best redundant. A is already immune to any mistakes made by M and N. So there is no inherent requirement that a node be present to supervise an ASIC's activities.

Quite the reverse...

#### Capitalism and Efficiency

Since no node-requirement exists, it is really none of A's business what software Miner M and Miner N choose to run (or, chose *not* to run).

For example, here is [an article on ASIC-design from last year](https://www.cryptocoinsnews.com/approximate-hardware-bigger-bitcoin-mining-profits/), about ["Approximate Hardware"](http://rakeshk.crhc.illinois.edu/dac_16_cam.pdf), stating "a processor that needs to guarantee correct operation 100 percent of the time sometimes consumes almost two times more power than a processor you're asking to be correct only 99 percent of the time". The article concluded that new designs could increase mining ROI by a staggering 30%, by allowing for some occasional miscalculations.

Regular users are entirely unaware of these miscalculations, and are completely unaffected by them.

In fact, regular users can *never* be affected by a miner's miscalculations. Every thing a miner does is "hard to generate, easy to verify". The merkle tree, the construction of the header, the checking of the header's hash, and the verification that the hash value meets the difficulty requirement. They are all near-instantaneous.

The only slow operation is the validation of each transaction (specifically, signature-checking), but miners have no reason to use "approximate" computation here -- they cannot speed the process up, because it is limited by the arrival of new valid txns from users. Secondly, these computations have nothing to do with the header's proof-of-work and so there is no performance incentive.

#### Improvements in Pre-consensus Can Maximize the Modularity of PoW

In addition, it is my belief that, in the future, Bitcoin may have [1] better propagation technology, and [2] a more "patient" demand landscape. As a result, there will be no uncertainty whatsoever about a block's contents. For example, by the time block "35" is found, the network will already know, not just the transactions of 35, but also the transactions of block 36. In fact, the network will know exactly which txns will be in blocks 37, 38, and 39. Any ambiguities (including any mistakes, bluffs, attacks, feints) will be occurring far in the network's future, and will be resolved before the header gets any attention from the ASICs. Therefore, "miscalculations" will be unable to intersect with the double-sha256 computation that we today refer to as "mining".

But let us return to the present level of technology.

#### Miners Will Never Voluntarily Mine an Invalid Block

In all cases, miners get to choose *what* they mine. The mining-process may be imperfect, or inherently uncertain, but the miner can choose what *goes in* to the mining-process. They can either put an InvalidBlock "in", or put a ValidBlock "in". While a lazy miner may choose *not to learn* whether his block is a ValidBlock or an InvalidBlock, they are equally heavy, when it comes to hauling them over to the mining machine and dropping them in.

The mining process itself is costly, and this cost is also independent of the Valid-ness of the block. Therefore, if a miner inserts an invalid block, and *solves* the block, then this is, in fact, immediately costly to the miner! He has, in principle incurred an opportunity cost, as if the block were valid he could collect BTC from it.

An invalid block is costly right off the bat. Therefore, if "invalid mining" is ever to be incentive-compatible, an attacking miner will have to find a user for the InvalidBlock. The attack must have a marginal revenue which is greater than its marginal cost (this cost being equal to the value of each block).

What might an invalid block be useful for?

At best, the block can be used to temporarily distract or confuse some network participants. If our attacker has 51% of the hashrate, it will take him (on average) ~20 minutes to find another block. Whatever his intentions (to build atop the invalid block, or on top of its [valid] parent while other are distracted), it is inconceivable that the distraction could last long enough for anything significant to happen.

<!--

#### Hard to Generate, Easy to Verify

Perhaps we can combine the previous two sections to imagine a worst-case scenario where a miner has an incentive to accidentally mine invalid blocks, and then broadcast them.

Instead of requiring less power, a worst-case "approximate miner" could work ~twice as fast but have ~49% false positives. These "almost correct" false-positive blocks would meet the proof of work requirement, but be invalid. Once a miner finds th




Putting on a very large "conspiracy hat", we might then assume that miners could *deliberately construct an invalid block* and then use this software, hoping that it 

Since they meet the proof of work requirement, nodes would have to check the block's contents for accuracy, which [would be costly](http://www.truthcoin.info/blog/blockspace-demand/#cost-of-security). Theoretically, this would be a misaligned incentive, because individual miners would find this profitable, but they could collectively be jamming the network (or, each other) with hard-to-verify blocks.

This cannot be a concern of ours because of the way blocks are computed. The stage where transactions are assembled into a Merkle tree is already fast, and there is very little to be gained by making it faster. The stage where a block's header is hashed repeatedly to find a winning nonce (and thus mine the block), is the stage where speed is desired. Since the node software is *not* running on "approximate hardware", it will check this second stage without errors. As everyone who is familiar with Bitcoin knows, this second stage is merely one double-sha256 computation of an 80-byte header.



However, in do so, the miner incurs an opportunity cost.



This scenario requires us to assume that miners are malicious.

Even in this worst-case scenario, the miner would need to be deliberately inserting invalid data into blocks. This is because there is no need to "speed up"

After all, "the computation" is a series of SHA hashes (to roll up the block into a Merkle tree), and then finally
~nah, it is fine because they wouldn't garbage in.

On top of that, we assume that the miner is willfully malicious, and deliberately inserts invalid data into blocks
..


Solved with my preconsensus.
~~~

This would strip nodes of much of their DoS protection, and they would need to wait for many confirmations.


For a user who waits for 6 confirmations this does nothing at all, because......

 This would be no different from any other DoS attack, and A would respond in the usual way, by refusing connections from peers who give him bad blocks. Moreover, Satoshi's design fights back the tide of DoS, by requiring users to wait ~6 confirmation 

-->

#### In Praise of Node-less Miners

Ultimately, the simple truth is this: the miner is paid if he finds a valid block. How he finds this block, is his business. (How we validate it, is our business.) If a miner chooses to take a riskier, luck-based approach, then he bears the costs and benefits of that decision, just as he would if he cut corners on cooling equipment, or if he hired cheaper-but-less-reputable employees.

In my opinion, this principle applies equally to all "validation costs". Miners can minimize these costs in any way they deem appropriate. And this is exactly what they do, every day. Many developers today complain about SPV validation, but it is in principle no different from "Approximate Hardware" -- it is an efficient way of finding a valid block. The fact that it sometimes might find an *invalid* block, is the miner's problem, not ours. Miners are the ones [who stand to lose their mining-revenues](https://cointelegraph.com/news/miners-lost-over-50000-from-the-bitcoin-hardfork-last-weekend) if their blocks turn out to be invalid.

In fact, full-node-hardliners such as Peter Todd should *praise* the broadcasting of invalid blocks, not condemn it, as the presence of invalid blocks (and -really- nothing else) is precisely what incentivizes users to run full nodes in the first place. Here is [a bitcoin-dev email](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2015-August/010343.html) where PT espouses precisely this view (that the blockchain should contain some invalid data, such that users are forced to check the validity for themselves).

#### Implications of "Segregated ASICs"

The most direct implication is that the ASIC equipment can be hidden. The mining-critical information is on the order of bytes, and it can therefore be snaked around the internet through TOR and VPNs before arriving at the actual ASIC chip, Once a block is found, its header can also take a brief secrecy tour before being revealed to the world.

This will be explored further in the equilibrium analysis which follows the threat model. Because the two are connected, it is occasionally necessary to introduce a concept in one section and justify it in the other.

#### Scope of this Essay

In conclusion, this post is about a miner threat model which is *not* a node threat model. 

Many, including Dr. Back, [have claimed](https://0bin.net/paste/8YeL12K5CwP26YUP#kSSLpZ2+PC9RqgcbiP0-bYbDhIHAMRCB3t2CpHkxokQ) that it is bad if nodes are burdensome to run. With expensive nodes, users have only *expensive* recourse if these nodes come under pressure from violence-monopolies (such as governments or mafias). In this scenario, Bitcoin loses its distinctive "peer to peer" property, as someone is now a privileged non-peer.

Dr. Back's concern is a perfect example of something which is *not in the miner threat model*, because it is fully **dependent** on node costs. This model is **independent** of node costs. The threats I describe in this post will apply, whether nodes are expensive or cheap. And they will continue to apply if node-costs increase or decrease.


### 1. Setup

Let us consider a worst-case scenario, where:

* There is a single mining pool.
* Russian/Chinese Gov't / Evil Corporations / Mafia / NSA / etc are watching the Uni-Pool carefully.
* Miners trust the Pool to manage the blockchain, and run minimal software ("civilian" full nodes), to make sure that they are getting paid for their Hashing work.

However, I will assume that the physical equipment is hidden, and that there are no disparities in ASIC chip-efficiency.

These assumptions result from the equilibrium analysis, which follows this section.


### 2. The Adversary's Abilities

First we will look at what miners "can" do (Class I) to users.

A mining pool has, by my count, only two malicious powers:

1. Re-Write ("51% Attack")  -- directing Hashers to conduct a huge rewrite of the chain. 
2. Non-Write ("Censorship") -- preventing data from getting into the blockchain.


### 3. Cost of Attack

Each attack is costly -- it impoverishes the miners, forcing them to pay an [opportunity cost](https://en.wikipedia.org/wiki/Opportunity_cost) equal to their forgone transaction fees. Obviously, for the Non-Write attacks, each forsaken txn has fees which could have been paid to miners. For the Re-Write attacks, notice that the uni-pool earns tx-fees for each *new* block it mines, but none for any *old* blocks. If the uni-pool orphans itself, it will have mined two blocks but collected fees only once (if it instead mined two new blocks, it would have collected fees twice).

( This assumes that each block will be constructed to maximize total fees, [for which I have argued in the past](https://www.youtube.com/watch?v=YErLEuOi3xU&list=PLw8-6ARlyVciNjgS_NFhAu-qt7HPf_dtg&index=4). )

### 4. Baseline (No Attack-Benefits)

If attacking has no benefits, the uni-pool never has a motivation to attack. This is a complete victory, for everything of Class II.

On top of that, it is also a complete victory for all of Class I, if both of the following are true:

* Anyone can create a new pool.
* Miners will all always switch to whichever pool maximizes their revenue.

If these conditions are met, the pool is simply unable to get away with any non-revenue-maximizing activities.

And both seem quite achievable. The first condition can be met with [some open-source pool management software](https://www.google.com/search?q=open+source+bitcoin+mining+pool+software&ie=utf-8&oe=utf-8). The second is more complex -- the biggest challenge would be the [collective action problem](https://www.britannica.com/topic/collective-action-problem-1917157) inherent to coordinating the switch to the new pool. We can further break this step into three: [1] recognize that malfeasance is taking place, [2] choose a new pool, [3] choose a time to switch.

Of these sub-requirements, the second and third are trivial. Even the first sub-requirement is very solvable. For Re-Write problems (ie, a large chain reorganization), there is no ambiguity whatsoever -- this misbehavior will be noticed, by everyone. Miners will not be happy about orphaning themselves, and will have a reasons to switch pools. For Non-Write ("censorship") problems, the victims can prove that they are under attack. For example, full nodes might collect "suspicious transactions" (which should have been mined but mysteriously aren't), and as these pile up, it will become obvious that something is clearly amiss (and that there is money to be made by ditching the uni-pool for a new one).

### 5. Benefits to an Attacking Miner

The previous section assumed that the miner didn't benefit from the attack.

Of course, that isn't true. Certainly, with Re-Write attacks, the attack can unspend all of the money he has recently spent, and purchase things without paying for them. This threat was highlighted by Satoshi in his whitepaper:

![satoshi-incentive](/images/satoshi-one-miner.png)

...and we will discuss it two sections from now.

For the Non-Write attacks, we can further divide into three flavors:

2a. Freeze Funds -- barring a specific txn (or class of txns) from entering the chain.
2b. Denial of Service -- barring *all* txns from the chain (this puts the Bitcoin network "on strike").
2c. LN Corruption -- denying justice to a user who is being robbed by a LN-counterparty.

An attacker might benefit from 2c by conspiring with the robber. An attacker cannot directly benefit from 2a or 2b, outside of a desire to cause mayhem. In a future world of Schnorr signatures, 2a will likely be outright impossible.

### 6. Coercing the Miners

It is true that an adversary may pressure the uni-pool to block transactions, or apply policy to them arbitrarily. For example, the adversary may wish to "freeze" an individual's account, holding it hostage such that he cannot spend money until he meets some arbitrary criteria. Criminals may simply hold an account ransom, such that no money can be spent until the victim agrees to pay some bribe.

As I've previously argued, this is unlikely to work against the pool, because the pools will compete strongly on efficiency, and censorship is inherently inefficient (as it leaves transaction fees on the table).

An adversary may put pressure on the ASIC chips themselves. I describe why this is infeasible in two later sections: the "equilibrium analysis", and "evolving to extinction". But for now I will simply remark that, if someone can coerce an ASIC-owner to force the ASICs to behave a certain way, then, as far as I'm concerned, the ASIC chips have already been stolen. The "coercer" is the de facto owner of the ASIC equipment (not whoever originally paid for it). 
Then, the ASIC-owner (whoever that may be) may want to do something to us, that we don't like. I think that they can only do four things:

1. Harass individual Bitcoiners by censoring their payments.
2. Mine empty blocks, to cause mayhem.
3. Steal payments from a LN channel by allowing a counterparty to broadcast a previously-invalidated transaction state. 
4. Steal payments by rewriting the chain.


### 7. Our Best Responses

What can we do, if we are attacked in any of these four ways?

1. Increase transaction privacy.
2. Use payment channels.
3. Use "healthy" channels (rebalance to avoid one-sidedness, long custodial periods).
4. Change the proof-of-work algorithm.

The first two are quite straightforward. Privacy protects individual users from being targeted, and payment channels -once open- allow users to transact without miner's knowledge or permission.

This emphasis on channels exacerbates the threat of channel-theft.

However, users can discourage channel-theft by making it expensive. Like all censorship, the cost to the miner is the foregone tx-fees, and users can cheaply amplify channel tx-fees. This is because lightning-channels are designed such that, if your counterparty attempts to defraud you, you [1] get some of your money back immediately, and [2] can immediately broadcast a txn claiming all of your counterparty's money.

Therefore, the strategy is simple: always require your counterparty to have lots of their money in the channel. A channel might start 50%-50% or 60%-40%, but as it gets nearer to 90%-10% or 95%-5%, users should rebalance the channel to keep it as close to 50%-50% as they can. This allows the victim to use the attacker's money to pay a large transaction fee (more than the attacker could possibly steal from the victim, and more than the attacker could offer to a conspiring miner).

For example, imagine a channel starts (you: 50 BTC, they: 50 BTC), and progresses to (25, 75), and then (90, 10). Imagine then that an attacker attempts to defraud you by broadcasting the (25, 75) state. You immediately get 25 BTC of your own money, and you can also get 75 BTC if you can broadcast a single transaction within the next 1000 blocks. Since the transaction is worth 75 to you if broadcast, and zero otherwise, you (or [someone else](https://www.google.com/search?q=Unlinkable+outsourced+channel+monitoring++Tadge+Dryja&ie=utf-8&oe=utf-8)) will pay a tx fee of up to 75 BTC to get this transaction included in a block. You can therefore outbid your attacker, who is only earning 65 from this maneuver. In contrast, if you'd allowed the state to drift through (100,0) before reaching (25,75), then the attacker would have nothing to lose by broadcasting (100,0). Therefore, you should *not* allow this.

Secondly, you can insist on a long custodial period. Channel-theft requires a miner to retain total censorship-dominance for many consecutive blocks. If we are not in a uni-pool situation, this all-but-guarantees that channel theft will fail immediately (on the grounds that it will almost certainly fail eventually, and that whichever miner who causes it to fail first will get more money). Even in a uni-pool situation, a long custodial period can help tremendously (on the grounds that a victim merely needs to outlast the replacement of this inefficient pool).

I will deal with the PoW-algo change in the final section of this essay "evolving to extinction".

### 8. Conclusion

In the above sections, I have assumed that all of the miners were joined together in a single pool. Even with this assumption in place, I have searched for ways in which users can realistically damage users, and not found any.

And this theoretical result mirrors reality, as there have been no instances of any of the four attacks (censorship, miner strike, channel theft, or 6+ block rewrite). Miners have historically [stolen some money](https://bitcointalk.org/index.php?topic=327767.0), but these instances support rather than refute my analysis, as they are all instances of miners aggressively maximizing their revenue...while playing entirely by the rules of Satoshi's protocol.

However, a new question remains: will things always be as safe as they are today?

To answer that, we will need to guess at what mining will be like once everything stops improving and changing. If mining is OK today, and mining is also OK in the far future, then we might conjecture that a line connecting the two points will only travel through "OK" territory.


## Miner Equilibrium Analysis

> What will mining look like in the future? Where is it going?

First, I'll separate "mining" into 4 sub-processes:

1. Hashing -- the rapid, industrial-scale calculation of 2x-sha256 hashes.
2. Logistics -- the shipping of information from users, to miners, and back to users.
3. Exchange -- selling new BTC in order to reimburse mining costs.
4. Income Smoothing -- reducing the uncertainty in revenue arrival-time.

Today, the 'mining pools' perform most of sub-process #2, and all of sub-process #4. ASICs chips perform sub-process #1, and individual miners perform #3. 

Let's get exchange out of the way, because it is quite simple.

### 1. (#3) Exchange

Miners certainly compete on their ability to "make the most" of their newly-minted BTC.

For example, some miners glean extra value from Chinese capital controls, by using their Chinese currency (which is trapped in China) to purchase power, in order to mine BTC which they later take out of the country to sell for other international currencies.

It is also known that some buyers will pay a premium for large, private, over-the-counter purchases. Miners often facilitate these transactions, using their newly-minted BTC.

Miners will continue to use these and other strategies. Some, such as the first, may encourage power to be sourced from a certain jurisdiction or purchased with a certain currency. Others, such as the second, merely encourage miners to practice another skill (or diversify further, into intermediaries).

### 2. (#1) Hashing

ASIC designing is a quest for efficiency -- turning dollars into hashes as cheaply as possible.

Eventually, technological knowledge -of all kinds- will saturate. Everyone will know the best techniques for building chips, for cooling the chips, the cheapest sources of power and so forth.

Since very much has been said about this already, my only contribution here will be to agree that hashing will be co-located with cheap sources of power, and that these cheap sources of power will be geographically distributed.

This is largely for physics reasons: the transportation of energy is not free, and there are certain terrain features which are conducive to the production of energy (geothermal, hydroelectric, solar). As a result, 'Hashing' will probably eventually consist of many chips, all of uniform quality, co-located with cheap sources of electrical power.

It happens to be the case that ASICs produce waste heat, and heat radiates in all directions. If we imagine a cube of ASICs, 3x3x3, then the 8 ASICs on the corners of the cube will cool fastest, followed by the 12 which surround each 'face' of the cube (where the dots are, on a pair of dice), followed by the 6 in the center of each face, with the single remaining center-ASIC being cooked like an oven. This effect diminishes if the ASICs are physically separated from each other. While it is often fashionable to point this out, the effect operates on very small scales (a few meters), and is unlikely to affect the risk of confiscation.


### 3. (#2) Logistics, ie "Info-Shipping"

Logistics is quite a puzzle. I'm not sure I've completely solved it, but I will share with you my results so far.

In order to mine, a miner needs two pieces of information that he cannot himself create:

1. hashPrevBlock -- the hash of the blockheader corresponding to the block which [1] is valid and [2] is at the end of the heaviest-work chain of blocks.
2. hashMerkleRoot -- the hash of a set of transactions which are [1] valid and [2] pay appropriate tx-fees.

Since miners both buy and sell this information, to other miners, we are in a highly strategic environment which I will refer to as **Broadcast Strategy**.

A naive miner would: connect to the network, download and validate all blocks, download and validate all transactions, search for a block, and then, if a block is found, broadcast the block immediately to the network.

However, over Bitcoin's eight years, the broadcast strategies have grown to be increasingly sophisticated. Currently:

* Hashers outsource their validation work to pool operators.
* Pool operators frequently take risks, by:
* * ...assuming PoW-sufficient blocks are also valid-blocks, and switching to them immediately.
* * ...assuming certain transactions won't be double-spends, because they are too undesireable (and including them before checking the prevBlock).
* Pool operators also connect to rival pool-operators, to 'spy' on them and avoid falling behind.

In fact, no one knows for sure exactly how the miners ship their information back and forth. There was once a single mempool that was shared by ~75% of the network hashrate. Perhaps there still is.

Let's talk about two of the most important Broadcast Strategies. These are instances in which you do *not* broadcast immediately, in what was originally called 'Block Withholding'.

#### Block Withholding Attack

However, the current 'block withholding attack' is named after [the phenomenon](https://arxiv.org/abs/1402.1718) that is is in fact *directly profitable* to do [something a little bizarre](/images/bitcoin-mining-block-withholding.txt): join a mining pool, work for it honestly, submit the losing blocks, but then sabotage the pool by not relaying any winning blocks. [The bigger the pool, the greater your disproportionate profits](/images/block-withholding-attack-sim-2.xlsx).

And, most interestingly of all, you can [attack all open pools](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2015-December/012108.html), and it's just as wonderful for you as if you had attacked a single gigantic pool.

Perhaps we should rename it to the 'pool withholding attack', as it only works on pools.

In fact, it only works on open pools (pools where anyone can join). And, if you join a 'closed pool', it is unclear to me who actually 'owns' or 'controls' the ASIC hardware -- it seems that the closed-pool operator is a partial owner of some kind. Obviously, a solo miner can be said to be running a closed pool, consisting of only himself as the sole member.

So, joining an 'open pool' is costly, relative to joining a 'closed pool', or 'solo mining' (ie, mining without a pool, which could itself be considered as a closed pool with just one member).

Is it worth it? Clearly, most of today's mining is done in open pools. What are the benefits of today's open pools?

1. Convenience (ie, "specialization") -- Hashers can just focus on hashing, and point their hashpower to some destination.
2. Income Smoothing -- Hashers can be certain to avoid long periods of zero revenue.
3. Collective Barganing -- Hashers can pledge themselves to act as a group, giving their leader (the pool operator) more negotiating leverage.

My guess is that, in equilibrium, none of these three advantages will remain, let alone be attractive enough to outweigh the costs of the PBW-attack.

Peter Todd [proposed that we fix the attack](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2015-December/012046.html), which unfortunately requires a hard fork.

If the attack is not fixed (as I expect it won't be), then we will probably see a decline and disappearance of open mining pools. 

#### Selfish Mining

On the opposite of the naive 'broadcast immediately' strategy, would be an aggressive 'never broadcast' strategy. By not broadcasting, you deny other miners the ability to build on your blocks. This was originally referred to as a 51% attack, as the attack -so we thought- would only be effective if a miner had at least 51% of the hashrate.

However, it was noticed that one could refine the 'never broadcast' strategy, specifically to take advantage of those miners who were using the naive 'broadcast immediately' strategy. If the agent has sufficient hashpower ([about 30%](https://petertodd.org/2016/block-publication-incentives-for-miners#temporary-block-withholding)), then he can reliably 'juke' the 'broadcast immediately' miners, and accumulate enough statistical luck to eventually claim 100% of the block rewards.

This strategy came to be known as 'Selfish Mining' after a paper by Emin Gun Sirer and Ittay Eyal [with that phrase](http://hackingdistributed.com/2013/11/04/bitcoin-is-broken/).

The implication of Selfish Mining, is that the strategy='broadcast immediately' is [vulnerable to invasion by](https://en.wikipedia.org/wiki/Evolutionarily_stable_strategy) strategy='selfish mining'.

However, a peculiarity of the SM is that it is itself horrifically unstable if a large % of hashpower uses it. For example, if 100% of the network uses it, no one will ever see any blocks at all, as no miners will be publishing any. If three groups of 33% hashrate use it, no blocks will be seen until the remaining 1% 'broadcast immediately' miner finds a single block and publishes it. Then the network will explode forward as each of the three publishes their private chains. If two groups of 50% use SM, then not only does the network cease to exist (as no blocks are ever being published), but even if it could magically exist (say, blocks are magically and simultaneously published as soon as each of the two groups has 5 private blocks), then 'selfish mining' would actually be *worse* than 'brodcast immediately'. This is because there is no one for selfish mining to exploit (the profitability is the same, under both strategies); the revenue-variance, however, is much higher.

So, 'selfish mining' is good if many rivals are using 'broadcast immediately', but it is bad if many rivals are using 'selfish mining'.

There is no known way to discourage selfish mining, other than to force >70% of the hashrate to decline to use the strategy.

Now let's talk about a way to *force* someone to broadcast to you.

#### 'SPY' Mining

The technique of [spy mining](https://bitcoinmagazine.com/articles/why-bitcoin-mining-pools-aren-t-incentivized-to-broadcast-blocks-quickly-1475249510/) is one where pool operators connect to rival pools to "spy" on what they're up to. If you "spy", then it effectively forces everyone you spy against to use the 'broadcast immediately' strategy, at least to you.

This is done by waiting for the rival pool to command its ASICs to work on a new block header. Then, you just re-route that header to all of your own ASICs.

If all pools are open, then spy mining is a [dominant strategy](https://en.wikipedia.org/wiki/Strategic_dominance). The intuition is that you can choose to ignore the 'spied' information, if you need to.

( Incidentally, the only time you would choose to ignore this information is if you 'spied' that a rival found a "problematic block". A "problematic block" would be one that was unlikely to make it into the final chain, perhaps because it is very large and rivals are *not* spy-mining. Usually, you have a pure advantage if you spy-mine. If others are spy-mining, you are at a disadvantage if you don't spy. )

Not only is spying dominant, but it is probably also dominant to immediately mine on whatever header your rival pool is giving everyone. There may be complex strategies that pools use to smoke out spies, but they probably won't work, as the information is only flowing "one way" (from rival-pool to pool to ASIC) for the duration of the block search process (~10 minutes). Since attempts to 'smoke out spies' are costly to the rival pool operator (in wasted hashrate), and since they ultimately achieve nothing, it is probably safe to assume that any hashPrevBlock that the rival-pool-operator sends is genuine.

Of course, to prevent yourself (and everyone) from joining an invalid chain, you should also validate the block as quickly as possible. If you are unable to validate the block (within, say, 10 minutes), you should reject it.

Spy-mining is very difficult to prevent. The only known way to prevent this strategy is to close the pool **completely** -- ie, it only contains ASICs that you own (or that are owned by your perfectly-trusted group). Even then, it is conceivable that pools might simply *bribe* each other for on-demand access to the latest prevBlockHash.


### 4. (#4) Income Smoothing

When miners are asked "Why do you use a pool?" they all emphasize 'income smoothing' above all.

The prospect of going an unlucky week (or so) without *any* revenue coming in, is one that is extremely distressing to miners.

Interestingly, this concern is purely financial in nature. And the realm of theoretical finance does not even allow this to be a legitimate concern: because there is a perfect -1.0 correlation among miner block-finding, this would all be considered ["diversifiable risk"](http://www.businessdictionary.com/definition/diversifiable-risk.html). Miners would be advised to buy stock in each other, if they wanted to hedge against this risk.

Of course, miners can't usually buy stock in each other! And not every miner is a corporation! And we don't always know whom to buy stock in!

Nonetheless, it is interesting. I have started working on a 'smart contract' version of "miner insurance" which I think might allow miners to hedge their risk by betting that they themselves will *not* find a block. It's pretty complicated to work out.


### Synthesis

Firstly, it is [a straightforward application of the efficient markets hypothesis](/images/efficient-hasher-hypothesis.md) that miners will eventually be earning the lowest ROI possible, and paying themselves subsistence wages. (However, this is true for all industries, and the critical word is 'eventually'.)

Secondly, we know we know that hashing equipment will likely be co-located with cheap sources of electrical power.

We also know that spy-mining is hard to prevent. If unprevented, everyone will use the (very tame and desirable) 'broadcast immediately' strategy.

However, spy-mining requires open pools, which makes it vulnerable to block withholding. Perhaps the block-withholding attack will be fixed, or perhaps pools will become closed (but will still broadcast quickly). Or perhaps pools will have a fee structure that heavily discourages withholding.

We do not see any selfish mining in today's mining network. I feel that miners have actively chosen to avoid SM, as it leads to a road of tremendous headache for no net gain.

For example, if one user will [1] single-handedly purchases >30% of the hashrate, [2] closes his pool to prevent spy-mining, and [3] aggressively selfish mines until he is the only pool, he will lead the chain for a time. However, rival miners can guard against this by forming a closed pool of ~70% of the hashrate, and doing selfish-mining of their own.

Nonetheless, since there is a significant risk of uni-poolness, the threat analysis must contain that assumption.

( Which is how the analysis began, if you remember. )

Let's talk a little more about extreme situations where one agent controls all of the hashpower.

<!--

Government permits to mine. -- long run, no one will be able to afford these.

Peter Todd works for BTCC -- https://petertodd.org/2016/block-publication-incentives-for-miners

...

#### Good News

Fortunately, even though these conditions cannot be changed, they are actually relatively favorable to the defensive team.

Firstly, there is the basic principle of heat diffusion. This is kind of an "anti fixed cost", in that it is the opposite of reusable. Imagine that we have two groups of 27 ASICs. Group A has 27 ASICs, each in a different home or office. Group B has 27 ASICs arranged in a cube, 3x3x3. All of the ASICs are initially at room temperature. When they are activated, they will begin to produce waste heat. Group A will be slower to overheat than Group B, because A's ASICs can mix with the atmosphere in a full 360 degree radius. B's ASICs have problems -- the four corner-ASICs have the easiest time, followed by the perimeter non-corners; the six ASICs in the center of each of the outer squares (if the cube were a large die) have it quite bad and the single ASIC in the center is being cooked like an oven.

While the pizzeria could recapture the waste heat and use it for more pizzas, now the ASICs are giving off waste heat that warms nearby ASICs. This favors the ASICs being spread apart. Or else, it favors investment in cooling systems to compensate for this.

Secondly, 




~ "reusing" the atmospheric temperature.



~ would it be different if the government had to actually *build* the ASIC hardware.

-->



## Evolving to Extinction

> Why we need the "change the proof of work algorithm" card, and why we're always going to need it.

The title of this section is based on [Yudkowsky's essay of the same name](http://lesswrong.com/lw/l5/evolving_to_extinction/). In my opinion, EtE is one of the most important pieces of writing on the internet.

In applying it to Bitcoin, my observation is this: there is an entire category of problem which, in my view, cannot be solved algorithmically.

### The Problem: Specialization Leads to Fragility

Bitcoin is vulnerable in the following way:

1. The world might be "Bitcoin Friendly" or it might be "Bitcoin Hostile". To a Bitcoin miner, these worlds are quite different.
2. Since more configurations are possible in the "Bitcoin Friendly" world, the optimal mining configuration of that world will be more 'efficient' than the optimal mining configuration of the Hostile world.
3. If the world is Friendly for a time, mining will optimize and specialize for Friendly-world. 
4. *After mining is specialized for Friendly*, it will be reliant on the world remaining Friendly.
5. Anyone with the power to "change the world from Friendly to Hostile", can probably seize control of all of the Bitcoin Hashpower.

This is more or less what happened to Ancient Rome (according to some).

There is nothing we can do to change this aspect of reality. It is a consequence of the fact that the miner-elimination process lacks sophistication. The elimination process is not "intelligent" enough to comprehend, measure, or respond to the "E2E risk". If it were, Step 3 (above) would optimize for the possibility of the world becoming Hostile. Instead, it just counts some numbers and divides them.

### The Solution Must Involve Human Judgment

However, **we** can change *how we respond* to this aspect of reality. We, since we possess thinking brains, can use our sophisticated brains to comprehend, measure, and respond to the E2E risk.

The challenge is: what response could we make, that would be [1] strong enough to solve the problem, [2] simple enough to have minimal unforeseen consequences, [3] principled enough to be uncontroversial, and -above all- [4] *inherently connected* to this problem exclusively, such that a scattered crowd of leaderless Bitcoiners, seeking [common knowledge](https://en.wikipedia.org/wiki/Common_knowledge_%28logic%29) and [leaderless coordination](https://en.wikipedia.org/wiki/Focal_point_%28game_theory%29), would view the response as "what other people are most likely to conclude that we will conclude that we should respond with".

The problem is one where an adversary comes into sudden possession of the mining equipment. The solution, then, must be one which deprives the adversary of the mining equipment. It also must be some kind of temporary alternative to proof-of-work consensus, by definition, because in this attack scenario, the attacker controls the "work". (This is in direct contradiction to Satoshi's qualifier "As long as a majority of CPU power is controlled by nodes that are not cooperating to attack the network".)

The clear solution is to change the proof of work algorithm, and to warn users to wait for many confirmations until the algorithm-change process.

This [1] solves the problem, [2] is simple enough that [users can understand exactly what change was made](http://www.truthcoin.info/blog/against-the-hard-fork/#when-is-a0-) and how it will affect them, and [4] very tightly connected to the original problem. It is a "temporary alternative to PoW" for merely the briefest instant, rejecting PoW one time and then renewing our loyalty to it.

To satisfy requirement [3], it must be the case that we only undertake this action if there is a clear need. In other words, if someone attempts a large Re-Write of the chain (and seems to be succeeding).

To protect against channel-theft, I recommend setting the channel's custodial period as long as possible.


### Off-Path Reasoning and Equilibrium

Although our defense cannot be algorithmic, we can modify our behavior so that it includes some of the beneficial aspects of algorithms, namely 'transparency', 'predictability', and 'clarity'.

Firstly, to avoid mis-coordination, we could have a previously-defined sequential list of PoW algorithms, and this list could even ship with the latest version of Bitcoin Core. Then the required user-action would be as informationally-simple as possible: either press "the button", or don't. Pressing the button would move the user to the next PoW algorithm in the sequence.

Imagine a world where all of Bitcoin land could **magically commit** to behaving such that the phenomenon "a 20+ block Re-Write of the chain" would necessarily be met with the response "we all press the button".

What effect would this have on today's adversaries? What effect would it have on today's miners?

#### Making Miners Responsible for their own Problems

Obviously, the miners would want to avoid this doomsday scenario more than anyone. I think it possible that, despite a collective-action problem, some miners might react to [physical mining concentration](https://twitter.com/petertoddbtc/status/793877455639498752) by *hiding their own equipment* and making light investment in 'hidden mining facilities'. These would be relatively inefficient but could be ramped-up on short notice. The purpose of this would be to wait for an adversary to steal ASIC equipment, and then to react by this by turning on additional equipment while the adversary is attempting a Re-Write (after all, a two week reorganization will take about four weeks to complete, if an adversary has 51% of the hashrate, and 2.7 weeks if the adversary has 75%). These 'savior miners' would then [1] save the Bitcoin network from the attack, [2] save their investment from failing immediately, and [3] disable much of their competition (at least, for the duration of the attack).

#### Making Confiscation Attacks Futile

Most important of all is the effect this would have on the adversaries themselves. Since the attack ultimately can't succeed, there is less reason to attempt the attack in the first place. Instead of "mafia takes control of Bitcoin", the newspaper headline would read "mafia vaguely slows down Bitcoin payments temporarily". If we replace the 'mafia' with the 'Russian or Chinese government', the headline changes from "China moves to block Bitcoin payments, impose capital controls" to "Chinese government seizes control of Bitcoin equipment, now worthless". Whereas the former is a victory over the Bitcoin cryptosystem, the latter is pointless theft, violation of property rights, and arbitrary destruction of property. The former is impressive, the latter is embarrassing. 


## Conclusion

My conclusion is this: **miners can't harm Bitcoin**. If an attacker gets 60% or even 99% of the network hashrate, he can do very little. In a world of fungibility and payment channels, he can *not* censor or otherwise interfere with any payments. If he desires to re-write the chain, he is torn between a "small rewrite" of a few blocks (which is relatively unproductive) and a "large rewrite" of many blocks (which is slow, noticeable, and perilous).

I'm gravely concerned, that some members of the Bitcoin Community take Satoshi's qualifier "as long as a majority of CPU power is controlled by nodes that are not cooperating to attack the network" to be some kind of Divine Edict. As if, should we fail to obey this rule, we will be expelled from Paradise. In my view, the qualifier was instead a trivial statement of fact: as long as we have X, we will have result Y. The statement "X implies Y" fully allows us to achieve Y in any number of non-X ways, however much we might prefer to always get our Y from X, whenever possible.

Ultimately, no one can control 100% of the hashpower, or any % of "the hashpower". This is because the power is itself generated by the value of the coins which are created to bid for it. If we change (with the user's consent) the PoW algorithm (to something that we will select -at random- tomorrow), then what will "the hashrate" be? Where will it be? Who would control it?

By the laws of mathematics, 51% of the hashrate has been owned by one person, or one group of people (who could all conspire with each other), for each and every day of Bitcoins ~3000 day existence.

Bitcoin is not invulnerable. If someone can cheaply and easily obtain 51% of the network hashrate, over and over again, then it cannot survive.

But, make no mistake: *that* is the threat we need to worry about. Not how many pools control X% hashrate, or how easy it is to fit mining administrators on a stage, or if they all share a mempool or if they all know each other's phone number.

Those things don't really matter.