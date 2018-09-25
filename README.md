# meta-transaction-format-share

[EIP-1077](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1077.md) is a very good guideline for how to format your meta transactions, but we are finding that different organizations have different use-cases. We would like to start executing transactions for each other but we need a centralized place to facilitate sharing our current implementations.

Let's all post our meta transaction formats here in a digestible format. The TL;DR here is to communicate how someone else could execute your meta transaction for you.

We are having a meeting soon with the agenda of the meeting here (27th 5PM CEST Sept): https://hackmd.io/-fF2VKIFRzyiWaxVQAh0wA?edit

For now, join us on telegram for the discussion -> Found in the agenda above ^^^


## Implementations

[uport](https://github.com/austintgriffith/meta-transaction-format-share/blob/master/uport.org.md)

[tenzorum](https://github.com/austintgriffith/meta-transaction-format-share/blob/master/tenzorum.org.md)

[tokensubscription](https://github.com/austintgriffith/meta-transaction-format-share/blob/master/tokensubscription.com.md) 

[Gnosis Safe](safe.gnosis.io.md) 


## Important problems to solve

[relay collisions](https://github.com/austintgriffith/meta-transaction-format-share/issues/3)

[dynamic gas rewards](https://github.com/austintgriffith/meta-transaction-format-share/issues/4)


## Format 
```
{
  This is the format of my meta transaction 
}
```

### Rx 
```
{
  Here is my (pseudo?)code for getting transactions from my dapp
}
```

### Tx
```
{
  Here is my (pseudo?)code for putting transactions on chain
}
```


