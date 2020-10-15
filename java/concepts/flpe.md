# Format and Length Preserving Encryption

Format and Length Preserving Encryption (FLPE) are ways of encrypting data while keeping the cipher text format or size the same as the input. For example, with format preserving encryption, a credit card number will still look like a credit card number after encrypting, and will still pass the Luhn check required. With length preserving encryption, the cipher text will have the same size as the plain text. These approaches comes with security implications that must be understood.

## On key rotation

FLPE requires that the output be the same format or length as the input, which means that no information about the key can be stored with the cipher text. Additionally, the premise for FLPE is that the storage container cannot be changed, so key information cannot be stored alongside the cipher text. This means that once a key is selected for encryption, that same key needs to be used for all data. For the SDK, that means using a non-rotatable Smartkey.

Non-rotatable Smartkeys have serious drawbacks. When a user with access to a non-rotatable Smartkey has their access revoked, Smartcrypt deletes the key from the user's devices, same as with rotatable Smartkeys. However, the key could have been compromised. With rotatable Smartkeys we issue a new encryption key for use in all future encryptions, thereby ensuring that the removed user cannot access new data; with non-rotatable Smartkeys we continue to use the same encryption key.
