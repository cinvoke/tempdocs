# HOWTO: Using Tokens on the CasinoCoin Ledger

Prerequisites: 
- casinocoind installed on a system with json-rpc enabled
- a HTTP client to send json-rpc over http requests (We used curl for the examples)

Definitions:
- CasinoCoin Ledger > CSCL

## json-rpc commands
Using json-rpc calls to interact with the CSCL requires a basic message template and is defined in the following json template:

```
{
    "method": "",
    "params": [
        {}
    ]
}
```

```method```: The command to be executed.

```params```: The required parameters to execute the command.

This format is used in examples in this howto. 

## 1. Creating a CasinoCoin account
A CasinoCoin account is required to send and receive Tokens issued on the CSCL. In this section we'll describe how to create an account and what is need to activate the account.

### 1a. Account creation
```wallet_propose``` creates and returns a new account. All required details to use the created account are returned in JSON format.

It is important to create a back-up of the output as it contains all required information to interact with the account on the CSCL

Example: ```curl -d '{ "method": "wallet_propose", "params": [{}]}' https://wst01.casinocoin.org:5005```

```
{
    "result": {
        "account_id": "cBPTanHFGvu5eQifB6Ex2FhYj9exLkup5y",
        "key_type": "secp256k1",
        "master_key": "COG LUCK HAS NAB BAIT NOT LACK NORM DUET UP MUTT ONTO",
        "master_seed": "sh5XELK4KiaGLk9ercgaCrV7VG6Wq",
        "master_seed_hex": "91118CA1DFB518A9591035956470360B",
        "public_key": "aBPXkCNoQdwprGhgmNkSzZRjnZxGvTeMz7fTZzDHhY7mQGtRBHc6",
        "public_key_hex": "02CF335226B8F64197B261AFF51629C743EE9A88C58415B271DF84522720D7856B",
        "status": "success"
    }
}
```


### 1b. Account activation
Activate the account by sending 10 CSC to the created account. On Testnet we have a faucet available on our Discord server. Simply post the command ``` !testfaucet cBPTanHFGvu5eQifB6Ex2FhYj9exLkup5y ``` where cBPTanHFGvu5eQifB6Ex2FhYj9exLkup5y is the account you want to activate. 

## 2. Using a Token
Tokens are issued on the CSCL. A CasinoCoin account requires a trustline for each token. This allows the account to send and receive tokens. To create a trustline for a specific token, token information is required and can be obtained with the ```config_info``` comand.

### 2a. Retrieve Token information
```config_info``` returns the current configurations on the CSCL. The token details are identified by ```"ConfigType" : "Token"```. For each token, the following details are returned: 
```
 {
    "apiEndpoint" : "https://api.t02.com/endpoint",
    "contactEmail" : "info@t02.com",
    "flags" : 0,
    "fullName" : "TestToken2",
    "iconURL" : "https://raw.githubusercontent.com/casinocoin/CasinoCoin-Assets/master/v4/testtoken2-icon-512.png",
    "issuer" : "caPMaRL4mQkhvEbQsJ8AjBwfXCuThwZCjF",
    "token" : "T02",
    "totalSupply" : "5000000000",
    "website" : "https://www.t02.com"
}
```

Example: ```curl -d '{ "method": "config_info", "params": [{}]}' https://wst01.casinocoin.org:5005 ```

