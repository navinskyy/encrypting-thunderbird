<div align="center">

<img src="https://github.com/user-attachments/assets/e4d90e60-ee23-4e28-b9c1-ab35e68fed13" alt="Rivan Cyber Training Institute Logo" width="180">

RIVAN CYBER TRAINING INSTITUTE

Thunderbird

OpenPGP Email Encryption Lab

</div>

🎯 LAB OBJECTIVE

Use Thunderbird OpenPGP to send an encrypted email so that only the intended recipient, who has the matching private/secret key, can decrypt and read it.

Laboratory Accounts

Role

Account

Sender

cs@bdo.ph.com

Recipient

yourname@bdo.ph.com

Important: Thunderbird may automatically decrypt an encrypted message when the recipient's private key is available. Seeing readable text in the recipient's inbox does not by itself mean that the email was sent unencrypted.

🧪 PART 1 — OPEN THUNDERBIRD

Step 1 — Open Thunderbird

Start the Windows virtual machine containing Thunderbird.

Open Thunderbird.

Make sure the appropriate email account is configured.

Confirm that the laboratory accounts are available.

Sender: cs@bdo.ph.com
Recipient: yourname@bdo.ph.com

Tip: Verify the recipient address before sending an encrypted email.

🔑 PART 2 — CHECK OPENPGP KEYS

Step 2 — Open the OpenPGP Key Manager

In Thunderbird, open OpenPGP Key Manager.

Review the keys currently available.

Look for keys associated with:

cs <cs@bdo.ph.com>

yourname <yourname@bdo.ph.com>

Caution: Do not delete a secret/private key unless you have a backup. The private key is required to decrypt messages encrypted for that account.

🔐 PART 3 — UNDERSTAND OPENPGP KEYS

Step 3 — Understand the Public and Private Keys

OpenPGP uses a public/private key pair.

Public Key

The sender uses the recipient's public key to encrypt the message.

Private / Secret Key

The recipient uses the matching private/secret key to decrypt the message.

Key

Purpose

Sharing

Public key

Used to encrypt messages for the key owner

Can be shared

Private/secret key

Used to decrypt messages

Must remain protected

Key Rule:
Public key → Encrypt
Private key → Decrypt

🔑 PART 4 — EXCHANGE THE PUBLIC KEY

Step 4 — Make the Recipient Public Key Available to the Sender

If cs does not have yourname's public key, Thunderbird may display:

Cannot Encrypt
yourname@bdo.ph.com
No key available.

This means Thunderbird cannot encrypt a message to yourname because a usable public key is not available to cs.

Step 4.1 — Export the yourname Public Key

On the yourname Thunderbird installation:

Open OpenPGP Key Manager.

Select the yourname key.

Open the File menu.

Choose Export Public Key(s) To File.

Save the public key as an .asc file.

Example filename:

yourname-public.asc

Security Rule: Export only the public key. Never distribute the secret/private key.

Step 5 — Transfer the Public Key to the cs VM

Move the following file from the yourname VM to the cs VM:

yourname-public.asc

You can use a controlled VMware laboratory transfer method, such as:

VMware Shared Folders

A laboratory file-transfer location

Another controlled method available between the VMs

Important: The .asc file is a public-key file. It is not the encrypted email itself.

Step 6 — Import the yourname Public Key into cs's Thunderbird

On the cs Thunderbird VM:

Open OpenPGP Key Manager.

Click File.

Select Import Public Key(s) From File.

Select yourname-public.asc.

Confirm the import.

Verify that the yourname public key appears in the Key Manager.

Confirm that the key is associated with yourname@bdo.ph.com.

✉️ PART 5 — SEND AN ENCRYPTED EMAIL

Step 7 — Compose an Encrypted Email

On the cs Thunderbird VM:

Click Write / Compose.

Enter the recipient:

yourname@bdo.ph.com

Enter the subject:

OpenPGP Encryption Test

Enter the message:

This is an OpenPGP encryption test.

Enable Encrypt.

Confirm that Thunderbird identifies the yourname public key for encryption.

If "No Key Available" Appears

Open OpenPGP Key Manager.

Check that the yourname public key is present.

Confirm that it is associated with yourname@bdo.ph.com.

If the key is missing, import yourname-public.asc.

Step 8 — Send the Encrypted Email

Once Thunderbird confirms that encryption is available:

Review the recipient address.

Confirm that Encrypt is enabled.

Click Send Encrypted.

Wait for the message to be sent.

Open the recipient's Thunderbird account.

Important: The recipient's corresponding private key is required to decrypt the encrypted message.

🔎 PART 6 — VERIFY ENCRYPTION

