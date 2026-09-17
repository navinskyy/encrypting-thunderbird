<div align="center">

🛡️ RIVAN CYBER TRAINING INSTITUTE

🔐 OpenPGP Email Encryption Lab

THUNDERBIRD • OPENPGP • EMAIL SECURITY

Laboratory Guide
cs → yourname

</div>

<br>

🎯 LAB OBJECTIVE

Use Thunderbird OpenPGP to send an encrypted email so that only the intended recipient, who has the matching private/secret key, can decrypt and read it.

🧭 At a Glance

👤 Sender

📧 Recipient

🔑 Encryption Key

🔓 Decryption Key

cs

yourname

yourname PUBLIC KEY

yourname PRIVATE KEY

cs@bdo.ph.com

yourname@bdo.ph.com

Shared

Protected

🔐 The Core Concept

        ┌──────────────┐
        │      cs      │
        │    SENDER    │
        └──────┬───────┘
               │
               │ Encrypt with
               │ YOURNAME PUBLIC KEY
               ▼
      ╔══════════════════════╗
      ║   🔒 ENCRYPTED EMAIL ║
      ╚══════════╤═══════════╝
                 │
                 │ Decrypt with
                 │ YOURNAME PRIVATE KEY
                 ▼
        ┌─────────────────┐
        │   📖 READABLE   │
        │     MESSAGE     │
        └─────────────────┘

💡 Remember: Thunderbird may automatically decrypt an encrypted message when the recipient's private key is available. Seeing readable text in the recipient's inbox does not by itself mean the email was sent unencrypted.

🧪 LABORATORY PROCEDURE

01 · Open Thunderbird

Start the Windows virtual machine containing Thunderbird.

Do this

Open Thunderbird.

Make sure the appropriate email account is configured.

Confirm the laboratory accounts below.

┌─────────────────────────────────────────┐
│  SENDER                                  │
│  cs@bdo.ph.com                           │
│                                         │
│  RECIPIENT                               │
│  yourname@bdo.ph.com                     │
└─────────────────────────────────────────┘

💡 Tip: Always verify the recipient address before sending an encrypted message.

02 · Open the OpenPGP Key Manager

In Thunderbird, open OpenPGP Key Manager.

Review the keys currently available.

You may see entries similar to:

🔑 cs <cs@bdo.ph.com>
🔑 yourname <yourname@bdo.ph.com>

🔴 Key Safety

⚠️ CAUTION

Do not delete a secret/private key unless you have a backup.
The private key is required to decrypt messages encrypted for that account.

03 · Understand the OpenPGP Keys

OpenPGP uses a public/private key pair.

🔑 Key

Purpose

Can it be shared?

Public key

Encrypt messages for the owner

✅ Yes

Private / secret key

Decrypt messages for the owner

❌ No

Encryption Flow

       YOURNAME PUBLIC KEY
                │
                │  🔒 Encrypt
                ▼
      ┌───────────────────┐
      │  ENCRYPTED EMAIL  │
      └─────────┬─────────┘
                │
                │  🔓 Decrypt
                │
                ▼
       YOURNAME PRIVATE KEY
                │
                ▼
       ┌───────────────────┐
       │  READABLE EMAIL   │
       └───────────────────┘

🔐 Key Rule

PUBLIC KEY → ENCRYPT
PRIVATE KEY → DECRYPT

🔑 PUBLIC-KEY EXCHANGE

04 · Make the Recipient Public Key Available to the Sender

If cs does not have yourname's public key, Thunderbird may display:

╔══════════════════════════════════╗
║        Cannot Encrypt            ║
║                                  ║
║  yourname@bdo.ph.com             ║
║  No key available.               ║
╚══════════════════════════════════╝

This means Thunderbird cannot encrypt the message because a usable yourname public key is not available to cs.

4.1 · Export the yourname Public Key

On the yourname Thunderbird installation:

Open OpenPGP Key Manager.

