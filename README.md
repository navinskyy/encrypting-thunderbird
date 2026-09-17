<div align="center">

🛡️ RIVAN CYBER TRAINING INSTITUTE

OpenPGP Email Encryption Lab

Cybersecurity Laboratory • Thunderbird • OpenPGP

</div>

[!NOTE]
Lab Focus: Using Thunderbird OpenPGP to encrypt an email so that only the intended recipient, who possesses the corresponding private/secret key, can decrypt and read the message.

🎯 Lab Objective

This laboratory demonstrates how to use Thunderbird OpenPGP to send an encrypted email.

The core concept is simple:

┌──────────────┐
│   Sender     │
│      cs      │
└──────┬───────┘
       │
       │ Encrypts using
       │ yourname PUBLIC KEY
       ▼
┌──────────────────────┐
│   ENCRYPTED EMAIL    │
└──────────┬───────────┘
           │
           │ Decrypts using
           │ yourname PRIVATE KEY
           ▼
┌──────────────────────┐
│   READABLE MESSAGE   │
│      yourname        │
└──────────────────────┘

Expected Result

Role

Account

Key Used

Sender

cs@bdo.ph.com

yourname's public key

Recipient

yourname@bdo.ph.com

yourname's private/secret key

[!IMPORTANT]
Thunderbird normally decrypts an encrypted message automatically when the recipient's private key is available. Therefore, seeing readable plaintext in the recipient's Thunderbird window does not by itself mean the message was sent unencrypted.

🧪 Laboratory Procedure

Step 1 — Open Thunderbird

Start the Windows virtual machine containing Thunderbird.

Open Thunderbird.

Make sure the appropriate email account is configured.

Confirm the laboratory accounts.

Laboratory Accounts

Sender:
  cs@bdo.ph.com

Recipient:
  yourname@bdo.ph.com

[!TIP]
Verify the To: address carefully before sending an encrypted message. Thunderbird needs a usable public key that matches the recipient's address.

Step 2 — Open the OpenPGP Key Manager

In Thunderbird, open OpenPGP Key Manager.

Review the keys currently available.

The Key Manager may contain keys similar to:

cs <cs@bdo.ph.com>
yourname <yourname@bdo.ph.com>

🔐 Key Safety

[!CAUTION]
Do not delete a secret/private key unless you have a backup. The private key is required to decrypt messages encrypted for that account.

Step 3 — Understand the OpenPGP Keys

OpenPGP uses a public/private key pair.

Public Key

The sender needs the recipient's public key to encrypt the message.

Private / Secret Key

The recipient needs the matching private/secret key to decrypt the message.

Encryption Flow

             YOURNAME PUBLIC KEY
                      │
                      │ Used by cs
                      ▼
              ┌───────────────┐
              │ ENCRYPTED     │
              │ EMAIL         │
              └───────┬───────┘
                      │
                      │ Requires matching
                      │ private/secret key
                      ▼
             YOURNAME PRIVATE KEY
                      │
                      ▼
              ┌───────────────┐
              │ DECRYPTED     │
              │ EMAIL         │
              └───────────────┘

[!IMPORTANT]
The public key can be shared. The private/secret key must remain protected and should not be sent to other users.

🔑 Public-Key Exchange

Step 4 — Make the Recipient Public Key Available to the Sender

If cs does not have yourname's public key, Thunderbird may display an error similar to:

Cannot Encrypt

yourname@bdo.ph.com
No key available.

This means Thunderbird cannot encrypt a message to yourname because a usable yourname public key is not available to cs.

4.1 — Export the yourname Public Key

On the yourname Thunderbird installation:

Open OpenPGP Key Manager.

Select the yourname key.

Open the File menu.

Choose the option to Export Public Key(s) To File.

Save the public key as an .asc file.

Example:

yourname-public.asc

[!CAUTION]
Only export the public key. Do not export or distribute the secret/private key.

Step 5 — Transfer the Public Key to the cs VM

Move the exported .asc public-key file from the yourname VM to the cs VM.

Possible methods in a VMware laboratory include:

VMware Shared Folders

A laboratory file-transfer location

Another controlled method available between the VMs

Example:

yourname-public.asc

[!NOTE]
The .asc file is a public-key file. It is not the encrypted email itself.

Step 6 — Import the yourname Public Key into cs's Thunderbird

On the cs Thunderbird VM:

Open OpenPGP Key Manager.

Click File.

Select Import Public Key(s) From File.

Select:

yourname-public.asc

Confirm the import.

Verify that the yourname public key appears in the OpenPGP Key Manager.

Verification

You should be able to locate a key associated with:

yourname@bdo.ph.com

✉️ Sending the Encrypted Email

Step 7 — Compose an Encrypted Email

On the cs Thunderbird VM:

Click Write / Compose.

Enter the recipient:

To: yourname@bdo.ph.com

Enter the subject:

Subject: OpenPGP Encryption Test

Enter a test message:

This is an OpenPGP encryption test.

Enable Encrypt.

Thunderbird should identify the yourname public key as the key used for encryption.

If Thunderbird Reports "No Key Available"

Verify:

cs Thunderbird
       │
       ▼
OpenPGP Key Manager
       │
       ▼
yourname public key
       │
       ▼
yourname@bdo.ph.com

If the key is missing, import the yourname-public.asc file again.

Step 8 — Send the Encrypted Email

After Thunderbird confirms that encryption is available:

Review the recipient.

Confirm that Encrypt is enabled.

Click Send Encrypted.

The message is sent using OpenPGP encryption.

