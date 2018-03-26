## lightning network

> what is lightning network?
>> lightning network is a second layer network which is used to solving the scaling problem that bitcoin currently facing . it is used a machanism called off-chain transaction which allowed two parties communicate directly from each other without putting all there transactions verified by the network. or two parties don't know each other but connect through a third party .

> first let's talk about the first case , two parties talk directly to each other.
>> the mechanism called `recovable sequence maturity contract` short for RSMC.![Lightning RSMC](https://github.com/xiaomingfuckeasylife/imgStore/blob/master/lightningNetwork.png)
here is the thing . bob know alice very well , and they have finantial interaction with each other all the time ,but the transaction is two slow . today bob and alice is trying to this new technology LightningNetwork.

>>so here is what they have done from that picture .
>>>* bob and alice all putting aside 1 btc for there finantial transfer . 
>>>* now Alice start a transaction and she ask bob to sign the transaction and alice also sign the transaction , the output of this transaction is that if i broke the promise between us . you will get 1 btc right array while i will get my 1btc after 1000 network confirmation as a punishment.
>>>* after that Bob start a transaction too, the transaction is basicly the same as alice ,except bob need alice to sign the transaction and if bob break the promise bob will be the one get punished .
>>>* after they bose sign the transaction they starting to sign the funding transaction as i showed in the picture . the funding tx's input are 1btc from alice signed by alice and 1btc from bob signed by bob and the output is a multi-signature. now the rsmc is created and the transaction will be broadcast to other peers. so we can say that this output is live in the mainet. 
>>>* now alice and bob can both broadcast the their transaction because they both have the mult-sign but they both would't because the they don't want there bitcoin freezon for 1000 comfirmation . 
>>>* so let'us move on. Now Alice bought some service from bob and alice need to pay bob 0.2btc.  as we can see in the third part of the picture.  now we got a problem ?? why should alice broadcast the second tx why not just broadcast the first one so that she could not pay bob? because there is another smart move in there .when alice is creating the second tx she will send her private key ALICE2 to bob to let bob know that alice is giving up the first tx . because bob now the first tx private key bob now can modify the output of that transaction . so ALICE will absolutely not broadcast the first tx to the main network. 
>>>* so what happends in the end ? in the end if bob got 1.8btc and alice got 0.2btc they can just create a confirm tx and broadcast it to the mainet close the deal between them . 

>> Conclution. from above we can see that bob and alice can transfer multiple times using lightning network and only interact with the bitcoin mainet twice . first is open a Funding tx and second is close the confirm tx.  so cool . right . 