Select the yourname key.

Open File.

Choose Export Public Key(s) To File.

Save the key as an .asc file.

Example:

yourname-public.asc

🛡️ Security Rule

Export only the public key.
Never distribute the secret/private key.

05 · Transfer the Public Key to the cs VM

Move:

yourname-public.asc

from the yourname VM to the cs VM.

VMware Transfer Options

📁 VMware Shared Folders

📂 Laboratory file-transfer location

🔄 Another controlled transfer method between the VMs

ℹ️ Important

The .asc file is a public-key file.
It is not the encrypted email itself.

06 · Import the yourname Public Key into cs's Thunderbird

On the cs Thunderbird VM:

Open OpenPGP Key Manager.

Click File.

Select Import Public Key(s) From File.

Select:

yourname-public.asc

Confirm the import.

Verify that the yourname public key appears.

✅ Checkpoint

You should be able to locate a key associated with:

yourname@bdo.ph.com

✉️ SEND AN ENCRYPTED EMAIL

07 · Compose an Encrypted Email

On the cs Thunderbird VM:

Field

Enter

To:

yourname@bdo.ph.com

Subject:

OpenPGP Encryption Test

Message:

This is an OpenPGP encryption test.

Steps

Click Write / Compose.

Enter the recipient.

Enter the subject.

Enter the test message.

Enable Encrypt.

Confirm that Thunderbird identifies the yourname public key for encryption.

If "No Key Available" Appears

Check:

cs Thunderbird
      │
      ▼
OpenPGP Key Manager
      │
      ▼
yourname PUBLIC KEY
      │
      ▼
yourname@bdo.ph.com

If the key is missing, import yourname-public.asc.

08 · Send the Encrypted Email

Once Thunderbird confirms that encryption is available:

Review the recipient.

Confirm Encrypt is enabled.

Click Send Encrypted.

┌──────┐
│  cs  │
└──┬───┘
   │
   │ 🔒 Encrypt
   ▼
┌───────────────────────┐
│ YOURNAME PUBLIC KEY   │
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│ 🔒 ENCRYPTED EMAIL    │
└───────────┬───────────┘
            │
            │ 🔓 Decrypt
            ▼
┌───────────────────────┐
│ YOURNAME PRIVATE KEY  │
└───────────┬───────────┘
            │
            ▼
       📖 READABLE
          MESSAGE

🔐 The recipient's corresponding private key is required to decrypt the message.

🔎 VERIFICATION

09 · Verify That the Message Is Encrypted

Open the received message in Thunderbird.

Look for the OpenPGP indicator or message security information.

A correctly encrypted message may show:

🔒 Message Is Encrypted

Thunderbird may also display information about the decryption key:

Your decryption key:
0x...

A status such as:

✓ Good Digital Signature
🔒 Message Is Encrypted

provides OpenPGP security information confirming encryption.

✅ Verification Table

Test

Expected Result

Recipient

yourname@bdo.ph.com

Encryption enabled

✅ Yes

Message status

🔒 Message Is Encrypted

Decryption key

yourname private key

Message after decryption

📖 Readable

📎 UNDERSTANDING .ASC FILES

10 · The .asc Attachment

An email may display an attachment similar to:

📎 OpenPGP_0xXXXXXXXXXXXX.asc

This .asc file is a public-key file.

It is not the encrypted email.

Email Structure

📧 EMAIL
│
├── 🔒 OpenPGP encrypted message
│
└── 📎 OpenPGP_0xXXXXXXXXXXXX.asc
       └── 🔑 Public key

Item

Meaning

.asc file

Public-key file

Encrypted email

Protected message contents

Public key

Used to encrypt

Private key

Used to decrypt

⚠️ Do not confuse the .asc public-key attachment with the encrypted message itself.

🧩 KEY-DEPENDENCY TEST

11 · Demonstrate That a Private Key Is Required

