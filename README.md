<div align="center">

<img src="https://github.com/user-attachments/assets/e4d90e60-ee23-4e28-b9c1-ab35e68fed13" alt="Rivan Cyber Training Institute Logo" width="180">

# Thunderbird OpenPGP Email Encryption Lab

**Rivan Cyber Training Institute**

</div>

## Objective

Use Thunderbird's built-in OpenPGP support to configure two email accounts in a single Thunderbird installation and send an encrypted email from one account to the other.

The lab demonstrates:

- OpenPGP public/private key pairs
- Public-key exchange
- Email encryption
- Private-key-based decryption
- Thunderbird's OpenPGP security indicators

---

## Lab Environment

The lab uses **one Windows VM and one Thunderbird installation**, with two separate email accounts configured inside it.

| Role | Account |
|------|---------|
| Sender | `cs@bdo.ph.com` |
| Recipient | `emman@bdo.ph.com` |
| Virtual machines | 1 |
| Thunderbird installations | 1 |

```text
                ONE WINDOWS VM
                     │
                     ▼
              ┌─────────────┐
              │ Thunderbird │
              └──────┬──────┘
                     │
            ┌────────┴────────┐
            ▼                 ▼
     cs@bdo.ph.com      emman@bdo.ph.com
        SENDER              RECIPIENT
```

> **Note:** Both accounts live inside the same Thunderbird installation. There is no second VM involved in this lab.

---

## Part 1 — Open Thunderbird

<img width="700" alt="Thunderbird accounts" src="https://github.com/user-attachments/assets/8fb549b1-6461-49f5-82f8-53db59c3724f" />

1. Start the Windows VM.
2. Open Thunderbird.
3. Confirm that both email accounts are configured:
   - Sender: `cs@bdo.ph.com`
   - Recipient: `emman@bdo.ph.com`
4. Verify both accounts appear in the Thunderbird account list.

Because both accounts are inside the same Thunderbird installation, switching between sender and recipient is done directly within the app — no second machine required.

---

## Part 2 — Configure the Sender's OpenPGP Key (`cs@bdo.ph.com`)

<img width="975" height="731" alt="image" src="https://github.com/user-attachments/assets/536dc064-ca80-4fb7-af53-d71beb1ec6bc" />

Open **Account Settings → End-to-End Encryption** for the `cs` account.

If the account doesn't already have an OpenPGP key:

1. Click **Add Key...**
2. Select **Create a new OpenPGP key pair**.
3. Confirm the identity is `cs@bdo.ph.com`.
4. Configure the key:

   | Setting | Value |
   |---------|-------|
   | Key type | RSA |
   | Key size | 3072 bits |
   | Key expiration | 3 years |

5. Click **Generate Key**.
6. When prompted, choose **Yes, treat this key as a personal key**.

The CS account now has its own key pair (public + private).

<img width="975" height="709" alt="image" src="https://github.com/user-attachments/assets/387b6460-e2ea-4b01-81ea-a90d5b2cf958" />

---

## Part 3 — Configure the Recipient's OpenPGP Key (`emman@bdo.ph.com`)

<img width="975" height="731" alt="image" src="https://github.com/user-attachments/assets/22d7f201-b848-49a7-9bb5-0648b44e650c" />

Open **Account Settings → End-to-End Encryption** for the `emman` account and confirm it has its own OpenPGP key pair.

Example key details observed during this lab:

| Field | Value |
|-------|-------|
| Key ID | `0xB28EF8275BA88335` |
| Fingerprint | `C17A 9F14 EB53 55D3 A064 528F B28E F827 5BA8 8335` |
| Created | 09/16/2026 |
| Expires | 09/15/2029 |
| Associated identity | `emman@bdo.ph.com` |

---

## Part 4 — Public vs. Private Keys

OpenPGP uses asymmetric cryptography:

| Key | Function | Sharing |
|-----|----------|---------|
| Public key | Encrypts messages *for* the key owner | Can be shared |
| Private/secret key | Decrypts messages sent *to* the key owner | Must remain secret |

**Rule to remember:** Public key → Encrypt · Private key → Decrypt

For this lab (CS sending to emman):

```text
CS  --(uses emman public key)-->  Encrypted message  --(emman private key)-->  Readable message
```

