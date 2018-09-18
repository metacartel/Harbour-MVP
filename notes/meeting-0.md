# MetaTx Crew Agenda - Meeting 0 

Since the creation of ERC-1077/1078, we figured out that we can do somepretty cool things with delegated execution of meta-txs. Many projects have sprung up surrounding this. So far we have had little communication between projects. There is a lot to be gained in doing so as we seem to all face similar challenges and issues. 

Plus there is an opportunity to also explore the possibility of a permissionless p2p meta-tx network which will be srsly fucking cool.

## What we look to work on & progress so far...

- **Exploring the possibility of a permissionless p2p meta-tx network** where there is a decentralised pool of meta-txs that can be executed by a decentralised pool of service node executors

-> Austin & the Tenzorum squad have had an initial first call about this and have decide that the first step to take is to share how we have designed the meta-tx. So far TokenSubs (Austin), Tenzorum, Uport have uploaded their meta-tx designs here: https://github.com/austintgriffith/meta-transaction-format-share* 

-> We would love more teams that are working on this to participate and we are going to decide on the next step in meeting-0

- **Sharing what problems** we have come across and how we can figuring out how to solve them efficiently

-> We have started to share problems we are trying to solve here with discussions beginning to take place: https://github.com/austintgriffith/meta-transaction-format-share/issues

-> Mainly about how to propagate meta-tx across service node network + reward economics / gas-reward dynamics & finding market equilibrium

- **Gathering a list of projects** & info about meta-tx / gas delegation: [LINK]

- **Writing up a blog post overviewing** all the different meta-tx projects out there. Currently in-progress by Golem, MP & friends.

- **We now have a telegram chat to join** When you join, please introduce yourself and share what you are working on: https://t.me/joinchat/HcTaOxJH6OzCna2UWJZo9A

## Agenda for this first meeting
*// Free free to add your own points, notes or even add to existing agenda items. Find this buttom on the top left side of the page to edit/add.*

![](https://i.imgur.com/6MTGy3k.png)

### tldr: get on the same page with language, define our problems and share our roadmaps in development

- **Define the language we use and get on the same page.** (gas delegation, meta-tx, gasless transactions, service nodes, relayers etc.) 

-> Tenzorum has begun to create a definitions section here in their docs https://github.com/Tenzorum/.documentation/blob/master/TSNN.md#jargon--definition-of-terms

- **Share our progress so far on meta-txs and what our individual project/team project are at. What problems we are working on right now, what next, what is on our minds...**

-> Anything that comes up here outside the agenda is added to the bottom of the agenda for this meeting's call.

- **Discuss immediate next steps to achieve decentralised execution of meta-tx** (now that we have shared how we have designed our meta-txs)

-> Austin & the Tenzorum squad have had an initial first call about this and have decide that the first step to take is to share how we have designed the meta-tx. So far TokenSubs (Austin), Tenzorum, Uport have uploaded their meta-tx designs here: https://github.com/austintgriffith/meta-transaction-format-share* 

-> Settle on a standard way to design meta-txs?

-> Discuss the transportation layer of this protocol? libp2p, whisper?

-> Contract standard for establishing onchcain indication between fund contract (that pays for gas) and the public key that is signing the meta-tx. ERC725? Or a lighter more specific design?

-> What other considerations do we potentially need to share/standardise on for the decentralised execution of meta-txs happen? 

- **Other problems surrounding the design meta-tx protocol** (This meeting we will likely just be defining issues and problems to solve then creating threads on the github for them.)

-> Problem surrounding service node Tx relay selection (needs algo to to determine which nodes gets Txs and how much / how often). Should this be designed on the p2p service node layer or the network transportation layer?

-> Consensus on tx ordering? batched POW of txs with trimmed header hash? 

-> Service node staking, slashing? Service node onchain eth gas issue...

- **Decide on a org name for the github**

--- Inteplantetary Meta Wagon (IPMW)
--- FullMeta Alchemists
--- The Meta Cartel
--- Long Range Ballistic Transactions (LRBT)

## Recommended preparation for this meeting

- Read this whole page carefully.

# Details 

**Date & Time:** 
Yet to be decided - need to coordinate a good time

**Time:** 1hr, highly likely to go over this as a caution

**Meeting join link:**
Yet to be added (we will be using google hangout + livestreaming it). For now join the telegram (Please intro yourself, if u don't intro yourself, I swear to god I will kcick you from the chat lmao): https://t.me/joinchat/HcTaOxJH6OzCna2UWJZo9A

**Attendees**

Pet3rpan, Tunnel-rat @Tenzorum 
Mark, Software @Tenzorum
Radek, Blockchains @Tenzorum

**...add yourself, permissionless cryptoanarchy!!**
