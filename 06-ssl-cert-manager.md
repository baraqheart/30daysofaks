cert manager is kubernetes native certificate managements controller
- which can help with issueing of certificates from various providers such as Let's Encrypt, venafi, hashicorp vault and many other

which ensures certificate are up to date and attempt renewal on expiry

### STEP I: INSTALL CERTIFICATE MANAGER
we wil use helm to install the cert manager 


### STEP II: CONFIGURE CERT MANIFEST FILE

Configure the manifest to use Let's Encrypt as the provider

## REFERENCES
[cert manager doccs](https://cert-manager.io/docs/)

[Let's Encrypt docs](https://cert-manager.io/docs/configuration/acme/)