---

## Part 5 — Make emman's Public Key Available to CS

Since both accounts live in the same Thunderbird installation, there's no need to transfer a file between machines — the key just needs to be visible in the shared Key Manager.

### Step 5.1 — Check the emman Key

Open **Account Settings → End-to-End Encryption** for `emman@bdo.ph.com` and confirm its OpenPGP key exists and is correctly associated with that address.

### Step 5.2 — Export the emman Public Key (if needed)

1. Open **OpenPGP Key Manager**.
2. Select the emman key.
3. Choose **Export Public Key(s)**.
4. Save it as `emman-public.asc`.

> **Security rule:** Export only the public key. Never distribute the secret/private key.

### Step 5.3 — Make the Key Available for CS to Use

Confirm in **OpenPGP Key Manager** that `emman@bdo.ph.com` is available as a recipient key.

**Important:** Do not replace CS's personal key with emman's public key. The CS account should end up holding:

```text
CS Account
├── CS personal key pair (public + private)
└── emman public key  →  used to encrypt mail TO emman
```

---

## Part 6 — Initial Encryption Problem

When first attempting to send an encrypted email, Thunderbird may show:

```text
Cannot Encrypt
emman@bdo.ph.com — No key available.
```

This means Thunderbird can't find a usable **public** key for `emman@bdo.ph.com`. It does **not** need emman's private key — only the public one.

---

## Part 7 — Compose the Encrypted Email

<img width="623" height="472" alt="image" src="https://github.com/user-attachments/assets/ed0855f6-b309-462e-83cb-d52641b54acd" />

Switch to the `cs@bdo.ph.com` account and click **Write / Compose**.

```text
From:    cs@bdo.ph.com
To:      emman@bdo.ph.com
Subject: OpenPGP Encryption Test
```

Example message:

> Hello emman,
>
> This is a test email for our OpenPGP encryption laboratory. The purpose of this message is to demonstrate encrypted email communication using Thunderbird.
>
> Test ID: PGP-001
>
> Regards,
> CS Lab

<img width="621" height="476" alt="image" src="https://github.com/user-attachments/assets/9c16c669-2d94-42d2-bf56-ef2e09491141" />

**or just send the website that you use for testing**
---

## Part 8 — Enable Encryption

<img width="546" height="58" alt="image" src="https://github.com/user-attachments/assets/13ab6b7b-65ed-46ee-b2cf-2a8f6f4cbd02" />


Before sending:

1. Verify the recipient: `emman@bdo.ph.com`
2. Click the **Encrypt 🔒** button.
3. Confirm Thunderbird recognizes the recipient's public key and the earlier **No key available** warning is gone.

```text
Plaintext → [emman public key] → Encrypted message → Email server
    → emman's Thunderbird → [emman private key] → Decrypted message
```

---

## Part 9 — Send the Message

<img width="975" height="511" alt="image" src="https://github.com/user-attachments/assets/196b5456-dbc9-44fd-8ed4-03dc192b04ed" />


1. Check the sender address.
2. Check the recipient address.
3. Confirm the encryption/lock indicator is enabled.
4. Send the email.

CS uses **emman's public key** to encrypt the message.

---

## Part 10 — Verify the Received Message

Switch Thunderbird from `cs@bdo.ph.com` to `emman@bdo.ph.com` and open the received message.

Thunderbird may auto-decrypt it, since emman's private key is available in the same installation — so the message may just look like normal readable text.

> **Important:** Readable text does not mean the message was sent unencrypted. Check the message's OpenPGP/security information — it should show **Message Is Encrypted**, along with details about the decryption key used.

---

## Part 11 — Public-Key / Private-Key Relationship

| Account | Public key | Private key |
|---------|-----------|-------------|
| `cs@bdo.ph.com` | Used by others to encrypt *to* CS | Used by CS to decrypt |
| `emman@bdo.ph.com` | Used by CS to encrypt *to* emman | Used by emman to decrypt |

**CS → emman:** `CS → (encrypt with emman public key) → encrypted email → (emman private key) → readable email`

**emman → CS:** `emman → (encrypt with CS public key) → encrypted email → (CS private key) → readable email`

---

