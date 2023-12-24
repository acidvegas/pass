# pass
> posix password manager

A very simple password manager that keeps passwords inside gpg encrypted files inside a simple directory tree.

Similar to [password-store](https://git.zx2c4.com/password-store/about/), but written in POSIX compliant shell script instead of bash.

## Requirements
- [gpg](https://gnupg.org/)
- [tree](https://oldmanprogrammer.net/source.php?dir=projects/tree)

###### Optional Requirements
- [nano](https://www.nano-editor.org/)                      *(required only if environment variable `$EDITOR` is not set)*
- [pinentry-dmenu](https://github.com/ritze/pinentry-dmenu) *(required for menu)*
- [xclip](https://github.com/astrand/xclip)                 *(required for menu to copy passwords)*
- [xdotool](https://github.com/jordansissel/xdotool)        *(required for menu to type passwords)*
- [oath-toolit](https://www.nongnu.org/oath-toolkit/)       *(required for 2FA)*
- [gnupg2-scdaemon](https://linux.die.net/man/1/scdaemon)   *(required for smartcard support)*

## Config
Edit the source code to change these settings:

| Setting    | Description                                                                                                      |
| ---------- | ---------------------------------------------------------------------------------------------------------------- |
| `GPG_ID`   | Default GPG key ID to use for encrypting/decrypting                                                              |
| `GPG_OPTS` | Do not edit this unless you know what you are doing                                                              |
| `METHOD`   | Method used for the menu *("copy" will use xclip to copy passwords & "type" will use xdotool to type passwords)* |
| `PASS_DIR` | Directory to store all password information                                                                      |

## Usage
| Command            | Description                                                                                                  |
| ------------------ | ------------------------------------------------------------------------------------------------------------ |
| `pass`             | Display a directory tree of stored passwords                                                                 |
| `pass <path>`      | Display password information for `<path>` or a directory tree of stored passwords if `<path>` is a directory |
| `pass menu`        | Use pass in dmenu *(Selected line is copied to the clipboard or typed out depending on the `METHOD` used)*   |
| `pass edit <path>` | Display stored password information for `<path>`                                                             |
| `pass gen <len>`   | Generate a random password that is `<len>` characters long                                                   |
| `pass otp <path>`  | Return a 2-Factor-Authenticaion code for `<path>` *(Last line of `<path>` must be a valid otpauth:// URI)*   |

###### Note
`<path>` is not a direct path per-say. If the password is stored in `$PASS_DIR/www/github.gpg` all you have to put is `www/github` for `<path>`

When using the menu, the clipboard is cleared after 3 seconds or passwords are typed after 3 seconds, depending on what `METHOD` you set in the config.

For setting up 2FA, you can download the QR code image & use [zbar](https://github.com/mchehab/zbar) to convert it to a string to get a valid URI.

## Pinentry Setup
To keep everything in the command line, make sure you edit your `$HOME/.gnupg/gpg-agent.conf` to include `pinentry-program /usr/bin/pinentry-curses`

If you plan on using the menu features, [pinentry-dmenu](https://github.com/ritze/pinentry-dmenu) will allow you to enter your GPG key password inside of dmenu, but in order to do that you will need to create a wrapper for pinetry at `$HOME/.gnupg/pinentry-wrapper`:
```
if [ "$PINENTRY_USER_DATA" = "dmenu" ]; then
    exec /usr/local/bin/pinentry-dmenu "$@"
else
    exec /usr/bin/pinentry-curses "$@"
fi
```
Make it executable with `chmod +x $HOME/.gnupg/pinentry-wrapper` and then edit your `$HOME/.gnupg/gpg-agent.conf` to include `pinentry-program $HOME/.gnupg/pinentry-wrapper`.

## SmartCard Support
Using a [Smart Card](https://en.wikipedia.org/wiki/Smart_card) such as a [YubiKey](https://www.yubico.com/) with pass simply requires setting up your GPG key to recognize your card.

First, you will need to install `scdaemon` & enable the service on your system in order to recognize your smartcards. After you set this up, you can check if your card is recognized with the `gpg --card-status` command.

Edit your GPG key with `gpg --edit-key [Your-Key-ID]` & run the follow commands in the interactive session:
```
key 1
keytocard
save
```


## Ideas & TODO
- Hash file names for obsurity *(`pass rm <entry>` & `pass mv <entry>` since file names will be hashed)*
- Better way than using a hard coded `GPG_ID` & maybe on the fly `METHOD` selection

___

###### Mirrors
[acid.vegas](https://git.acid.vegas/pass) • [GitHub](https://github.com/acidvegas/pass) • [GitLab](https://gitlab.com/acidvegas/pass) • [SuperNETs](https://git.supernets.org/acidvegas/pass)
