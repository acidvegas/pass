# pass
> posix password manager

A very simple password manager that keeps passwords inside gpg encrypted files inside a simple directory tree.

Similar to [password-store](https://git.zx2c4.com/password-store/about/), but simplified & written in POSIX compliant shell script.

## Requirements
- [gpg](https://gnupg.org/)
- [tree](https://oldmanprogrammer.net/source.php?dir=projects/tree)
- [oath-toolit](https://www.nongnu.org/oath-toolkit/)     *(optional: required for 2FA)*
- [gnupg2-scdaemon](https://linux.die.net/man/1/scdaemon) *(optional: required for smartcard support)*

## Config
Edit the source code to change these settings:

| Setting    | Description                                                                                                      |
| ---------- | --------------------------------------------------- |
| `GPG_ID`   | Default GPG key ID to use for encrypting/decrypting |
| `GPG_OPTS` | Do not edit this unless you know what you are doing |
| `PASS_DIR` | Directory to store all password information         |

## Usage
| Command            | Description                                                                                                  |
| ------------------ | ------------------------------------------------------------------------------------------------------------ |
| `pass`             | Display a directory tree of stored passwords                                                                 |
| `pass <path>`      | Display password information for `<path>` or a directory tree of stored passwords if `<path>` is a directory |
| `pass edit <path>` | Display stored password information for `<path>`                                                             |
| `pass gen <len>`   | Generate a random password that is `<len>` characters long                                                   |
| `pass otp <path>`  | Return a 2-Factor-Authenticaion code for `<path>` *(Last line of `<path>` must be a valid otpauth:// URI)*   |

**Note:** `<path>` is not a direct path per-say. If the password is stored in `$PASS_DIR/www/github.gpg` all you have to put is `www/github` for `<path>`

For setting up 2FA, you can download the QR code image & use [zbar](https://github.com/mchehab/zbar) to convert it to a string to get a valid URI.

## Pinentry Setup
To keep everything in the command line, make sure you edit your `$HOME/.gnupg/gpg-agent.conf` to include `pinentry-program /usr/bin/pinentry-curses`

## SmartCard Support
Using a [Smart Card](https://en.wikipedia.org/wiki/Smart_card) such as a [YubiKey](https://www.yubico.com/) with pass simply requires setting up your GPG key to recognize your card.

First, you will need to install `scdaemon` & enable the service on your system in order to recognize your smartcards. After you set this up, you can check if your card is recognized with the `gpg --card-status` command.

Edit your GPG key with `gpg --edit-key [Your-Key-ID]` & run the follow commands in the interactive session:
```
key 1
keytocard
save
```

___

###### Mirrors [SuperNETs](https://git.supernets.org/acidvegas/pass) • [GitHub](https://github.com/acidvegas/pass) • [GitLab](https://gitlab.com/acidvegas/pass) • [Codeberg](https://codeberg.org/acidvegas/pass)
