---
layout: post
title: Bandit Labs
subtitle: Exploring T1547.009 
cover-img: /assets/img/honk.png
thumbnail-img: /assets/img/domo.png
share-img: /assets/img/shortcut.png
tags: [poc, shortcut]
---

Many time I have started Over The Wire's Bandit labs, but never have I gone through the whole thing. I am documenting my journey through the process.

Level 0
- Given by the website

Level 0 → Level 1
- simple cat to a file

Level 1 → Level 2
- things start out a little sneaky. the file is named '-', which if you try to cat as normal, cat interprets it as a commandline argument. instead, we need to redirect the input to the cat command, like 'cat < -'

Level 2 → Level 3
- here we just need to escape the white space between the words in the file name

Level 3 → Level 4
- this make use of . to hide files. a simple ```ls -lah``` will display it

Level 4 → Level 5
- my solution here was to script an input redirection to find which one might have the pass, like ```for i in {0..9}; do cat < -file0$i; done```

Level 5 → Level 6
- this is where things started ramping up a bit. i started to employ chat gpt, but limited my questions so as to not just get a full working script. i lacked a bit of knowledge here so i just wanted to see what pieces i could mash together. this is what i came up with: ```find ./ -type f -exec head -n 1 {} \; | awk 'length == 32'```

Level 6 → Level 7
- from here i stopped yoloing and read the instructions for each challenge

Level 7 → Level 8

Level 8 → Level 9

Level 9 → Level 10

Level 10 → Level 11

Level 11 → Level 12

Level 12 → Level 13

Level 13 → Level 14

Level 14 → Level 15

Level 15 → Level 16

Level 16 → Level 17

Level 17 → Level 18

Level 18 → Level 19

Level 19 → Level 20

Level 20 → Level 21

Level 21 → Level 22

Level 22 → Level 23

Level 23 → Level 24

Level 24 → Level 25

Level 25 → Level 26

Level 26 → Level 27

Level 27 → Level 28

Level 28 → Level 29

Level 29 → Level 30

Level 30 → Level 31

Level 31 → Level 32

Level 32 → Level 33

Level 33 → Level 34

**Resources**

| Type | Link |
| :------ | :--- |
| PoC | [https://v3ded.github.io/redteam/abusing-lnk-features-for-initial-access-and-persistence](https://v3ded.github.io/redteam/abusing-lnk-features-for-initial-access-and-persistence) |
| PoC | [https://www.ired.team/offensive-security/initial-access/phishing-with-ms-office/phishing-ole-+-lnk](https://www.ired.team/offensive-security/initial-access/phishing-with-ms-office/phishing-ole-+-lnk) |

```
