---
layout: post
title: Bandit Labs
subtitle: Getting practice with common Linux commands
cover-img: /assets/img/honk.png
thumbnail-img: /assets/img/domo.png
share-img: /assets/img/domo.png
tags: []
---

Many times have I started Over The Wire's Bandit labs, but never have I gone through the whole thing. I am documenting my journey through the process. This is a nice way to be re-introduced to Linux commands that I do not frequently use

Level 0
- Given by the website

Level 0 → Level 1
- Simple ```cat``` to a file

Level 1 → Level 2
- Things start out a little sneaky. the file is named ```-```, which if you try to ```cat``` as normal, ```cat``` interprets it as a commandline argument. instead, we need to redirect the input to the cat command, like ```cat < -```

Level 2 → Level 3
- Here we just need to escape the white space between the words in the file name, such as ```cat hello\ world.txt```

Level 3 → Level 4
- This makes use of ```.``` to hide files. a simple ```ls -lah``` will display it

Level 4 → Level 5
- My solution here was to script an input redirection to find which one might have the pass, like ```for i in {0..9}; do cat < -file0$i; done```

Level 5 → Level 6
- This is where things started ramping up a bit. I started to employ chat gpt, but limited my questions so as to not just get a full working script. I lacked a bit of knowledge here so i just wanted to see what pieces I could mash together. this is what i came up with: ```find ./ -type f -exec head -n 1 {} \; | awk 'length == 32'```

Level 6 → Level 7
- From here I stopped yolo'ing and read the instructions for each challenge. This one said that the following criteria must be met: owned by user bandit7, owned by group bandit6, and 33 bytes in size.
  The home for bandit 6 was empty. When I went up in the file system I saw a bunch of different folders with different owners and permissions. I first tried to repurpose some earlier code, which worked, but did not find the pass
  ```find ./ -size -35 2>/dev/null -exec head -n 1 {} \;```
  I then ran ```find ./ -group bandit6``` and realized that some of the critical criteria were missing. No files were owned by the group bandit 6 in the home directory. The prompt also states that it is somewhere on the server, not necessarily the home direcotry. I was still not finding it with this, however: ```find / -user bandit7 -group bandit6 -size 33 2>/dev/null```
  From here I upped the size limit just in case and actually found something at /var/lib/dpkg/info/bandit7.password. What is strange is that the file size is definitely 33 bytes. Not sure why the size filter was not working.
  ```find / -user bandit7 -group bandit6 -size -50 2>/dev/null```

Level 7 → Level 8
- This one was very straightforward, just based off of how grep works by default; it will print the whole line. You can also add the line number here, in case the instructions meant above or below, not just besides. ```cat data.txt | grep -n millionth```. 

Level 8 → Level 9
- This one is also pretty straightforward, as it reminds you in the instructions which commands you could use. We sort things first, then count the number of times each string is present. ```sort data.txt | uniq -c```
  
Level 9 → Level 10
- The key words that tipped me off here were 'human readable', which instantly tells me to use ```strings```. It also says that the pass is next to several equals signs, so a simple pipe to ```grep``` will take care of this. ```strings data.txt | grep ===```

Level 10 → Level 11
- Here I started off running ```head``` against the data file, to learn that it is only one line. As stated in the instructions, it is encoded with base64. Piping it to ```base64``` will be the easiest way to go here. ```cat data.txt | base64 -d```
  
Level 11 → Level 12
- Just based off the instructions, we immediately know it is a caesar cipher, or similar substitution cipher, one of the oldest encryption techniques. Every time I have dealt with caesar ciphers I have always ran it through on online tool. I have never done it through the command line, so this will be interesting. Some googleing leads me to the ```tr``` command, which is installed on the host. I have never heard of it, but I also see that it is one of the recommended commands from the instructions.

- I came across this suggestion in a github gist ```echo "GUR DHVPX OEBJA SBK WHZCF BIRE GUR YNML QBT" | tr '[N-ZA-M]' '[A-Z]'```, but it did not work for me out of the box, so I had to dig into why. Not surprisingly, the ```tr``` options are uppercase for a reason. Here I am working with at least some lowercase, so I had to make at least some of the options lower case as well. I was not sure how to do it in one single command, so I just piped the output of lowercase into the uppercase command. ``` cat data.txt | tr [n-za-m] [a-z] | tr [N-ZA-M] [A-Z]```

- https://gist.github.com/IQAndreas/030b8e91a8d9a407caa6

Ill pick up here at a later date.

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
| PoC ||
| PoC ||

```
