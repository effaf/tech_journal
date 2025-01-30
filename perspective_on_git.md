## How does he Git it?

###### Constructing a vantage for Git by examining the components of the software

_This is an intermediate article covering few, but essential components of git._

The very first time I heard the term git it was followed by hub. I visit the GitHub website, create an account, and after a minute I close the tab. 

After nearly a year, I visit the website again. However, this time I am following a tutorial. I have initialized a local repository, staged the files, established connection to my GitHub account and am ready to push my files. I go through my files again and realize a Jupyter notebook is missing. It was a data preprocessing file I had spent a week coding. I panic. I search the sub directories, parent directory, recycle bin, and even my other drive, but can’t find the notebook. Exhausting my options, I download the old version I previously had sent to my professor. I make all the changes again and push it to GitHub. My blunder? I had created two branches from master, and did not know git branches effect the file you see in the explorer. 

The third time, an older me, is reading a book on git. I learn to differentiate between local git repository and remote git repository. I learn about git branches, and how commits are implemented in git.

In this article, I answer the questions which I had while learning git the third time. I hope this helps solidify the functioning of git.

**Question 1 - If git is tracking versions of large code files, will it not consume surmountable memory to store it? Remember how we used to create backup of our work, and at certain time it would fill up our Gmail space? Will git have the same disadvantage?**

Storing text does not require large amounts of space. Furthermore, text compression techniques are highly efficient and sophisticated. Since code repositories are mainly text, it is not expensive for git to track it. 

The other smart move git makes is that for every commit it only stores the differences. Git maintains a base file. As you commit changes, it stores the differences and discards the similarities as compared to the base file. However, this is a deferred operation. It initially stores complete snapshot of each file, and has a hook which computes and compresses the diff.

**Question 2 - What files are there inside the git directory? What are they storing, and what is their role?**

There are three main directories inside the git directory.