cs
 │
 │ Encrypt
 │
 ▼
yourname PUBLIC KEY
 │
 ▼
ENCRYPTED EMAIL
 │
 ▼
yourname PRIVATE KEY
 │
 ▼
READABLE MESSAGE

[!IMPORTANT]
The recipient's corresponding private key is required to decrypt the message.

[!NOTE]
Encryption protects the message contents while the message is being stored or transmitted. The recipient's Thunderbird may automatically decrypt it after receiving it if the required private key is available.

🔎 Verification

Step 9 — Verify That the Message Is Encrypted

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

Verification Checklist

Check

Expected

Recipient address

yourname@bdo.ph.com

Encryption enabled

✅ Yes

Message status

Message Is Encrypted

Recipient private key

Available to decrypt

Decrypted message

Readable by authorized recipient

📎 Understanding .asc Attachments

Step 10 — Understand the .asc Attachment

An email may display an attachment similar to:

OpenPGP_0xXXXXXXXXXXXX.asc

This .asc file is a public-key file.

It is not the encrypted email.

The encrypted email is handled by OpenPGP/PGP-MIME, while the .asc attachment contains a public key that can be imported by another user.

Example Structure

Email
├── OpenPGP encrypted message
└── OpenPGP_0xXXXXXXXXXXXX.asc
    └── Public key

[!IMPORTANT]
Do not mistake the .asc public-key attachment for the encrypted message itself.

Public Key vs. Encrypted Message

Item

Purpose

.asc public-key file

Shares/imports a public key

Encrypted email

Protects the message contents

Public key

Used to encrypt

Private/secret key

Used to decrypt

🧩 Key-Dependency Test

Step 11 — Test the Requirement That a Key Is Needed

This step demonstrates that the recipient needs the matching private key to decrypt the encrypted message.

Procedure

Keep the recipient's public key available to the sender.

Send a new encrypted message to yourname.

Make sure the yourname Thunderbird profile does not have the corresponding private/secret key available.

Open the received message.

Observe whether Thunderbird can decrypt the message.

Expected Behavior

Without the corresponding private key:

ENCRYPTED MESSAGE
       │
       │ No matching private key
       ▼
   CANNOT DECRYPT

With the corresponding private key:

ENCRYPTED MESSAGE
       │
       │ Matching private key
       ▼
     DECRYPT
       │
       ▼
READABLE MESSAGE

[!CAUTION]
Do not permanently delete a private key just for testing unless you have a backup. Removing the private key can make previously encrypted messages impossible to decrypt.

♻️ Key Restoration

Step 12 — Restore the Recipient's Private Key

After completing the test:

Restore/import the yourname secret/private key from a trusted backup if it was removed.

Open the encrypted message again.

Confirm that Thunderbird can decrypt the message when the correct private key is available.

Expected Result

yourname PRIVATE KEY
        │
        ▼
Decrypt OpenPGP Message
        │
        ▼
Readable Message

🛠️ Troubleshooting

"Cannot Encrypt — No key available"

Cause

The sender does not currently have a usable public key for the recipient.

Check

cs Thunderbird
      │
      ▼
OpenPGP Key Manager
      │
      ▼
yourname public key

Solution

Import the yourname public key if it is missing.

The Recipient Can Read the Message Immediately

Cause

This is normally expected when the recipient's private key is installed in Thunderbird.

Thunderbird can automatically decrypt the message for the authorized recipient.

Test

To test whether encryption is actually protecting the message, test with the recipient's private key unavailable.

[!NOTE]
Automatic decryption in Thunderbird does not mean the message was sent as plaintext.

The .asc Attachment Is Missing

The .asc file is a public-key attachment, not the encrypted message.

If you specifically want Thunderbird to attach a public key to a message, check the account's OpenPGP/end-to-end encryption settings and the signing/public-key attachment options available in your Thunderbird version.

[!IMPORTANT]
The presence or absence of this attachment does not determine whether the email itself is encrypted.

✅ Final Verification Checklist

Before considering the laboratory complete, verify each item:

cs has a working OpenPGP key pair.

yourname has a working OpenPGP key pair.

cs has yourname's public key.

yourname's private key is kept secret.

cs can select Encrypt when composing to yourname.

Thunderbird reports Message Is Encrypted for the received message.

yourname can decrypt the message when the correct private key is available.

yourname cannot decrypt the message when the corresponding private key is unavailable.

Any .asc attachment is understood as a public-key file, not the encrypted message.

📚 Quick Reference

OpenPGP Encryption at a Glance

┌──────────────────────────────────────────────────────────┐
│                    OPENPGP WORKFLOW                      │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  1. yourname creates/owns a key pair                    │
│                                                          │
│       PUBLIC KEY  ─────────────► shared                  │
│       PRIVATE KEY ─────────────► protected               │
│                                                          │
│  2. cs obtains yourname's PUBLIC KEY                    │
│                                                          │
│  3. cs writes an email                                  │
│                                                          │
│  4. Thunderbird encrypts using yourname's PUBLIC KEY    │
│                                                          │
│  5. Encrypted message is delivered                      │
│                                                          │
│  6. yourname uses the matching PRIVATE KEY              │
│     to decrypt the message                              │
│                                                          │
└──────────────────────────────────────────────────────────┘

Key Rule

Public key → encrypt

Private/secret key → decrypt

<div align="center">

🛡️ RIVAN CYBER TRAINING INSTITUTE

Cybersecurity Laboratory — OpenPGP Email Encryption

End of Laboratory Exercise

</div>