Step 9 — Verify That the Message Is Encrypted

Open the received message in Thunderbird.

Look for the OpenPGP indicator.

Open the message security information if available.

Confirm that Thunderbird shows information similar to:

Message Is Encrypted

Thunderbird may also display information about the decryption key, such as:

Your decryption key: 0x...

A status such as:

Good Digital Signature

Message Is Encrypted

provides OpenPGP security information about the message.

Verification

Check

Expected Result

Recipient

yourname@bdo.ph.com

Encryption

Enabled

Message status

Message Is Encrypted

Decryption key

yourname private key

Message after decryption

Readable

📎 PART 7 — UNDERSTAND THE .ASC ATTACHMENT

Step 10 — Understand the .asc Attachment

An email may display an attachment similar to:

OpenPGP_0xXXXXXXXXXXXX.asc

This .asc file is a public-key file.

It is not the encrypted email.

The encrypted email is handled by OpenPGP/PGP-MIME, while the .asc attachment contains a public key that can be imported by another user.

Public Key vs. Encrypted Email

Item

Purpose

.asc file

Public-key file

Encrypted email

Protected message contents

Public key

Used to encrypt

Private key

Used to decrypt

Important: Do not confuse the .asc public-key attachment with the encrypted message itself.

🧩 PART 8 — TEST THE PRIVATE KEY REQUIREMENT

Step 11 — Demonstrate That a Private Key Is Required

This test demonstrates that the recipient needs the matching private key to decrypt an encrypted message.

Keep the recipient's public key available to the sender.

Send a new encrypted message to yourname.

Make sure the yourname Thunderbird profile does not have the corresponding private/secret key available.

Open the received encrypted message.

Observe whether Thunderbird can decrypt the message.

Expected Result

Without the corresponding private key, Thunderbird should not be able to decrypt the encrypted message.

With the correct private key available, Thunderbird should be able to decrypt the message.

Caution: Do not permanently delete a private key just for testing unless you have a backup. Removing the private key can make previously encrypted messages impossible to decrypt.

♻️ PART 9 — RESTORE THE PRIVATE KEY

Step 12 — Restore the Recipient's Private Key

After completing the test:

Restore/import the yourname secret/private key from a trusted backup if it was removed.

Open the encrypted message again.

Confirm that Thunderbird can decrypt the message when the correct private key is available.

🛠️ PART 10 — TROUBLESHOOTING

Problem 1 — "Cannot Encrypt — No key available"

Cause: The sender does not currently have a usable public key for the recipient.

Check

Open Thunderbird on the cs account.

Open OpenPGP Key Manager.

Confirm that the yourname public key is present.

Confirm that the key is associated with yourname@bdo.ph.com.

Solution

Import the yourname public key if it is missing.

Problem 2 — The Recipient Can Read the Message Immediately

This is normally expected when the recipient's private key is installed in Thunderbird.

Thunderbird can automatically decrypt the message for the authorized recipient.

Important: Automatic decryption does not mean that the message was sent as plaintext.

To test encryption, make the recipient's private key unavailable and examine the encrypted message.

Problem 3 — The .asc Attachment Is Missing

The .asc file is a public-key attachment, not the encrypted message.

If you specifically want Thunderbird to attach a public key to a message:

Open the account's OpenPGP/end-to-end encryption settings.

Review the signing and public-key attachment options available in your Thunderbird version.

Enable the appropriate public-key attachment option if available.

Important: The presence or absence of an .asc attachment does not determine whether the email itself is encrypted.

✅ PART 11 — FINAL LAB CHECKLIST

Before Completing the Laboratory

cs has a working OpenPGP key pair.

yourname has a working OpenPGP key pair.

cs has yourname's public key.

yourname's private key is kept secret.

cs can select Encrypt when composing to yourname.

Thunderbird reports Message Is Encrypted.

yourname can decrypt the message when the correct private key is available.

yourname cannot decrypt the message when the corresponding private key is unavailable.

Any .asc attachment is understood as a public-key file, not the encrypted message.

📚 QUICK REFERENCE

Action

Key Required

Encrypt an email to yourname

yourname public key

Decrypt an email received by yourname

yourname private/secret key

Share with the sender

Public key

Keep protected

Private/secret key

🔐 REMEMBER

PUBLIC KEY → ENCRYPT
PRIVATE KEY → DECRYPT

<div align="center">

🛡️ RIVAN CYBER TRAINING INSTITUTE

Cybersecurity Laboratory — OpenPGP Email Encryption

Thunderbird • OpenPGP • Email Security

End of Laboratory Exercise

</div>
