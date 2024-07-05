## OKIC Payment Gateway Documentation

### Introduction
OKIC payment integration allows merchants to use OKIC as a payment currency. It supports a seamless payment experience on your platform
while protecting your application from payment frauds and complexity related to various regulations.

### Staging and Production Environment
OKIC staging and production environments are separate.

Merchants need an active and verified OKIC account to use both environments.

- [OKIC Staging Wallet](https://okiwallet.xyz)
- _OKIC Production Wallet is not available yet_

All transactions initiated by merchant on our staging environment are not real and there are strictly for testing.
All functionality on both environments are the same and it will be easy to switch on production environment after functionality 
testing by changing the URL.

- OKIC staging URL: https://backend.okiwallet.xyz
- OKIC production URL: **not available - coming soon**

All transactions on both environments are available in OKIC explorer.

- [OKIC Staging Explorer](https://explorer.okiwallet.xyz)
- _OKIC Production Explorer is not available yet_

### Requirements for OKIC integration

Following credentials are required for merchant OKIC integration:

- [x] **merchant public key**
- [x] **merchant private key**

Copy your credentials from OKIC wallet and place it on secure place on your server.
You can see the video bellow how to get your public and private key.

[getKeys.webm](https://github.com/okisoftware/okic-payment-gateway/assets/63173020/7bdf4a76-a8dd-4657-b86b-983a70e53066)

For security reasons you can roll back your keys and replace old keys with new ones.

### OKIC Integration Methods

OKIC supports two different methods for merchants integration:

- [x] Payment Gateway integration
- [x] Payment Processing integration

For the understanding of the integration methods we have to introduce the OKIC transaction structure, or the most important parts:

- **txid**: a unique transaction identifier, created by hashing transaction details
- **status**: transaction status after transaction validation
- **sender**: Wallet address that initiated the payment transaction
- **recipient**: Wallet address that is receiving the payment transaction
- **issuer**: Wallet address that issued the payment transaction
- **amount**: The amount of currency being transacted
- **txFee**: The fee charged to process the transaction
- **additionalFee**: Additional fee charged to process the transaction

Merchant has to be verified as a OKIC business account. After successful verification Merchant will have one of the following payment types:

- Regular (fixed transaction fee)
- Pre-Payment (one part of the transaction fee is fixed, and other is variable depending of the number of transactions)

#### Payment Gateway integration

Payment Gateway integration method is intended to the merchant that want to use OKIC token as means of payment.
This method is commonly used on e-commerce web services. In this case merchant is the recipient and issuer, and sender is merchant
user that pays merchant services. Transaction fee is charged to the merchant. 
Sender will pays only the merchant service price.

#### Payment Processing integration

Payment Processing integration method is intended to the merchants that wants to transfer payment transaction from sender to the recipient.
Merchant is issuer and can charged additional transaction fee for merchant services. Transaction fee and additional transaction fee are charged to the sender.