```
{
    "result": {
        "configuration": [
           {
              "ConfigData" : [
                 {
                    "account" : "chXKNwZUjRLxdUumHoWji6iAAs2YLPV5eY"
                 }
              ],
              "ConfigID" : 4,
              "ConfigType" : "Blacklist_Signer"
           },
           {
              "ConfigData" : [
                 {
                    "apiEndpoint" : "https://api.100.com/endpoint",
                    "contactEmail" : "info@100.com",
                    "flags" : 0,
                    "fullName" : "100.com",
                    "iconURL" : "https://raw.githubusercontent.com/casinocoin/CasinoCoin-Assets/master/v4/100-coin-icon-512.png",
                    "issuer" : "cEWTA66ypmFVzZksJntBetbeQdDqnRLA44",
                    "token" : "100",
                    "totalSupply" : "250000000000",
                    "website" : "https://www.100.com"
                 },
                 {
                    "apiEndpoint" : "https://api.pokercoin.com",
                    "contactEmail" : "info@pokercoin.com",
                    "flags" : 0,
                    "fullName" : "PokerCoin",
                    "iconURL" : "https://raw.githubusercontent.com/casinocoin/CasinoCoin-Assets/master/v4/poker-coin-icon-512.png",
                    "issuer" : "cfiikpLkuWH2rYK1b7fV7RziYVuqUY6koo",
                    "token" : "PCN",
                    "totalSupply" : "1000000000",
                    "website" : "https://www.pokercoin.com"
                 },
                 {
                    "apiEndpoint" : "https://api.t01.com/endpoint",
                    "contactEmail" : "info@t01.com",
                    "flags" : 0,
                    "fullName" : "TestToken1",
                    "iconURL" : "https://raw.githubusercontent.com/casinocoin/CasinoCoin-Assets/master/v4/testtoken1-icon-512.png",
                    "issuer" : "cfWBfnbQEWGJkRYQ3AmPCs7PWyq63KZsZj",
                    "token" : "T01",
                    "totalSupply" : "5000000000",
                    "website" : "https://www.t01.com"
                 },
                 {
                    "apiEndpoint" : "https://api.t02.com/endpoint",
                    "contactEmail" : "info@t02.com",
                    "flags" : 0,
                    "fullName" : "TestToken2",
                    "iconURL" : "https://raw.githubusercontent.com/casinocoin/CasinoCoin-Assets/master/v4/testtoken2-icon-512.png",
                    "issuer" : "caPMaRL4mQkhvEbQsJ8AjBwfXCuThwZCjF",
                    "token" : "T02",
                    "totalSupply" : "5000000000",
                    "website" : "https://www.t02.com"
                 },
                 {
                    "apiEndpoint" : "https://api.t03.com/endpoint",
                    "contactEmail" : "info@t03.com",
                    "flags" : 0,
                    "fullName" : "TestToken3",
                    "iconURL" : "https://raw.githubusercontent.com/casinocoin/CasinoCoin-Assets/master/v4/testtoken3-icon-512.png",
                    "issuer" : "cGbq34QJFMnFecpNXaZo4errtXNiFNXmk5",
                    "token" : "T03",
                    "totalSupply" : "100000000000",
                    "website" : "https://www.t03.com"
                 },
                 {
                    "apiEndpoint" : "https://api.t04.com/endpoint",
                    "contactEmail" : "info@t04.com",
                    "flags" : 0,
                    "fullName" : "TestToken4",
                    "iconURL" : "https://raw.githubusercontent.com/casinocoin/CasinoCoin-Assets/master/v4/testtoken4-icon-512.png",
                    "issuer" : "csV3e9kNBNXbdUrqvxRzkYZfW1Ch3883yM",
                    "token" : "T04",
                    "totalSupply" : "1000000000",
                    "website" : "https://www.t04.com"
                 }
              ],
              "ConfigID" : 3,
              "ConfigType" : "Token"
           },
           {
              "ConfigData" : [
                 {
                    "public_key_hex" : "02D7EE8BA8CECD7E573B756094B234F6CA2B46F7F00AD23F61D05ECFF73B818A3A"
                 }
              ],
              "ConfigID" : 2,
              "ConfigType" : "Message_PubKey"
           },
           {
              "ConfigData" : [
                 {
                    "account" : "cwmm2RkhynaPHXWCD94xcxLSVn1pJrU9Fh"
                 }
              ],
              "ConfigID" : 1,
              "ConfigType" : "KYC_Signer"
           }
        ],
        "ledger_current_index": 674534,
        "status": "success",
        "validated": false
    }
}

```

### 2b. Configure an Account to send and receive a Token
```submit``` can be used to create and submit a transaction to the CSCL. 

