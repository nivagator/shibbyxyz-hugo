---
title: "GPG Keys Expired"
date: 2022-08-15T10:16:02-05:00
tags: ['linux']
draft: true
---

[How to understand the `gpg failed to sign the data` problem in git · GitHub](https://gist.github.com/paolocarrasco/18ca8fe6e63490ae1be23e84a7039374)

If everything fails, use GIT_TRACE=1 to try and see what git is actually doing:
```bash
[gavin@eostower shibbyxyz-hugo]$ GIT_TRACE=1 git commit -m "move recipes to separate dir"
10:13:15.808842 git.c:460               trace: built-in: git commit -m 'move recipes to separate dir'
10:13:15.858691 run-command.c:654       trace: run_command: gpg --status-fd=2 -bsau 5A9B58B529C10A73
error: gpg failed to sign the data
fatal: failed to write commit object
```

run the failing gpg command separately  
```bash
[gavin@eostower shibbyxyz-hugo]$ gpg --status-fd=2 -bsau 5A9B58B529C10A73
[GNUPG:] KEYEXPIRED 1660420337
[GNUPG:] KEY_CONSIDERED 3572C3E96D9C0F641CAA8F9701539EC57FFADE9B 3
gpg: skipped "5A9B58B529C10A73": Unusable secret key
[GNUPG:] INV_SGNR 9 5A9B58B529C10A73
[GNUPG:] FAILURE sign 54
gpg: signing failed: Unusable secret key

```

my keys have expired. 

attempting to renew fails with   
gpg: signing failed: No secret key
gpg: make_keysig_packet failed: No secret key

gpg --edit-key the


other key?
gpg --edit-key 3572C3E96D9C0F641CAA8F9701539EC57FFADE9B



[How to renew an expired keypair with gpg - Stack Exchange](https://unix.stackexchange.com/questions/177291/how-to-renew-an-expired-keypair-with-gpg)   
[Renew Expired GPG key · GitHub](https://gist.github.com/krisleech/760213ed287ea9da85521c7c9aac1df0)  

[How to renew an expired encryption subkey with gpg - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/552707/how-to-renew-an-expired-encryption-subkey-with-gpg)

From my notes and from the YubiKey Guide:  

## Renewing sub-keys

Renewing sub-keys is simpler: you do not need to generate new keys, move keys to the YubiKey, or update any SSH public keys linked to the GPG key.  All you need to do is to change the expiry time associated with the public key (which requires access to the master key you just loaded) and then to export that public key and import it on any computer where you wish to use the **GPG** (as distinct from the SSH) key.

To change the expiration date of all sub-keys, start by selecting all keys:

```console
$ gpg --edit-key $KEYID

Secret key is available.

sec  rsa4096/0xFF3E7D88647EBCDB
    created: 2017-10-09  expires: never       usage: C
    trust: ultimate      validity: ultimate
ssb  rsa4096/0xBECFA3C1AE191D15
    created: 2017-10-09  expires: 2018-10-09  usage: S
ssb  rsa4096/0x5912A795E90DD2CF
    created: 2017-10-09  expires: 2018-10-09  usage: E
ssb  rsa4096/0x3F29127E79649A3D
    created: 2017-10-09  expires: 2018-10-09  usage: A
[ultimate] (1). Dr Duh <doc@duh.to>

gpg> key 1

Secret key is available.

sec  rsa4096/0xFF3E7D88647EBCDB
     created: 2017-10-09  expires: never       usage: C
     trust: ultimate      validity: ultimate
ssb* rsa4096/0xBECFA3C1AE191D15
     created: 2017-10-09  expires: 2018-10-09  usage: S
ssb  rsa4096/0x5912A795E90DD2CF
     created: 2017-10-09  expires: 2018-10-09  usage: E
ssb  rsa4096/0x3F29127E79649A3D
     created: 2017-10-09  expires: 2018-10-09  usage: A
[ultimate] (1). Dr Duh <doc@duh.to>

gpg> key 2

Secret key is available.

sec  rsa4096/0xFF3E7D88647EBCDB
     created: 2017-10-09  expires: never       usage: C
     trust: ultimate      validity: ultimate
ssb* rsa4096/0xBECFA3C1AE191D15
     created: 2017-10-09  expires: 2018-10-09  usage: S
ssb* rsa4096/0x5912A795E90DD2CF
     created: 2017-10-09  expires: 2018-10-09  usage: E
ssb  rsa4096/0x3F29127E79649A3D
     created: 2017-10-09  expires: 2018-10-09  usage: A
[ultimate] (1). Dr Duh <doc@duh.to>

gpg> key 3

Secret key is available.

sec   rsa4096/0xFF3E7D88647EBCDB
      created: 2017-10-09  expires: never       usage: C
      trust: ultimate      validity: ultimate
ssb*  rsa4096/0xBECFA3C1AE191D15
      created: 2017-10-09  expires: 2018-10-09  usage: S
ssb*  rsa4096/0x5912A795E90DD2CF
      created: 2017-10-09  expires: 2018-10-09  usage: E
ssb*  rsa4096/0x3F29127E79649A3D
      created: 2017-10-09  expires: 2018-10-09  usage: A
[ultimate] (1). Dr Duh <doc@duh.to>
```

Then, use the `expire` command to set a new expiration date.  (Despite the name, this will not cause currently valid keys to become expired).

```console
gpg> expire
Changing expiration time for a subkey.
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0)
```
Follow these prompts to set a new expiration date, then `save` to save your changes.

Next, export the public key:

```console
$ gpg --armor --export $KEYID > gpg-$KEYID-$(date +%F).asc
```

Transfer that public key to the computer from which you use your GPG key, and then import it with:

```console
$ gpg --import gpg-0x*.asc
```

This will extend the validity of your GPG key and will allow you to use it for SSH authorization.  Note that you do _not_ need to update the SSH public key located on remote servers.



---

## FIXED IT

found the encrypted $GNUPGHOME directory referenced by the yubikey-guide. the mastersub.key was there. i was able to extend the existing keys using my master passphrase. 