## OKIC Payment Gateway Documentation

### Introduction
OKIC payment integration allows merchants to use OKIC as a payment currency. It supports a seamless payment experience on your platform
while protecting your application from payment frauds and complexity related to various regulations.

### Staging and Production Environment
OKIC staging and production environments are separate.

Merchants need an active and verified OKIC account to use both environments.

- [OKIC Staging Wallet](https://okiwallet.xyz)
- OKIC Production Wallet is not available yet

All transactions initiated by merchant on our staging environment are not real and there are strictly for testing.
All functionality on both environments are the same and it will be easy to switch on production environment after functionality 
testing by changing the URL.

- OKIC staging URL: https://backend.okiwallet.xyz
- OKIC production URL: **not available - coming soon**

All transactions on both environments are available in OKIC explorer.

- [OKIC Staging Explorer](https://explorer.okiwallet.xyz)
- OKIC Production Explorer is not available yet

### Requirements for OKIC integration

Following credentials are required for merchant OKIC integration:

- [x] **merchant public key**
- [x] **merchant private key**