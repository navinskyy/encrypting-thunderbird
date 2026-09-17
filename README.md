# Thunderbird OpenPGP Email Encryption Lab

**Rivan Cyber Training Institute**

## Objective

Use Thunderbird's built-in OpenPGP support to send an encrypted email so that only the intended recipient — the one holding the matching private key — can decrypt and read it.

## Lab Accounts

| Role | Account |
|------|---------|
| Sender | `cs@bdo.ph.com` |
| Recipient | `yourname@bdo.ph.com` |

> **Note:** Thunderbird may automatically decrypt a message when the recipient's private key is available locally. Seeing readable text in the recipient's inbox does not by itself prove the email was sent unencrypted.

---

## Part 1 — Open Thunderbird

1. Start the Windows VM containing Thunderbird.
2. Open Thunderbird and confirm the account is configured.
3. Verify both lab accounts are available:
   - Sender: `cs@bdo.ph.com`
   - Recipient: `yourname@bdo.ph.com`

**Tip:** Double-check the recipient address before sending an encrypted email.

---

## Part 2 — Check OpenPGP Keys

1. Open **OpenPGP Key Manager** in Thunderbird.
2. Review the keys currently available, looking for:
   - `cs <cs@bdo.ph.com>`
   - `yourname <yourname@bdo.ph.com>`

> **Caution:** Do not delete a secret/private key unless you have a backup. The private key is required to decrypt any message encrypted for that account.

---

## Part 3 — Public vs. Private Keys

OpenPGP uses a public/private key pair:

| Key | Purpose | Sharing |
|-----|---------|---------|
| Public key | Encrypts messages *to* the key owner | Can be shared freely |
| Private / secret key | Decrypts messages | Must stay protected |

**Rule to remember:** Public key → Encrypt · Private key → Decrypt

---

## Part 4 — Exchange the Public Key

If `cs` doesn't yet have `yourname`'s public key, Thunderbird will show:

```
Cannot Encrypt
yourname@bdo.ph.com — No key available.
```

### Step 4.1 — Export yourname's Public Key

On the **yourname** Thunderbird installation:

1. Open **OpenPGP Key Manager**.
2. Select the `yourname` key.
3. Go to **File → Export Public Key(s) To File**.
4. Save it as `yourname-public.asc`.

> **Security rule:** Export only the public key. Never distribute the secret/private key.

### Step 4.2 — Transfer the Key to the cs VM

Move `yourname-public.asc` from the `yourname` VM to the `cs` VM using a controlled lab method, e.g.:

- VMware Shared Folders
- A designated lab file-transfer location
- Another approved transfer method

> The `.asc` file is a public-key file — it is **not** the encrypted email itself.

### Step 4.3 — Import the Key into cs's Thunderbird

On the **cs** Thunderbird VM:

1. Open **OpenPGP Key Manager**.
2. Go to **File → Import Public Key(s) From File**.
3. Select `yourname-public.asc` and confirm the import.
4. Verify the key now appears in Key Manager, associated with `yourname@bdo.ph.com`.

---

## Part 5 — Send an Encrypted Email

### Step 5.1 — Compose the Message

On the **cs** Thunderbird VM:

1. Click **Write / Compose**.
2. **To:** `yourname@bdo.ph.com`
3. **Subject:** `OpenPGP Encryption Test`
4. **Body:** `This is an OpenPGP encryption test.`
5. Enable **Encrypt**.
6. Confirm Thunderbird recognizes `yourname`'s public key for encryption.

If **"No Key Available"** appears:
- Open OpenPGP Key Manager and confirm the `yourname` public key is present and associated with the correct address.
- If missing, re-import `yourname-public.asc`.

### Step 5.2 — Send

1. Review the recipient address.
2. Confirm **Encrypt** is enabled.
3. Click **Send Encrypted**.
4. Wait for the message to send, then open the recipient's Thunderbird account.

> The recipient's matching private key is required to decrypt this message.

