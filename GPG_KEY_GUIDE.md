# Guide: How to Get GPG Key Name and GPG Private Key

This guide explains how to generate, export, and use a GPG key for publishing to Maven Central via GitHub Actions.

## What is a GPG Key?

GPG (GNU Privacy Guard) is a cryptographic software used to create digital signatures. Maven Central requires that all artifacts be signed with a GPG key to verify their authenticity.

- **GPG Key Name/ID**: A unique identifier for your GPG key (e.g., `F4DCF18FE1EC17172701813C03725B0EB6B46CD1`)
- **GPG Private Key**: The secret part of your key pair used for signing

## 1. Generate a New GPG Key Pair

If you don't already have a GPG key, you can generate one:

```bash
gpg --full-generate-key
```

Follow the prompts:
- Choose key type: RSA and RSA (default)
- Key size: 4096 bits (recommended)
- Key validity: 0 (key does not expire)
- Enter your user information (name, email, comment)
- Set a secure passphrase

## 2. Find Your GPG Key Name (Key ID)

To list your GPG keys and find the key ID:

```bash
gpg --list-keys --keyid-format LONG
```

Look for a line like this:
```
pub   rsa4096/F4DCF18FE1EC1717 2023-01-01 [SC]
```

The part after `rsa4096/` is your key ID (`F4DCF18FE1EC1717`). For Maven Central, you'll need the full fingerprint:

```bash
gpg --fingerprint your_email@example.com
```

This will show the full fingerprint (e.g., `F4DCF18FE1EC17172701813C03725B0EB6B46CD1`).

## 3. Export Your GPG Private Key

To export your private key in ASCII armor format (for GitHub Actions):

```bash
gpg --export-secret-keys --armor YOUR_KEY_ID > private-key.asc
```

Replace `YOUR_KEY_ID` with your actual key ID or email.

## 4. Configure Your Project

Update your `pom.xml` with your GPG key name:

```xml
<properties>
    <gpg.keyname>YOUR_KEY_ID</gpg.keyname>
</properties>
```

## 5. Set Up GitHub Repository Secrets

For the GitHub workflow to use your GPG key, add these secrets to your repository:

1. Go to your GitHub repository → Settings → Secrets and variables → Actions
2. Add the following secrets:
   - `GPG_PRIVATE_KEY`: The entire content of your `private-key.asc` file
   - `GPG_PASSPHRASE`: The passphrase you created for your GPG key
   - `GPG_KEYNAME`: Your full GPG key ID (e.g., `F4DCF18FE1EC17172701813C03725B0EB6B46CD1`)
   - `SONATYPE_USERNAME`: Your Sonatype username
   - `SONATYPE_PASSWORD`: Your Sonatype password

## 6. Publish Your GPG Public Key

For others to verify your signatures, publish your public key to a key server:

```bash
gpg --keyserver keyserver.ubuntu.com --send-keys YOUR_KEY_ID
```

Also publish to other key servers for redundancy:
```bash
gpg --keyserver keys.openpgp.org --send-keys YOUR_KEY_ID
gpg --keyserver pgp.mit.edu --send-keys YOUR_KEY_ID
```

## 7. Verify Your Setup

The GitHub workflow will use these secrets to sign and publish your artifacts to Maven Central when you create a new release.

## Additional Resources

- [Sonatype Documentation on GPG](https://central.sonatype.org/publish/requirements/gpg/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [GPG Documentation](https://gnupg.org/documentation/)
