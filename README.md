## OKIC Payment Gateway Documentation

### Introduction
OKIC payment integration allows merchants to use OKIC as a payment currency. It supports a seamless payment experience on your platform
while protecting your application from payment fraud and complexity related to various regulations.

### Staging and Production Environment
OKIC staging and production environments are separate.

Merchants need an active and verified OKIC account to use both environments.

- [OKIC Staging Wallet](https://okiwallet.xyz)
- _OKIC Production Wallet is not available yet_

All transactions initiated by merchants on our staging environment are not real and are strictly for testing.
All functionality on both environments is the same and it will be easy to switch on the production environment after functionality 
testing by changing the URL.

- OKIC staging URL: https://backend.okiwallet.xyz
- OKIC production URL: **not available - coming soon**

All transactions on both environments are available in OKIC Explorer.

- [OKIC Staging Explorer](https://explorer.okiwallet.xyz)
- _OKIC Production Explorer is not available yet_

### Requirements for OKIC integration

The following credentials are required for merchant OKIC integration:

- [x] **merchant public key**
- [x] **merchant private key**

Copy your credentials from the OKIC wallet and place them in a secure place on your server.
You can see the video below on how to get your public and private key.

[getKeys.webm](https://github.com/okisoftware/okic-payment-gateway/assets/63173020/7bdf4a76-a8dd-4657-b86b-983a70e53066)

For security reasons, you can roll back your keys and replace old keys with new ones.

### OKIC Integration Methods

OKIC supports two different methods for merchant integration:

- [x] Payment Gateway integration
- [x] Payment Processing integration

To understand the integration methods we have to introduce the OKIC transaction structure or the most important parts:

- **txid**: a unique transaction identifier, created by hashing transaction details
- **status**: transaction status after transaction validation
- **sender**: Wallet address that initiated the payment transaction
- **recipient**: Wallet address that is receiving the payment transaction
- **issuer**: Wallet address that issued the payment transaction
- **amount**: The amount of currency being transacted
- **txFee**: The fee charged to process the transaction
- **additionalFee**: Additional fee charged to process the transaction

The merchant has to be verified as an OKIC business account. After successful verification, Merchant will have one of the following payment types:

- Regular (fixed transaction fee)
- Pre-Payment (one part of the transaction fee is fixed, and the other is variable depending on the number of transactions)

#### Payment Gateway integration

The payment Gateway integration method is intended for merchants that want to use OKIC tokens as a means of payment.
This method is commonly used on e-commerce web services. In this case, the merchant is the recipient and issuer, and the sender is the merchant
user who pays for merchant services. The transaction fee is charged to the merchant. 
The sender will pay only the merchant service price.

#### Payment Processing integration

The payment processing integration method is intended for merchants who want to transfer payment transactions from sender to recipient.
The merchant is an issuer and can charge an additional transaction fee for merchant services. Transaction fees and additional transaction fees are charged to the sender.

## Payment Gateway Integration Example

```typescript

import axios from "axios";

const generatePaymentGateway = async () => {
  const walletAddress = "okicx5222..." // Sender OKIC Wallet Address
  const amount = 100; // Transaction value for processing
  const token = "VKX9LD"; // Secure token generated in sender OKIC wallet account
  const senderPublicKey = "6087eddca5e18eb51730ae1992de5777" // Sender OKIC Public Key
  const publicKey = process.env.PUBLIC_KEY // merchant public key;
  const privateKey = process.env.PRIVATE_KEY // merchant private key;

  const OKIC_URL = "https://backend.okiwallet.xyz/api"

  try {
    const { data } = await axios.post(`${OKIC_URL}/transaction/external/payment/client`, {
      walletAddress,
      amount,
      token,
      senderPublicKey,
      publicKey,
      privateKey
    });

    const { txid } = data;

    /** save txid from the response in your storage with your order details */
    /** in this moment transaction has PreAuthorized status and it's not validated yet */
    /** txid is unique and you can query your storage later by txid to find and 
     * update your order if the transaction status is Success */
  } catch (err) {
    // catch exception from the OKIC server
  }
};

generatePaymentGateway();

```

## Payment Processing integration Example

```typescript

import axios from "axios";

const generatePaymentProcessing = async () => {
  const senderWalletAddress = "okicx5222..." // Sender OKIC Wallet Address
  const receiverWalletAddress = "okicx6333..." // Recipient OKIC Wallet Address
  const amount = 100; // Transaction value for processing
  const additionalFee = 0.5; // Fee charged by merchant for the services, it can be 0 
  const token = "VKX9LD"; // Secure token generated in sender OKIC wallet account
  const senderPublicKey = "6087eddca5e18eb51730ae1992de5777" // Sender OKIC Public Key
  const publicKey = process.env.PUBLIC_KEY // merchant public key;
  const privateKey = process.env.PRIVATE_KEY // merchant private key;

  const OKIC_URL = "https://backend.okiwallet.xyz/api"

  try {
    const { data } = await axios.post(`${OKIC_URL}/transaction/external/payment/business`, {
      senderWalletAddress,
      receiverWalletAddress,
      amount,
      additionalFee,
      token,
      senderPublicKey,
      publicKey,
      privateKey
    });

    const { txid } = data;

    /** save txid from the response in your storage with your order details */
    /** in this moment transaction has PreAuthorized status and it's not validated yet */
    /** txid is unique and you can query your storage later by txid to find and 
     * update your order if the transaction status is Success */
  } catch (err) {
    // catch exception from the OKIC server
  }
};

generatePaymentProcessing();

```

For both examples, if the API request is successful you should get the response JSON payload:

```json
{
  "success": true,
  "message": "Transaction sent to the Blockchain network.",
  "txid": "3fc590b38bd2a6e6b59ccc538ae91de4d5c03b4e0e7f421bedfaa867e850a097"
}

```

For both examples, if the API request is unsuccessful the response returns JSON payload in the format:

```json
{
    "success": false,
    "error": {
        "code": 400,
        "message": "Invalid Confirmation Token"
    }
}
```

## Listen to transactions status from OKIC Blockchain Socket Server

The previous examples are only the first step to completing payment with OKIC.
You have to listen to the OKIC Blockchain Socket Server for transaction validation status.
To achieve that you can use _socket.io-client_ on your server.

**You can use only one Business and verified OKIC account for the merchant services.**

_Grouping merchant services in one OKIC account will come in the next version_.

```typescript
import { io } from 'socket.io-client';

const OKIC_URL = 'https://backend.okiwallet.xyz';

const ioClient = io(OKIC_URL, {
  transports: ["websocket"],
});

const publicKey = process.env.PUBLIC_KEY; // merchant public key
const CONNECT_OKIC_CHANNEL = 'connect_external_app';
const LISTEN_TX_CHANGES = 'external_app_validated_tx';

ioClient.on('connect', () => {
  logger.debug('Successful connected on OKIC Blockchain');

  ioClient.emit(CONNECT_OKIC_CHANNEL, ({ publicKey }));

  ioClient.on(LISTEN_TX_CHANGES, (data) => {
    const { txid, status, sender, recipient } = data;

    if (status === "Success") {
      /** do something with txid */

    }
  })
});

```

Payload from the channel **external_app_validated_tx**:
```typescript
{
  txid: '0e10ca33a08fe596bc618887e0826c566aa926f9c33c7c31f7d0ffca2719f260',
  status: 'Success',
  sender: 'okicx522e2ab639cd8eaf4fb95ed268e30c7a104350b9fbc8c560887cb3d3b4295907',
  recipient: 'okicxbbe3e224a6e018c93202c79f77af69384aada5981ba3e244d78e4b1641ca7ff0'
}
```
### Generate Secure Token Example

[generateSecureToken.webm](https://github.com/okisoftware/okic-payment-gateway/assets/63173020/8d62ba8b-11fa-45d9-aa65-52a5b7484b2d)

