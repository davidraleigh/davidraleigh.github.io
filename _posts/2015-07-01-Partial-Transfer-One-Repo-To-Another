![](/content/images/2015/07/dr-octocat.png)

In trying to get a node.js appliction up and running on Azure I came up against a problem. My git repository had two separate node.js projects in it so I couldn't use git publishing. In order to allow for git publishing I needed to separate out my projects, but I didn't want to lose my git history for each project.

After some searching I found Greg Bayer's "[Moving Files from One Git Repository to Another, Preserving History](http://gbayer.com/development/moving-files-from-one-git-repository-to-another-preserving-history/)." It seemed to work up until the point where I was moving files. It seems maybe he had a typo when it came to the ```mv * <directory 1>``` line (maybe is should be ```git mv * <directory 1>```?). Or maybe I was just doing it wrong.

Looking into the [stackoverflow question](http://stackoverflow.com/questions/1365541/how-to-move-files-from-one-git-repo-to-another-not-a-clone-preserving-history) he referenced I found the solution I needed.

The bard-chat[bard-chat](https://github.com/davidraleigh/bard-chat) repo had multiple projects in different directories. From the bard-server sub-directory I needed to pull the node.js project files and make those the basis for the [bard-chatter](https://github.com/davidraleigh/bard-chatter) repo.

```bash
$: git clone https://github.com/davidraleigh/bard-chat.git
$: cd bard-chat/
$: git filter-branch --subdirectory-filter bard-server/ -- --all
$: git remote rm origin
$: cd ..
$: git clone https://github.com/davidraleigh/bard-chatter.git
$: git remote add orig ../bard-chat
$: git fetch orig
$: git branch orig remotes/orig/master
$: git push origin master
```
