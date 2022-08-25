---
title: "Verify Files"
date: 2022-08-24T16:27:25-05:00
tags: ['linux']
---

as a good linux user, i collect install isos for major distributions. its recommended/good practice/absolutely required to verify the downloaded iso files haven't been tampered with maliciously or otherwise. There are two available methods to check the files for corruption

# checksums
checksums use a cryptographic algorithm to calculate a hash or value based on the piece of data, typically a single file. common algorithm types are SHA-1, SHA-256, SHA-512 and MD5. this calculated hash is then compared to the value calculated by the source file to ensure the local copy is genuine and has not had its contents tampered with. the different algorithms have increasing levels of resolution at the cost of computational time/power.

these cryptographic functions are available in the [GNU Coreutils](https://github.com/coreutils/coreutils). 

Generating a checksum is as simple as providing a file argument to the desired algorithm function.

```bash
$ sha256sum rubix-cube.pdf 
57e52c5ee5e90134490ad35bf7ada43b49b1be35ac5b895dc70fb1e257e28495  rubix-cube.pdf
```

using the example of the linux install iso (though not limited to this example by any means), typically a checksum file is available as a separate downloadable file in close proximity to the iso download. the below example is taken from the [EndeavourOS download page](https://endeavouros.com/latest-release/):

![eos download page](/img/eos_dl.png)

In this case, the file is labeled as a sha512sum file and contains the following:
```bash
$ cat EndeavourOS_Artemis_neo_22_7.iso.sha512sum 
a5e041d7e7f108bdf392f1d6b85b9c14942e1123b8412ba0c6e...3bf13fa26062c1792cb9af2940b  EndeavourOS_Artemis_neo_22_7.iso
```

This is great, so what do we do with it. Lucky for us, they provide the answer there as well: 

```bash
$ sha512sum -c EndeavourOS_Artemis_neo_22_7.iso.sha512sum
EndeavourOS_Artemis_neo_22_7.iso: OK
```
This calls the `sha512sum` command with the `-c or --check` flag. The check flag expects the input file to be a tab separate list of one or many sums and their corresponding file names. it reads the sum provided, calculates the has for the file declared in the list and returns the `OK` result if they match. Easy. 

In the event that they do not match, securely delete the file; do not open or use.


in other cases, a single file can contain the sums for multiple different files. [arch](https://mirrors.mit.edu/archlinux/iso/latest/) for example provides a single `sha256sums.txt` file for each of its versions in a given release. assuming you only downloaded one of the versions, the output might appear as follows:
```bash 
$ sha256sum -c sha256sums.txt 
archlinux-2022.08.05-x86_64.iso: OK
sha256sum: archlinux-x86_64.iso: No such file or directory
archlinux-x86_64.iso: FAILED open or read
sha256sum: archlinux-bootstrap-2022.08.05-x86_64.tar.gz: No such file or directory
archlinux-bootstrap-2022.08.05-x86_64.tar.gz: FAILED open or read
sha256sum: archlinux-bootstrap-x86_64.tar.gz: No such file or directory
archlinux-bootstrap-x86_64.tar.gz: FAILED open or read
sha256sum: WARNING: 3 listed files could not be read

```

[Ubuntu](https://ubuntu.com/download/desktop) provides a different approach. they provide the sum value and pass it to the hash algorithm through a pipe command rather than provide a specific file. 

```bash
$ echo "c396e956a9f52c418397867d1ea5c0cf1a99a49dcf648b086d2fb762330cc88d *ubuntu-22.04.1-desktop-amd64.iso" | shasum -a 256 --check
ubuntu-22.04.1-desktop-amd64.iso: OK
```
The algorithm is declared as a flag under the generic shasum function. The result is the same familiar output and the reassuring `OK`.

# gpg verify

isos and some other file downloads can be signed with a gpg signature. by providing the public key we can verify the downloaded file was signed by the original author. 

Again using the EndeavourOS download page as a guide, they provide the GPG signature file. Further down, they provide the public signature information. Step one is to import that public key. There are several ways to do so. 

```bash
# use the key-id provided and the system default keyserver:
$ gpg --recv-keys 8A63A62E

# declaring a specific keyserver
$ gpg --keyserver hkps://keys/openpgp.org --recv-keys E0D16589967E1FE68EF8693E7EED78C58A63A62E
```

Once the key is received, you may set the trust level using the `gpg --edit-key` command. 

Using the downloaded .sig file, we can now run the downloaded iso through the verify command.

```bash
$ gpg --verify EndeavourOS_Artemis_neo_22_7.iso.sig 
gpg: assuming signed data in 'EndeavourOS_Artemis_neo_22_7.iso'
gpg: Signature made Wed 10 Aug 2022 03:50:08 AM CDT
gpg:                using EDDSA key 2D3C9514DCAB9FB6A95D75EED99A3385BD695FF7
gpg: Good signature from "Bryan Poerwoatmodjo <bryanpwo@endeavouros.com>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: E0D1 6589 967E 1FE6 8EF8  693E 7EED 78C5 8A63 A62E
     Subkey fingerprint: 2D3C 9514 DCAB 9FB6 A95D  75EE D99A 3385 BD69 5FF7
```
In this example, i have not trusted the imported key, but I can verify the signature is good and the fingerprint matches the values provided on the webpage. 

