# dotf-yubikey-ssh

This repository is part of my dot files.

Important note on openssh FIDO/U2F support: [The private key file should be
useless to an attacker who does not have access to the physical token.](https://www.openssh.com/txt/release-8.2)
-> justification for publishing private keys here

## Key generation

Installation of `libfido2` is a must, and availability of package `yubikey-manager`
is recommended.

Keys are to be generated and stored with specifically two properties:

- For authentication only *touch* is required.
- Keys are created per user.

So the command for key generation should look like

``` sh
ssh-keygen -t ed25519-sk -O resident -O user=username -C "user's full name (Yubikey serial number)"
```

and we *don't* set a passphrase for the private key.

### Listing FIDO2 credentials

Enter command

``` sh
ykman --device <Yubikey serial number> fido credentials list --csv
```

### MacOS notes

Installation of `openssh` (e.g. via Nix) is mandatory.

## Git

### Signing and verifying commits signed with ssh locally

To achieve this, presence of file `~/.ssh/allowed_signers` containing a line like
`shuntingyard@gmail.com namespaces="git" <public sk key>`
is required.

And some entries in git's configuration file

``` git
[user]
            signingkey = <absolute path to public sk key>
[commit]
            gpgsign = true
[gpg]
            format = ssh
[gpg "ssh"]
            allowedSignersFile = <absolute path to allowed_signers file>
```

## Resources

Yubico official <https://developers.yubico.com/SSH/Securing_SSH_with_FIDO2.html>
SSH + Yubikeys via FIDO2 on MacOS <https://gist.github.com/jaybuidl/d42145c32adc48ba6ecc89f73c6bb0c9>