## Part 12 — The `.asc` Public-Key File

An exported OpenPGP public key looks like `emman-public.asc`.

| Item | Purpose |
|------|---------|
| `emman-public.asc` | emman's public-key file |
| emman public key | Encrypts messages to emman |
| emman private key | Decrypts messages sent to emman |
| Encrypted email | The protected message contents |

> The `.asc` file is **not** the encrypted email — it's just the key.

---

## Part 13 — Test the Private-Key Requirement

Purpose: prove that the public key alone cannot decrypt a message.

1. Send an encrypted message from `cs@bdo.ph.com` to `emman@bdo.ph.com` (encrypted with emman's public key).
2. Temporarily make emman's private key unavailable.
3. Try to open the encrypted message.

**Expected result:**
- Without emman's private key → decryption **fails**.
- With emman's private key available → decryption **succeeds**, message is readable.

> **Caution:** Don't permanently delete the private key for this test — back it up first, or use another safe method to make it temporarily unavailable.

---

## Part 14 — Troubleshooting

**"Cannot Encrypt — No key available"**
- *Cause:* Thunderbird can't find a usable public key for `emman@bdo.ph.com`.
- *Fix:* Open OpenPGP Key Manager, confirm the emman key's email identity and fingerprint, and import/refresh the public key for the CS account if needed.

**Importing emman's public key gives an error**
- Make sure you're not trying to import emman's public-only key as a *personal* key for CS — CS already has its own personal key pair. emman's key should sit alongside it as a recipient-only key:
  ```text
  CS Personal Key: CS Public Key + CS Private Key
  Other/Recipient Keys: emman Public Key
  ```

**The received email is readable right away**
- Expected when Thunderbird has emman's private key locally and auto-decrypts. Readable message ≠ plaintext email — verify via the message's OpenPGP/security info.

**Encrypt button is unavailable**
- Check that: CS has a personal key, emman has a key, emman's public key is available to Thunderbird, the recipient address is exactly `emman@bdo.ph.com`, the key isn't expired, and there's no "No key available" warning.

---

## Part 15 — Final Lab Checklist

- [ ] One Windows VM was used
- [ ] One Thunderbird installation was used
- [ ] `cs@bdo.ph.com` configured as sender
- [ ] `emman@bdo.ph.com` configured as recipient
- [ ] CS has a personal OpenPGP key pair
- [ ] emman has a personal OpenPGP key pair
- [ ] emman's public key is available to Thunderbird
- [ ] emman's private key remains protected
- [ ] CS can select **Encrypt** when sending to emman
- [ ] Thunderbird recognizes emman's public key
- [ ] An encrypted email was sent from CS to emman
- [ ] Thunderbird reports the received message as encrypted
- [ ] emman can decrypt the message with its private key
- [ ] The public/private key distinction was demonstrated
- [ ] The `.asc` file was correctly identified as a public-key file, not the message

---

## Quick Reference

| Action | Key Required |
|--------|--------------|
| CS encrypts email to emman | emman **public** key |
| emman decrypts email | emman **private** key |
| emman encrypts email to CS | CS **public** key |
| CS decrypts email | CS **private** key |
| Public key | Safe to distribute |
| Private key | Keep secret |

**Remember:** 🔓 Public key → Encrypt · 🔐 Private key → Decrypt

---

## Lab Result

This lab demonstrated OpenPGP email encryption using **two separate email accounts within a single Thunderbird installation**.

The sender (`cs@bdo.ph.com`) used the recipient's (`emman@bdo.ph.com`) **public key** to encrypt the email. Thunderbird then used the recipient's corresponding **private key**, available locally, to decrypt the message.

```text
             ONE THUNDERBIRD INSTALLATION
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
       cs@bdo.ph.com          emman@bdo.ph.com
          SENDER                  RECIPIENT
              │                     ▲
              │   emman PUBLIC KEY   │
              ├────────────────────►│
              ▼                     │
       ENCRYPTED EMAIL ─────────────┘
                                    │
                              emman PRIVATE KEY
                                    ▼
                             READABLE MESSAGE
```

<div align="center">

*Rivan Cyber Training Institute — Cybersecurity Laboratory*
*Thunderbird · OpenPGP · Email Security*

</div>
