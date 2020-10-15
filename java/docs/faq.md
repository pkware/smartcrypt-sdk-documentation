# FAQ

## What's a Smartkey?

A Smartkey is a managed, 256 bit symmetric encryption key with cryptographically validated metadata. Smartkeys are
stored in the Smartcrypt Enterprise Manager for easy auditing and escrow. Smartkeys have access control lists built-in
and can be versioned, allowing for key rotation.

## Smartkey Kinds and Owners

Smartkey kinds are used to sort Smartkeys into logical groups. For example, you might have multiple keys used for
encrypting personally identifiable information (PII), multiple keys for encrypting file shares, and multiple keys that
only certain Active Directory groups should ever have access to. By creating a Kind for each of these scenarios, you
can group the keys and define internal policies for how keys should be distributed to Applications.

Each Smartkey is owned by an identity. Most identities map to a user in Active Directory, but the PKWARE Enterprise
Manager also has an identity. Only the owner of a Smartkey is able to manage it, performing actions like modifying who
has access and changing it's name.

## I'm familiar with using RSA keys; what's the Smartcrypt equivalent?

Smartcrypt does not have a cryptographic primitive equivalent to RSA keys. Instead, Smartcrypt uses cryptographically
validated​ metadata to perform the most common task for which asymmetric encryption is used: managing access to the
symmetric key.

Smartkeys are distributed to authorized users automatically. When a user's access to a Smartkey is removed, the
Smartkey is automatically deleted from their systems. Additionally, rotatable Smartkeys are rotated, so a new key is
issued to all users still authorized to use the Smartkey, and all new encryptions are performed with the new key.

## What's the difference between rotatable and non-rotatable Smartkeys?

Smartkeys are versioned symmetric keys. When a Smartkey is rotated, a new key is issued and is attached to the Smartkey
as a version. Rotation typically happens when users are removed from the access control list for the Smartkey, but can
also be initiated for other reasons. Most Smartkeys are, and most scenarios want to use, rotatable Smartkeys.

There are certain scenarios, such as Format and Length Preserving Encryption, where key rotation is not acceptable. For
such scenarios, non-rotatable Smartkeys exist. Skipping key rotation weakens the long-term security of a Smartkey, so
be sure to read the [FLPE docs] before proceeding.

## Why are my Smartkeys not syncing to my client?

It's possible that the Application you are using does not have a mapping for the Smartkey `Kind`. See the
[PKWARE Enterprise Manager Documentation] for configuration details.

[FLPE docs]: /concepts/flpe
[PKWARE Enterprise Manager Documentation]: https://support.pkware.com/home/smar/latest/sem-reference/8-sdk/applications