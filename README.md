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

[Screencast from 05.07.2024. 12:48:46.webm](https://github.com/okisoftware/okic-payment-gateway/assets/63173020/7bc2528f-0581-44cc-a3d6-ac4d8b6aeabe)
