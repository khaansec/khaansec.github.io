---
layout: post
title: Adventures With Open Source Projects
subtitle: Giving back to the community and starting to learn git
cover-img: /assets/img/lol-rmm-logo.png
thumbnail-img: /assets/img/lol-rmm-logo.png
share-img: /assets/img/lol-rmm-logo.png
tags: []
---

During a recent threat hunt, I came across an open source project called LOLRMM, which focuses on cataloguing the Living Off the Land (LOL) techniques of Remote Monitoring and Management (RMM) software. 

As I used the site, I noticed some small things that would have made my searches easier or more reliable, such as an incomplete list of supported opperating systems. For example, If an executable is a .exe file, the supported OS is going to be Windows. I point this out not to poke fun, but highlight something small that pretty much anyone could contribute to. And on that point, there are so many projects out there that could use a little help, and I encourage you to find one that interests you and contribute to it.

Along this journey, I also inadvertently included my .vs/ folder in both a commit and a pull request. I kind of assumed that it was already included in my .gitgnore file, but for some reason it was not. For those just starting, .gitignore is a file that tells git which files or directories to ignore in a project. It is a good practice to include things like build artifacts, temporary files, and IDE-specific files and folders (like the .vs/ folder) in this file to keep your repository clean and focused on the source code.

In case this ever happens to you, I have included below what looks to be the best steps to take to remove the .vs/ folder from commits and a repository. Grabbed it from Google's AI preview, so I can't take credit for it, though I did review it myself and asked some friends who are better at git than me before running it.


1. Update your .gitignore file:

    If you don't have a .gitignore file: Create one in the root of your repository.
    If you have a .gitignore file: Add .vs/ to it. You might also want to add your project name followed by /.vs if necessary, like {yourgitprojectname}/.vs.
    This ensures that Git ignores the .vs folder in future commits. 

2. Stop tracking the .vs folder in your repository:

    Open your terminal or Git Bash within your repository's root directory.
    Run the command: git rm -r --cached .vs/.
        git rm: Removes the folder from the index (staging area).
        -r: Removes the folder recursively (including its contents).
        --cached: Removes it from the index without deleting it from your local filesystem.
    This removes the folder from the Git index but keeps it on your local machine. 

3. Commit the changes:

    Run the command: git commit -m "Removed .vs folder from repository".
    This commits the removal of the folder from the repository's index. 

4. Push the changes:

    Run the command: git push origin <your-branch-name>.
    Replace <your-branch-name> with the name of your branch. 


