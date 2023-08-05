# Tezos in-depth Project

The repository contains two jsligo contracts, their compiled michelson files and a michelson solution script. 
**It is not recommended to check the solution file before trying to find the problem(s) in the contracts**

## Main Contract

The main contract (`main_contract.jsligo`) has two entry points, `mint_shares` and `claim_share`. `mint_shares` entry point takes a map as an argument.
The argument map must contain the public addresses of owners of new tickets as keys and corresponding ticket amounts as values. The entry point creates tickets for
each public address. If the user has a ticket in the storage, the newly created ticket is joined with the existing ticket in the storage, thus the ticket amount of 
the given address is increased in the storage. In the end, the entry point returns and stores a ticket for each public address.

**NB:** `mint_shares` entry point checks if the sender is admin, which is defined in the storage in the origination phase. If the sender is not the admin, then the
contract will fail.

The `claim_share` entry point takes a public address of a smart contract as an argument. First, it checks whether the given address is stored in the storage and 
have a ticket. Then the ticket which is stored in storage is sent to the `default` entry point of the given smart contract. The `claim_share` entry point 
can be invoked by anyone.

## User Wallet Contract

The user wallet contract (`user_wallet.jsligo`) has only one entry point, `claim`. It checks if there is a ticket in the storage, if there is, it joins the new
ticket with the existing ticket and stores the resulting ticket. If there is no ticket in the storage, it stores the new ticket without performing any additional
operations.
