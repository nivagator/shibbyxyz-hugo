---
title: "Immich"
date: 2023-07-26T08:21:43-05:00
tags: ['linux', 'photos']
---

Using self hosted Immich as a google photos alternative for photo backup and consolidation.  

recursively find file types
```bash 
find . -type f -printf '%f\n' | sed 's/^.*\.//' | sort -u
```

remove spaces in file and directory names
```bash
find . -name "* *" \( -type f -o -type d \) -execdir rename -v --all " " "_" "{}" +
```
remove commas in file and directory names
```bash
find . -name "*,*" \( -type f -o -type d \) -execdir rename -v --all "," "" "{}" +
```

Parse directory recursively for image files and move them to the root directory  
```bash 
for f in $(find . -type f -iname '*JPG' -o -iname '*.PNG' -o -iname '*.jpeg' -o -iname '*.bmp' -o -iname '*.tif'); do mv "$f" ./"$(basename $(dirname "$f"))-$(basename "$f")"; done;
```

Parse directory recursively for video files and move them to the root directory  
```bash 
for f in $(find . -type f -iname '*.AVI' -o -iname '*.MOV' -o -iname '*.m4v' -o -iname '*.mp4'); do mv "$f" ./"$(basename $(dirname "$f"))-$(basename "$f")"; done;
```
AVI
MOV
m4v
mp4




combined:
```bash
find . -name "* *" \( -type f -o -type d \) -execdir rename -v --all " " "_" "{}" + && \
find . -name "*,*" \( -type f -o -type d \) -execdir rename -v --all "," "" "{}" + && \
for f in $(find . -type f -iname '*JPG' -o -iname '*.PNG' -o -iname '*.jpeg' -o -iname '*.bmp'); do mv "$f" ./"$(basename $(dirname "$f"))-$(basename "$f")"; done;
```


[Immich cli](https://documentation.immich.app/docs/features/bulk-upload)
 
```bash
alias immichup='immich upload --key 4bpl6KHn --server http://192.168.1.48:8322/api'
```

[Exiftool](https://linuxconfig.org/how-to-get-and-change-image-metadata-in-linux) for adding photo dates to metadata. this is used to sort photos on the timeline  
```bash
exiftool -DateTimeOriginal="2014:12:16 15:15:00" Owen/owen-bruce-21.jpg
```