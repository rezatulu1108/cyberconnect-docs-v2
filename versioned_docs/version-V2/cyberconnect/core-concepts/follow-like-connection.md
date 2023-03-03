---
id: follow-like-connection
title: Follow/Like connection
slug: /concepts/follow-like-connection
sidebar_label: Follow/Like (Connections 👥)
sidebar_position: 4
description: Off-chain connections supported
---

Follow and Like are additional forms of connection supported by the protocol, but unlike a SubscribeNFT, follow and like don't require any on-chain transaction and are not represented by NFTs. Instead, they are implemented through off-chain proofs that are synced to Arweave. Additionally, while subscriptions are represented through an address minting a ccProfile's SubscribeNFT, follows are represented as `address -> ProfileNFT` relationship and likes are represented as `address -> post` relationship. This is great for applications with light weight social graph needs, that do not want to incur gas costs.

## Idempotent Proof of Connection

Social platforms like Twitter have millions of new connections and posts generated by users every single day. Therefore, the data standard has to be efficient and good to work in at the same time to support scaling.

Take connection data as an example, a rather simple way of doing this would be to put all your following in a list and append the address to the list when a user follows someone new and remove the address from the list when an unfollow action takes place. However, this approach will require the data store and compute engine to provide an ACID guarantee to work in multi-thread scenarios like following multiple people or following and unfollowing in a short time span. Furthermore, the ACID guarantee does not come cheap in a decentralized setting.

Therefore, we adopt idempotent proof to describe the most up-to-date state for connections between any address and profile. The benefit of using idempotency is for easy conflict resolution without an ACID guarantee. Instead of using a data model like “following list” where each new following address gets appended to the same list, we describe the social connections using final desired state (following or not) at that timestamp.

There could only exist one state per operation, e.g. Alice could either only be following or not following Bob. The proof connection includes the following details of the desired state between an originator and a target profile:

```ts
type Proof = {
  content: string;
  digest: string;
  signature: string;
  signingKey: string;
  signingKeyAuth: SigningKeyAuth;
  arweaveTxHash: string;
};

type SigningKey = {
  publicKey: string;
  format: "SubjectPublicKeyInfo";
  algorithm: "ES256";
};

type SigningKeyAuth = {
  address: string;
  signingKeyMessage: string;
  signingKeySignature: string;
};
```

We only describe these data standard in the raw object format. However, the final message would be encoded with both a digest of the message and a signature signed by the owner.

## Data Integrity & Availability

For a decentralized data network, we must ensure data availability (that data cannot be censored) and data integrity (that data cannot be modified in an unauthorized manner).

We designed a mechanism for safely dealing with high frequent writes and updates to connections between users using a hash-linked list and a decentralized data store. Each address pair (for connection proof) and an operation would create a new hashed link list called ‘operation log’ upon the first transaction. Each update to the state (changing from following to unfollowing) would append a new node with the new state to the tail of the operation log. Each new state is stored locally on a central server until a batch upload logic uploads the tail of each operation log to a decentralized file storage system like Arweave or IPFS.

Every node in that operation log is stored locally on a central server for caching and serving fast queries. Users could verify data integrity by getting the latest connection state and traverse through the hashed link list by requesting the previous node from the central server. We assume a trusted central server here since the data availability requires the central node to respond to these data queries. We believe this is a sufficiently decentralized and highly performant system.

![linkedlist](/img/v2/linkedlist.png)