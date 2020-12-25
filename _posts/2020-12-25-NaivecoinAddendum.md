---
title: Naivecoin Addendum
date: '2020-12-25'
categories:
  - blog
tags:
  - crypto
  - cpp
published: true
---
Currently I am building a cryptocurrenct in the C++. The project can be found [here](https://github.com/zethon/Ash). This is not a project where I've forked another code base and am making it my own. The purpose of this project was to write a brand new protocol from scratch. 

Much of what I'm learning from writing a cryptocurrency has been taken from [this Javascript tutorial](https://lhartikk.github.io/) on writing your own cypto. However, like any tutorial, the author assumes a foundation of knowledge in the reader that may not exist. At other times important details are left out for reasons I can't figure out. This is not a knock on the author. The example is an excellent tutorial and has helped me immensley. This post is just an attempt to fill in some of things that I perceive to be missing pieces.

## Chapter 3 - Transactions

### Transaction Inputs

Here the reader is introduced to `TxIn` which is given the following structure:

```typescript
class TxIn 
{
    public txOutId: string;
    public txOutIndex: number;
    public signature: string;
}
```

The `signature` field is explained but the `txOutId` and `txOutIndex` fields never are! Here is what I could find out from both of them.

The `txIn` is the part of the transaction that says "the money being sent in this transaction comes from this `txtOut`". That `txOut` is located by two components:

#### `txOutIndex`

This is the block index that contains the transaction in which the originating `TxOut` exists.

#### `txOutId`

This is the transaction id in which the originating `TxOut` exists. 