---

## Part 6 — Verify Encryption

1. Open the received message.
2. Look for the OpenPGP indicator and check the message security details.
3. Confirm Thunderbird reports something like:
   - `Message Is Encrypted`
   - `Your decryption key: 0x...`
   - `Good Digital Signature`

| Check | Expected Result |
|-------|-----------------|
| Recipient | `yourname@bdo.ph.com` |
| Encryption | Enabled |
| Message status | Message Is Encrypted |
| Decryption key | yourname's private key |
| Message after decryption | Readable |

---

## Part 7 — The `.asc` Attachment

You may see an attachment like `OpenPGP_0xXXXXXXXXXXXX.asc` on a message.

| Item | Purpose |
|------|---------|
| `.asc` file | Public-key file |
| Encrypted email | Protected message contents (handled via PGP/MIME) |
| Public key | Used to encrypt |
| Private key | Used to decrypt |

> Don't confuse the `.asc` public-key attachment with the encrypted message itself.

---

## Part 8 — Test the Private Key Requirement

This demonstrates that decryption is impossible without the correct private key.

1. Keep the recipient's public key available to the sender.
2. Send a new encrypted message to `yourname`.
3. Ensure the `yourname` Thunderbird profile does **not** have the matching private key available.
4. Open the received encrypted message and observe whether Thunderbird can decrypt it.

**Expected result:**
- Without the private key → Thunderbird **cannot** decrypt the message.
- With the private key available → Thunderbird **can** decrypt the message.

> **Caution:** Don't permanently delete a private key for this test unless you have a backup — doing so can make previously encrypted messages unrecoverable.

---

## Part 9 — Restore the Private Key

1. Restore/import `yourname`'s secret key from a trusted backup if it was removed.
2. Reopen the encrypted message.
3. Confirm Thunderbird can now decrypt it with the correct private key in place.

---

## Part 10 — Troubleshooting

**"Cannot Encrypt — No key available"**
- *Cause:* `cs` doesn't currently have a usable public key for `yourname`.
- *Fix:* Open OpenPGP Key Manager on the `cs` account, confirm the `yourname` public key is present and correctly associated, and import it if missing.

**The recipient can read the message immediately**
- This is expected when the recipient's private key is installed locally — Thunderbird auto-decrypts for the authorized recipient.
- Automatic decryption does *not* mean the message was sent as plaintext.
- To verify encryption is actually happening, temporarily remove the private key and re-check the message (see Part 8).

**The `.asc` attachment is missing**
- The `.asc` file is only a public-key attachment, not the encrypted message itself.
- To have Thunderbird attach your public key automatically, check the account's OpenPGP / end-to-end encryption settings for a public-key attachment option and enable it if available.
- Whether or not the `.asc` attachment is present has no bearing on whether the email itself is encrypted.

---

## Part 11 — Final Lab Checklist

- [ ] `cs` has a working OpenPGP key pair
- [ ] `yourname` has a working OpenPGP key pair
- [ ] `cs` has `yourname`'s public key
- [ ] `yourname`'s private key is kept secret
- [ ] `cs` can select **Encrypt** when composing to `yourname`
- [ ] Thunderbird reports **Message Is Encrypted**
- [ ] `yourname` can decrypt the message when the correct private key is available
- [ ] `yourname` cannot decrypt the message when the private key is unavailable
- [ ] Any `.asc` attachment is correctly understood as a public-key file, not the encrypted message

---

## Quick Reference

| Action | Key Required |
|--------|--------------|
| Encrypt an email to `yourname` | `yourname`'s **public** key |
| Decrypt an email received by `yourname` | `yourname`'s **private/secret** key |
| Share with the sender | Public key |
| Keep protected, never share | Private/secret key |

**Remember:** Public key → Encrypt · Private key → Decrypt

---

*Rivan Cyber Training Institute — Cybersecurity Laboratory*
*Thunderbird · OpenPGP · Email Security*
