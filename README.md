# CSCL Docs
Getting started on CasinoCoin Ledger development. This documentation contains some guides for integrating CasinoCoin Ledger related tasks into your platform. This includes creating accounts, viewing ledger data and handling token transactions.

## Versions
We've written a few examples based on the type of accessibility or which development language is used. 

References:
- [Using command line](Token%20instructions/cli.md)
- [Using a WebSocket](Token%20instructions/websocket.md)
- [Using JSON-rpc over HTTP](Token%20instructions/json-rpc.md)

## Packages
Available packages:
- [npm](https://www.npmjs.com/package/@casinocoin/libjs)


## Things to keep in mind
- Validators only allow transactions for tokens issued on the CSCL
- Sending tokens before creating a trustline will result in a failed transaction.
- Transactions with a "TransactionResult" : "tesSUCCESS" are accepted by the node it was send to. To verify this, check if the returned transaction hash is stored in a validted ledger.
- Transaction fees are paid in CSC. Having CSC on the sender account is required for sending tokens.
- CasinoCoins are formatted in a big number which translate to a decimal with a precision of 8. Token values are used as decimals. Tokens values are not transformed.
- Tokens transaction details like Memo's, Destination tag and Flags work exactly the same as a regular CasinoCoin transaction. 