To create a trustline for a token, you need to submit a TrustSet transaction to the CSCL. The following details need to match your account and the desired token you want to create a Trustline for: 

```
{
    "method": "submit",
    "params": [
        {
            "offline": false,
            "secret": "sh5XELK4KiaGLk9ercgaCrV7VG6Wq",
            "tx_json": {
                "TransactionType": "TrustSet",
                "Account": "cBPTanHFGvu5eQifB6Ex2FhYj9exLkup5y",
                "Flags": 262144,
                "LimitAmount": {
                    "currency": "T02",
                    "issuer": "caPMaRL4mQkhvEbQsJ8AjBwfXCuThwZCjF",
                    "value": "5000000000"
                }
            }
        }
    ]
}
```

You account secret is used to sign and submit the transaction to the CSCL. The following example show how this is done where ```sh5XELK4KiaGLk9ercgaCrV7VG6Wq``` is the secret for the account.

Example: ```curl -d '{"method":"submit","params":[{"offline":false,"secret":"sh5XELK4KiaGLk9ercgaCrV7VG6Wq","tx_json":{"TransactionType":"TrustSet","Account":"cBPTanHFGvu5eQifB6Ex2FhYj9exLkup5y","Flags":262144,"LimitAmount":{"currency":"T02","issuer":"caPMaRL4mQkhvEbQsJ8AjBwfXCuThwZCjF","value":"5000000000"}}}]}' https://wst01.casinocoin.org:5005```

## 3. Sending Tokens to another account
Sending tokens requires the following prerequisites:
- The source account needs CSC to pay transaction fees. A minimum of 10.25 CSC is required on the account to pay the fee of 0.25 CSC.
- The source account needs an amount of a specific Token
- The source account needs a Trustline for that specific Token
- The receiving account needs a Trustline for the same Token

A Payment transaction handles the transfer of a given amount of tokens to another account. This transaction matches the following format: 
```
{
    "method": "submit",
    "params": [
        {
            "offline": false,
            "secret": "sh5XELK4KiaGLk9ercgaCrV7VG6Wq",
            "tx_json": {
                "Account": "cBPTanHFGvu5eQifB6Ex2FhYj9exLkup5y",
                "Amount": {
                    "currency": "T02",
                    "issuer": "caPMaRL4mQkhvEbQsJ8AjBwfXCuThwZCjF",
                    "value": "100000.752"
                 },
                "Destination": "c3SoWTACWk7n49yAiZEXMhLhDmfqNWYb5C",
                "TransactionType": "Payment"
            }
        }
    ]
}
```

Example: ```curl -d '{"method":"submit","params":[{"offline":false,"secret":"sh5XELK4KiaGLk9ercgaCrV7VG6Wq","tx_json":{"Account":"cBPTanHFGvu5eQifB6Ex2FhYj9exLkup5y","Amount":{"currency":"T02","issuer":"caPMaRL4mQkhvEbQsJ8AjBwfXCuThwZCjF","value":"100000.752"},"Destination":"c3SoWTACWk7n49yAiZEXMhLhDmfqNWYb5C","TransactionType":"Payment"}}]}' https://wst01.casinocoin.org:5005```


## 4. View Token balances 
```account_lines``` returns all token balances for an account. 

Example: ```curl -d '{ "method": "account_lines", "params": [{"account":"cBPTanHFGvu5eQifB6Ex2FhYj9exLkup5y"}]}' https://wst01.casinocoin.org:5005```
```
{
   "result" : {
      "account" : "cBPTanHFGvu5eQifB6Ex2FhYj9exLkup5y",
      "ledger_current_index" : 74559,
      "lines" : [
         {
            "account" : "caPMaRL4mQkhvEbQsJ8AjBwfXCuThwZCjF",
            "balance" : "100000.752",
            "currency" : "T02",
            "limit" : "5000000000",
            "limit_peer" : "0",
            "quality_in" : 0,
            "quality_out" : 0
         }
      ],
      "status" : "success",
      "validated" : false
   }
}

```
