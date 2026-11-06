# Hypervault Next Gen

One self-contained HTML file that locks files and text into a password protected vault. The vault is itself an HTML
file that opens in any modern browser, fully offline. Based on [Hypervault](https://github.com/hypervault/hypervault.github.io)
by Caleb Madrigal.

## Verify your copy

Keep one verified copy of `Hypervault_next_gen.html` on a computer you trust and open vaults (for example from a USB
stick) with its tab **Open vault file**. The vault file is then only read as data and its code never runs, so a
modified vault cannot steal your password. Verify that trusted copy once, as described below.

Locked vaults cannot be verified this way: every vault contains its own encrypted data. That is why vaults should be
opened with the verified copy.

### Signing key

Every release is signed with this Ed25519 key:

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICvtErANjUZmrTKBrwmOEvRDEGB1gtQhIOK8e8cFkeKf hypervault NG release key
```

Fingerprint: `SHA256:/B7LQH3dL9DYM7/q6fMtDiRVWqLUqEIcSbGMjmwbL8E`

If the key were only taken from this repository, a signature would not protect against a hijacked GitHub account.
Save the line above locally the first time and reuse it for later releases.

### Verify the signature (Windows, Linux and macOS)

Needs only OpenSSH 8.1 or newer, which is built into Windows 10/11, macOS 10.15+ and current Linux distributions.
Check the version with `ssh -V`.

1. Download `Hypervault_next_gen.html` and `Hypervault_next_gen.html.sig` from the
   [release](https://github.com/Harding-Stardust/hypervault_ng) into the same folder.
2. Open a terminal in that folder and create the file `hvng_allowed_signers` with the key line:

   Linux, macOS and Windows **cmd**:

   ```sh
   echo harding ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICvtErANjUZmrTKBrwmOEvRDEGB1gtQhIOK8e8cFkeKf hypervault NG release key > hvng_allowed_signers
   ```

   Windows **PowerShell** (`>` writes UTF-16 there, which ssh-keygen rejects with "invalid line"):

   ```powershell
   Set-Content -Encoding ascii hvng_allowed_signers "harding ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICvtErANjUZmrTKBrwmOEvRDEGB1gtQhIOK8e8cFkeKf hypervault NG release key"
   ```

3. Verify the signature.

   Linux, macOS and Windows **cmd**:

   ```sh
   ssh-keygen -Y verify -f hvng_allowed_signers -I harding -n file -s Hypervault_next_gen.html.sig < Hypervault_next_gen.html
   ```

   Windows **PowerShell** (`<` does not work there, so run the command through cmd):

   ```powershell
   cmd /c "ssh-keygen -Y verify -f hvng_allowed_signers -I harding -n file -s Hypervault_next_gen.html.sig < Hypervault_next_gen.html"
   ```

A genuine file prints `Good "file" signature for harding with ED25519 key SHA256:...` and the command exits with
code 0. A modified file prints `Signature verification failed` and exits with code 255.

### For the maintainer: signing a release

```sh

ssh-keygen -t ed25519 -C "hypervault NG release key" -f hvng_signing_key    # Once. Use a strong passphrase
ssh-keygen -Y sign -f hvng_signing_key -n file Hypervault_next_gen.html     # Creates Hypervault_next_gen.html.sig
ssh-keygen -lf hvng_signing_key.pub                                         # Shows the fingerprint to publish
```

Upload `Hypervault_next_gen.html` and `Hypervault_next_gen.html.sig`. The repository's `.gitattributes`
keeps the HTML file byte for byte (no line ending conversion), so the signature also matches a `git clone` on Windows.
