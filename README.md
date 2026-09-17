<div align="center">
  <h1>RIVAN CYBER TRAINING INSTITUTE</h1>
  <h2>OpenPGP Email Encryption Lab</h2>
</div>

<hr>

Lab Objective

This laboratory demonstrates how to use Thunderbird OpenPGP to send an encrypted email so that only the intended recipient, who possesses the corresponding private/secret key, can decrypt and read the message.

Expected Result

The final setup should work as follows:

Sender (Anne)
    |
    | Encrypts using NAVS PUBLIC KEY
    v
Encrypted Email
    |
    v
Recipient (NAVS)
    |
    | Decrypts using NAVS PRIVATE KEY
    v
Readable Message

Important: Thunderbird normally decrypts an encrypted message automatically when the recipient's private key is available. Therefore, seeing readable plaintext in the recipient's Thunderbird window does not by itself mean the message was sent unencrypted.

Step 1: Open Thunderbird

Start the Windows virtual machine containing Thunderbird.

Open Thunderbird.

Make sure the appropriate email account is configured.

For this laboratory:

Anne: cs@bdo.ph.com
NAVS: navs@bdo.ph.com

Step 2: Open the OpenPGP Key Manager

In Thunderbird, open OpenPGP Key Manager.

Review the keys currently available.

The Key Manager may contain keys similar to:

Anne <cs@bdo.ph.com>
navs <navs@bdo.ph.com>

Important: Do not delete a secret/private key unless you have a backup. The private key is required to decrypt messages encrypted for that account.

Step 3: Understand the OpenPGP Keys

OpenPGP uses a public/private key pair.

Sender

The sender needs the recipient's public key to encrypt the message.

Recipient

The recipient needs the matching private/secret key to decrypt the message.

NAVS PUBLIC KEY
       |
       | Used by Anne to encrypt
       v
   ENCRYPTED EMAIL
       |
       | NAVS private key required
       v
NAVS PRIVATE KEY
       |
       v
   DECRYPTED EMAIL

Important: The public key can be shared. The private/secret key must remain protected and should not be sent to other users.

Step 4: Make the Recipient Public Key Available to the Sender

If Anne does not have NAVS's public key, Thunderbird will show an error similar to:

Cannot Encrypt

navs@bdo.ph.com
No key available.

This means Thunderbird cannot encrypt a message to NAVS because a usable NAVS public key is not available to Anne.

4.1 Export the NAVS Public Key

On the NAVS Thunderbird installation:

Open OpenPGP Key Manager.

Select the NAVS key.

Use the File menu.

Choose the option to Export Public Key(s) To File.

Save the public key as an .asc file.

Example:

navs-public.asc

Do not export or distribute the secret/private key. Only export the public key.

Step 5: Transfer the Public Key to the Anne VM

Move the exported .asc public-key file from the NAVS VM to the Anne VM.

Possible methods in a VMware laboratory include:

VMware Shared Folders

A laboratory file-transfer location

Another controlled method available between the VMs

Example file:

navs-public.asc

The .asc file is a public-key file. It is not the encrypted email itself.

Step 6: Import the NAVS Public Key into Anne's Thunderbird

On the Anne Thunderbird VM:

Open OpenPGP Key Manager.

Click File.

Select the option to Import Public Key(s) From File.

Select:

navs-public.asc

Confirm the import.

Verify that the NAVS public key now appears in Anne's OpenPGP Key Manager.

Step 7: Compose an Encrypted Email

On Anne's Thunderbird:

Click Write / Compose.

Enter:

To: navs@bdo.ph.com
Subject: OpenPGP Encryption Test

Enter a test message, for example:

This is an OpenPGP encryption test.

Enable Encrypt.

Thunderbird should identify the NAVS public key as the key used for encryption.

If Thunderbird reports:

No key available

verify that the NAVS public key has been imported correctly.

Step 8: Send the Encrypted Email

After Thunderbird confirms that encryption is available:

Click Send Encrypted.

The message is sent using OpenPGP encryption.

The recipient's corresponding private key is required to decrypt the message.

Important: Encryption protects the message contents while the message is being stored or transmitted. The recipient's Thunderbird may automatically decrypt it after receiving it if the required private key is available.

Step 9: Verify That the Message Is Encrypted

Open the received message in Thunderbird.

Look for the OpenPGP indicator or message security information.

A correctly encrypted message can show information similar to:

Message Is Encrypted

Thunderbird may also display information about the decryption key, such as:

Your decryption key:
0x...

If Thunderbird displays:

Good Digital Signature
Message Is Encrypted

the OpenPGP status information confirms that the message was encrypted.

Step 10: Understand the .asc Attachment

An email may display an attachment similar to:

OpenPGP_0xXXXXXXXXXXXX.asc

This .asc file is a public-key file.

It is not the encrypted email.

The encrypted email is handled by OpenPGP/PGP-MIME, while the .asc attachment contains a public key that can be imported by another user.

Example:

Email
├── OpenPGP encrypted message
└── OpenPGP_0xXXXXXXXXXXXX.asc
    └── Public key

Important: Do not mistake the .asc public-key attachment for the encrypted message itself.

Step 11: Test the Requirement That a Key Is Needed

To demonstrate that the recipient needs the private key:

Keep the recipient's public key available to the sender.

Send a new encrypted message to NAVS.

Make sure the NAVS Thunderbird profile does not have the corresponding private/secret key available.

Open the received message.

Without the corresponding private key, Thunderbird should not be able to decrypt the encrypted message.

Do not permanently delete a private key just for testing unless you have a backup. Removing the private key can make previously encrypted messages impossible to decrypt.

Step 12: Restore the Recipient's Private Key

After completing the test:

Restore/import the NAVS secret/private key from a trusted backup if it was removed.

Open the encrypted message again.

Thunderbird should be able to decrypt the message when the correct private key is available.

Troubleshooting

"Cannot Encrypt — No key available"

The sender does not currently have a usable public key for the recipient.

Check:

Anne Thunderbird
    ↓
OpenPGP Key Manager
    ↓
NAVS public key

Import the NAVS public key if it is missing.

The recipient can read the message immediately

This is normally expected when the recipient's private key is installed in Thunderbird.

Thunderbird automatically decrypts the message for the authorized recipient.

To test whether encryption is actually protecting the message, test with the recipient's private key unavailable.

The .asc attachment is missing

The .asc file is a public-key attachment, not the encrypted message.

If you specifically want Thunderbird to attach a public key to a message, check the account's OpenPGP/end-to-end encryption settings and the signing/public-key attachment options available in your Thunderbird version.

The presence or absence of this attachment does not determine whether the email itself is encrypted.

Final Verification Checklist

Before considering the laboratory complete, verify:

Anne has a working OpenPGP key pair.

NAVS has a working OpenPGP key pair.

Anne has NAVS's public key.

NAVS's private key is kept secret.

Anne can select Encrypt when composing to NAVS.

Thunderbird reports Message Is Encrypted for the received message.

NAVS can decrypt the message when the correct private key is available.

NAVS cannot decrypt the message when the corresponding private key is unavailable.

Any .asc attachment is understood as a public-key file, not the encrypted message.

<div align="center">

RIVAN CYBER TRAINING INSTITUTE

Cybersecurity Laboratory — OpenPGP Email Encryption

</div>
