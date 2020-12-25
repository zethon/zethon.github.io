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
Currently I am building a cryptocurrency in the C++. The project can be found [here](https://github.com/zethon/Ash). This is not a project where I've forked another code base. The purpose of this project was to write a brand new protocol from scratch. 

Much of what I'm learning from writing a cryptocurrency has been taken from [this Javascript tutorial](https://lhartikk.github.io/) on writing your own crypto. However, like any tutorial, the author assumes a foundation of knowledge in the reader that may not exist. At other times important details are left out for reasons I can't figure out. This is not a knock on the author. The example is an excellent tutorial and has helped me immensely. This post is just an attempt to fill in some of the things that I perceive to be missing.

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

#### Example

Consider the following JSON:

```json
{
  "index": 57,
  /* lots of other Block fields */
  "transactions": [
    {
      "id'": "transaction0",
      "inputs": [
        {
          "txOutId": "",
          "txOutIndex": 0,
          "signature": ""
        }
      ],
      "outputs": [
        {
          "address": "Stefan",
          "amount": 10
        }
      ]
    },
    {
      "id'": "transaction1",
      "inputs": [
        {
          "txOutId": "transaction0",
          "txOutIndex": 57,
          "signature": ""
        }
      ],
      "outputs": [
        {
          "address": "Henry",
          "amount": 4
        },
        {
          "address": "Addy",
          "amount": 3
        },
        {
          "address": "Stefan",
          "amount": 1
        }
      ]
    }
  ]
}
```

Here we have Block #57 in our chain, which contains two transactions `transaction0` and `transaction1`. In practice the fields `id`, `address` and `signature` would have hashes, for purposes of this explanation they have been simplified.

You may notice that `transaction0` has no `txIn` information, which would only happen with a coinbase transaction (i.e. the reward that happens when a miner has solved a block). In this example the receiving address is `Stefan` who has balance of `10`. 

In `transaction1` we see that Stefan has sent `4` crypto to Henry and `3` to Addy, with the remainder being send back to Stefan. 

Also, notice how the `txOutIndex` value is `57` meaning that the source of this `txIn` is located in Block #57. Likewise, the value of `txOut` tells you exactly which transaction in the block from which this `txIn` comes.
