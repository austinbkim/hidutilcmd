# hidutilcmd

## Table of Contents

- [Name](#name)
- [Synopsis](#synopsis)
- [Description](#description)
- [Installation](#installation)
- [See Also](#see-also)
- [Standards](#standards)
- [Bugs](#bugs)

## Name

`hidutilcmd` -- Re-map _caps lock_, _tab_, _``_/_~_, and right-side _option_ (aka _alt_) keys on 2016--2019 Apple MacBook Pros without physical _esc_ape keys.

## Synopsis

```shell
sudo /path/to/hidutilcmd
```

`hidutilcmd` must be installed with execute permissions enabled, and the user must be authorized to use `sudo` (typically as a member of group `wheel` on Apple macOS systems).

## Description

`hidutilcmd` remaps the physical _caps lock_ key to a logical _tab_ key, the physical _tab_ key to a logical _``_/_~_ key, the physical _``_/_~_ key to a logical _esc_ key, and the right-side _option_ key (aka _alt_ key) to a logical _caps lock_ key, using Apple macOS’s [`hidutil`](https://developer.apple.com/library/archive/technotes/tn2450/_index.html) utility, intended for use on 2016--2019 Apple MacBook Pros without physical _esc_ keys.  The rationale for using the physical _caps lock_ key as a logical _tab_ key and for using the right _option_ (aka _alt_) key as a logical _caps lock_ key is that _caps lock_ is typically seldom used, whereas the right _option_ key is almost never used.

## Installation

`hidutilcmd` must be installed with execute permissions, and should be installed with write permissions disabled for “group” and “other,” _e. g._:  `chmod a+rx,go-w hidutilcmd`.  On Apple macOS systems, the primary user (aka administrator) will typically be a mamber of group `wheel`, allowing invocation of `hidutilcmd` via `sudo`.

To allow invoking `sudo hidutilcmd` without being prompted for the user password, optionally run `sudo visudo` and append the following line to `sudoers`:

    username ALL = NOPASSWD: /path/to/hidutilcmd

If Apple _Terminal_ be typically launched at startup (_e. g._, as one of the user’s “Login Items” in macOS), optionally add `sudo /path/to/hidutilcmd` to your `.kshrc` or other login shell startup script, _e. g._:
```
hidutil property --get "UserKeyMapping" |tee .DS_Store
if grep \(null\) .DS_Store >/dev/null
   then sudo $HOME/Documents/config/hidutilcmd/hidutilcmd
fi
rm .DS_Store
```
The above shell script snippet first queries whether the above key remapping already be in effect, and goes on to invoke `hidutilcmd` if not.

## See Also

_[Technical Note TN2450:  Remapping Keys in macOS 10.12 Sierra](https://developer.apple.com/library/archive/technotes/tn2450/_index.html)_ (from Apple macOS Documentation Archive)

## Standards

_USB HID Usage Tables Specification_, Section 10:  Keyboard/Keypad page.

## Bugs

Having to use `sudo` to run `hidutil` to remap keys presents a security risk, as all users on a system who wish to use `hidutil` must be granted `sudo` access.  Installing `hidutilcmd` with `root` as the owner and the `setuid` bit set (and with the directory wherein `hidutilcmd` is located as well as all directories above it all the way up through `/` owned by `root` as required by macOS for this to work) would potentially be a better solution, but does not appear to work in all versions of macOS and is [deprecated by Apple](https://developer.apple.com/library/archive/documentation/Security/Conceptual/SecureCodingGuide/Articles/AccessControl.html).

On the mid-2017 Apple MacBook Pro 15-inch used for testing, pressing the physical _caps lock_ key twice in succession causes _caps lock_ to become engaged (and the corresponding LED to turn on), even despite the above remapping.  The remapped physical right _option_ (aka _alt_) key can be used to turn _caps lock_ back off when this happens.

Last modified:  Thursday, March 27, 2025 (tested under Apple macOS ‘Ventura’ version 13.7.4)
