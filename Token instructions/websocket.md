# HOWTO: Using Tokens on the CasinoCoin Ledger

Prerequisites: 
- casinocoind installed on a system configured to allow websocket connections
- a CasinoCoin Account secret obtained either through the wallet or using the commandline as described in the [cli howto](cli.md#1-creating-a-casinocoin-account)
- a WebSocket client 

Definitions:
- CasinoCoin Ledger > CSCL


## websocket messages
Using a websocket to interact with the CSCL requires a basic message template and is defined in the following json template:
```
{
    "command": ""
}
```

```command```: The command to be executed.

Required parameters are added alongside the command to be executed.

This format is used in examples in this howto. 

## 1. Creating a CasinoCoin account
A CasinoCoin account is required to send and receive Tokens issued on the CSCL. In this section we'll describe how to create an account and what is need to activate the account.

### 1a. Account creation
Follow the instructions in the [cli howto](cli.md#1-creating-a-casinocoin-account) for creating an account or create one using one of the available wallets on the [website](https://casinocoin.org/downloads/)

### 1b. Account activation
Activate the account by sending 10 CSC to the created account. On Testnet we have a faucet available on our Discord server. Simply post the command ``` !testfaucet cH7bHBfzvVxwZyR4vM9ZK2aipSnvbfx4UC ``` where cH7bHBfzvVxwZyR4vM9ZK2aipSnvbfx4UC is the account you want to activate. 

## 2. Using a Token
Tokens are issued on the CSCL. A CasinoCoin account requires a trustline for each token. This allows the account to send and receive tokens. To create a trustline for a specific token, token information is required and can be obtained with the ```config_info``` comand.

### 2a. Retrieve Token information
```config_info``` returns the current configurations on the CSCL. The token details are identified by ```"ConfigType" : "Token"```. For each token, the following details are returned: 
```
 {
     "apiEndpoint": "https://api.t02.com/endpoint",
     "contactEmail": "info@t02.com",
     "extraFeeFactor": 0,
     "flags": 0,
     "fullName": "TestToken2",
     "iconURL": "https://raw.githubusercontent.com/casinocoin/CasinoCoin-Assets/master/v4/testtoken2-icon-512.png",
     "issuer": "caPMaRL4mQkhvEbQsJ8AjBwfXCuThwZCjF",
     "token": "T02",
     "totalSupply": "5000000000",
     "website": "https://www.t02.com"
 }
```

Example: 
```
{
    "command": "config_info"
} 
```
Response: 
```
{
    "result": {
        "configuration": [
            {
                "ConfigData": [
                    {
                        "account": "chXKNwZUjRLxdUumHoWji6iAAs2YLPV5eY"
                    }
                ],
                "ConfigID": 4,
                "ConfigType": "Blacklist_Signer"
            },
            {
                "ConfigData": [
                    {
                        "apiEndpoint": "https://api.100.com/endpoint",
                        "contactEmail": "info@100.com",
                        "extraFeeFactor": 0,
                        "flags": 0,
                        "fullName": "100.com",
                        "iconURL": "https://raw.githubusercontent.com/casinocoin/CasinoCoin-Assets/master/v4/100-coin-icon-512.png",
                        "issuer": "cEWTA66ypmFVzZksJntBetbeQdDqnRLA44",
                        "token": "100",
                        "totalSupply": "250000000000",
                        "website": "https://www.100.com"
                    },
                    {
                        "apiEndpoint": "https://api.pokercoin.com",
                        "contactEmail": "info@pokercoin.com",
                        "extraFeeFactor": 0,
                        "flags": 0,
                        "fullName": "PokerCoin",
                        "iconURL": "https://raw.githubusercontent.com/casinocoin/CasinoCoin-Assets/master/v4/poker-coin-icon-512.png",
                        "issuer": "cfiikpLkuWH2rYK1b7fV7RziYVuqUY6koo",
                        "token": "PCN",
                        "totalSupply": "1000000000",
                        "website": "https://www.pokercoin.com"
                    },
                    {
                        "apiEndpoint": "https://api.t01.com/endpoint",
                        "contactEmail": "info@t01.com",
                        "extraFeeFactor": 0,
                        "flags": 0,
                        "fullName": "TestToken1",
                        "iconURL": "https://raw.githubusercontent.com/casinocoin/CasinoCoin-Assets/master/v4/testtoken1-icon-512.png",
                        "issuer": "cfWBfnbQEWGJkRYQ3AmPCs7PWyq63KZsZj",
                        "token": "T01",
                        "totalSupply": "5000000000",
                        "website": "https://www.t01.com"
                    },
                    {
                        "apiEndpoint": "https://api.t02.com/endpoint",
                        "contactEmail": "info@t02.com",
                        "extraFeeFactor": 0,
                        "flags": 0,
                        "fullName": "TestToken2",
                        "iconURL": "https://raw.githubusercontent.com/casinocoin/CasinoCoin-Assets/master/v4/testtoken2-icon-512.png",
                        "issuer": "caPMaRL4mQkhvEbQsJ8AjBwfXCuThwZCjF",
                        "token": "T02",
                        "totalSupply": "5000000000",
                        "website": "https://www.t02.com"
                    },
                    {
                        "apiEndpoint": "https://api.t03.com/endpoint",
                        "contactEmail": "info@t03.com",
                        "extraFeeFactor": 0,
                        "flags": 0,
                        "fullName": "TestToken3",
                        "iconURL": "https://raw.githubusercontent.com/casinocoin/CasinoCoin-Assets/master/v4/testtoken3-icon-512.png",
                        "issuer": "cGbq34QJFMnFecpNXaZo4errtXNiFNXmk5",
                        "token": "T03",
                        "totalSupply": "100000000000",
                        "website": "https://www.t03.com"
                    },
                    {
                        "apiEndpoint": "https://api.t04.com/endpoint",
                        "contactEmail": "info@t04.com",
                        "extraFeeFactor": 0,
                        "flags": 0,
                        "fullName": "TestToken4",
                        "iconURL": "https://raw.githubusercontent.com/casinocoin/CasinoCoin-Assets/master/v4/testtoken4-icon-512.png",
                        "issuer": "csV3e9kNBNXbdUrqvxRzkYZfW1Ch3883yM",
                        "token": "T04",
                        "totalSupply": "1000000000",
                        "website": "https://www.t04.com"
                    }
                ],
                "ConfigID": 3,
                "ConfigType": "Token"
            },
            {
                "ConfigData": [
                    {
                        "public_key_hex": "02D7EE8BA8CECD7E573B756094B234F6CA2B46F7F00AD23F61D05ECFF73B818A3A"
                    }
                ],
                "ConfigID": 2,
                "ConfigType": "Message_PubKey"
            },
            {
                "ConfigData": [
                    {
                        "account": "cwmm2RkhynaPHXWCD94xcxLSVn1pJrU9Fh"
                    }
                ],
                "ConfigID": 1,
                "ConfigType": "KYC_Signer"
            },
            {
                "ConfigData": [
                    {
                        "activated": true,
                        "foundationFeeFactor": 25,
                        "foundationPublicKey": "026E69FA61E04D4469AF5B79B305472DF0C06C06BC5D7A539737FB74C438820F04"
                    }
                ],
                "ConfigID": 5,
                "ConfigType": "CRN_Settings"
            }
        ],
        "ledger_current_index": 391839,
        "validated": false
    },
    "status": "success",
    "type": "response"
}

```

### 2b. Configure an Account to send and receive a Token
```submit``` can be used to create and submit a transaction to the CSCL. 

To create a trustline for a token, you need to submit a TrustSet transaction to the CSCL. The following details need to match your account and the desired token you want to create a Trustline for: 

You account secret is used to sign and submit the transaction to the CSCL. The following example show how this is done where ```snkcJSA1YbCzNVPT1ADGX1KUKS5Rb``` is the secret for the account.

Example:
```
{
    "command": "submit",
    "tx_json": {
        "TransactionType": "TrustSet",
        "Account": "cH7bHBfzvVxwZyR4vM9ZK2aipSnvbfx4UC",
        "Flags": 262144,
        "LimitAmount": {
            "currency": "T02",
            "issuer": "caPMaRL4mQkhvEbQsJ8AjBwfXCuThwZCjF",
            "value": "5000000000"
        }
    },
    "secret": "snkcJSA1YbCzNVPT1ADGX1KUKS5Rb",
    "offline": false,
}
```

## 3. Sending Tokens to another account
Sending tokens requires the following prerequisites:
- The source account needs CSC to pay transaction fees. A minimum of 10.25 CSC is required on the account to pay the fee of 0.25 CSC.
- The source account needs an amount of a specific Token
- The source account needs a Trustline for that specific Token
- The receiving account needs a Trustline for the same Token

A Payment transaction handles the transfer of a given amount of tokens to another account. 

Example:
```
{
    "command": "submit",
    "tx_json": {
        "Account": "cH7bHBfzvVxwZyR4vM9ZK2aipSnvbfx4UC",
        "Amount": {
            "currency": "T02",
            "issuer": "caPMaRL4mQkhvEbQsJ8AjBwfXCuThwZCjF",
            "value": "100000.752"
        },
        "Destination": "c3SoWTACWk7n49yAiZEXMhLhDmfqNWYb5C",
        "TransactionType": "Payment"
    },
    "secret": "snkcJSA1YbCzNVPT1ADGX1KUKS5Rb",
    "offline": false
}
```

## 4. View Token balances 
```account_lines``` returns all token balances for an account. 

Example: 
```
{
    "command": "account_lines",
    "account": "cnQBcReUuPx1MmoiCQu9eL7SBLbvkHTgcT"
} 
```

Response:
```
{
    "result": {
        "account": "cnQBcReUuPx1MmoiCQu9eL7SBLbvkHTgcT",
        "ledger_current_index": 674617,
        "lines": [
            {
                "account": "c3FUV8BoxCbvr3jAxzWdxTXhqDPeCUx527",
                "balance": "800000",
                "currency": "CR8",
                "limit": "500000000000",
                "limit_peer": "0",
                "quality_in": 0,
                "quality_out": 0
            }
        ],
        "validated": false
    },
    "status": "success",
    "type": "response"
}
```