This test demonstrates that the recipient needs the matching private key to decrypt the encrypted message.

Procedure

Keep the recipient's public key available to the sender.

Send a new encrypted message to yourname.

Make sure the yourname Thunderbird profile does not have the corresponding private/secret key available.

Open the received message.

Observe whether Thunderbird can decrypt it.

Expected Behavior

Without the private key:

🔒 ENCRYPTED MESSAGE
          │
          │ ❌ No matching private key
          ▼
     CANNOT DECRYPT

With the private key:

🔒 ENCRYPTED MESSAGE
          │
          │ 🔑 Matching private key
          ▼
        DECRYPT
          │
          ▼
     📖 READABLE
       MESSAGE

🚨 CAUTION

Do not permanently delete a private key just for testing unless you have a backup. Removing the private key can make previously encrypted messages impossible to decrypt.

♻️ RESTORE THE KEY

12 · Restore the Recipient's Private Key

After completing the test:

Restore/import the yourname secret/private key from a trusted backup if it was removed.

Open the encrypted message again.

Confirm that Thunderbird can decrypt the message.

Expected Result

🔑 YOURNAME PRIVATE KEY
          │
          ▼
   🔓 Decrypt Message
          │
          ▼
   📖 Readable Message

🛠️ TROUBLESHOOTING

❌ "Cannot Encrypt — No key available"

Cause: The sender does not have a usable public key for the recipient.

Check

cs Thunderbird
      ↓
OpenPGP Key Manager
      ↓
yourname PUBLIC KEY

Solution: Import the yourname public key if it is missing.

👀 "The recipient can read the message immediately"

Cause: This is normally expected when the recipient's private key is installed in Thunderbird.

Thunderbird can automatically decrypt the message for the authorized recipient.

ℹ️ Automatic decryption does not mean the message was sent as plaintext.

Test: Make the recipient's private key unavailable and examine the behavior of the encrypted message.

📎 "The .asc attachment is missing"

The .asc file is a public-key attachment, not the encrypted message.

If you specifically want Thunderbird to attach a public key to a message, check the account's OpenPGP/end-to-end encryption settings and the signing/public-key attachment options available in your Thunderbird version.

🔐 The presence or absence of an .asc attachment does not determine whether the email itself is encrypted.

✅ FINAL LAB CHECKLIST

Before completing the laboratory

🔑 cs has a working OpenPGP key pair.

🔑 yourname has a working OpenPGP key pair.

📤 cs has yourname's public key.

🛡️ yourname's private key is kept secret.

🔒 cs can select Encrypt when composing to yourname.

✅ Thunderbird reports Message Is Encrypted.

🔓 yourname can decrypt the message with the correct private key.

🚫 yourname cannot decrypt it when the corresponding private key is unavailable.

📎 Any .asc attachment is understood as a public-key file, not the encrypted message.

📚 QUICK REFERENCE

The Three Things to Remember

┌────────────────────────────────────────────────────────┐
│                    🔐 OPENPGP                          │
├────────────────────────────────────────────────────────┤
│                                                        │
│  ① PUBLIC KEY                                          │
│     ↓                                                  │
│     Used by the sender to ENCRYPT                     │
│                                                        │
│  ② ENCRYPTED MESSAGE                                   │
│     ↓                                                  │
│     Can only be opened with the matching secret key   │
│                                                        │
│  ③ PRIVATE / SECRET KEY                                │
│     ↓                                                  │
│     Used by the recipient to DECRYPT                  │
│                                                        │
└────────────────────────────────────────────────────────┘

🔑 KEY RULE

PUBLIC KEY → 🔒 ENCRYPT

PRIVATE KEY → 🔓 DECRYPT

<div align="center">

🛡️ RIVAN CYBER TRAINING INSTITUTE

Cybersecurity Laboratory — OpenPGP Email Encryption

THUNDERBIRD • OPENPGP • EMAIL SECURITY

End of Laboratory Exercise

</div>
