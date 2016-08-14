---
layout: post
description: Solving git Merge Conflicts with a GUI Tool like diffmerge or sublime 
title: Solving git Merge Conflicts with a GUI Tool
---

![This image is huuuuuge!](https://davidraleigh.github.io/assets/git-merge/foundingfather_v2.png)


**Merging in git is almost as scary as merging in Dallas:**![Photo Credit: Dallas Observer](http://davidraleigh.io/content/images/2014/12/Trafficpocalypse-thumb-565x317.jpg)


Wrecking a merge can feel as bad as breaking the build. The best way to avoid the process of a manual merge is to be proactive about commits and pull requests. But there will be times when you and another developer are in lightspeed-productivity-mode on the same file, and when it comes to spinning down the hyperdrives, one of you may be met with the heinous task of joining your histories together.

Integrating a GUI merge tool into your git workflow is a breeze. If you're a terminal pro you might flinch at the idea of working outside of your text editor, but I assure you it's more fun to get your merges done quickly and save your brain's computing power for more engaging programming problems. If you're using Github's fancy desktop client, you'll have to touch your terminal a little, but I swear it won't hurt. This tutorial is all Mac, but it applies for Windows as well.

In order to visualize the merge you'll need your GUI. If you've got money to burn($25) you can buy yourself the fancy [Sublimerge](http://www.sublimerge.com/). If you're frugal like me, you'll need to download the [DiffMerge](https://sourcegear.com/diffmerge/) application from SourceGear (DiffMerge is Mac only, but similar softwares exist for Windows). For DiffMerge select the *OS X 10.6+ Installer (Intel)*  package installer (not the DMG). The reason to not install from the DMG is that the installation won't do all the fancy symlinks that the git client will require in order to call DiffMerge from the terminal. 

**DiffMerge GUI experience**![DiffMerge example](http://davidraleigh.io/content/images/2014/12/DiffMerge.png)

**Sublimerge GUI experience**![Sublimerge example](http://davidraleigh.io/content/images/2014/12/sublimerge.png)

After you've downloaded your GUI of choice with all it's fancy symlinks you're ready to prep your git to interact with GUI goodness. You'll need to add a few lines to your *.gitconfig*. If you're not a terminal lover I suggest editing your *.gitconfig* with *nano* by using the following command at your terminal: `nano ~/.gitconfig`.

If you're following the DiffMerge path you'll need to add the following lines to your *.gitconfig*:
```bash
[diff]
  tool = diffmerge
[merge]
  tool = diffmerge
  
[mergetool "diffmerge"]
  cmd = /usr/bin/diffmerge --merge --result=\"$MERGED\" \"$LOCAL\" \"$BASE\" \"$REMOTE\"
  trustExitCode = true
  
[difftool "diffmerge"]
  cmd = /usr/bin/diffmerge \"$LOCAL\" \"$REMOTE\"
```

If you're choosing the slick Sublimerge gui you'll need to add the following lines to your *.gitconfig*:
```bash
[diff]
  tool = sublimerge
[merge]
  tool = sublimerge

[mergetool "sublimerge"]
  cmd = subl -n --wait \"$REMOTE\" \"$BASE\" \"$LOCAL\" \"$MERGED\" --command \"sublimerge_diff_views\"
trustExitCode = true

[difftool "sublimerge"]
  cmd = subl -n --wait \"$REMOTE\" \"$LOCAL\" --command \"sublimerge_diff_views {\\\"left_read_only\\\": true, \\\"right_read_only\\\": true}\"
```

Using the above *.gitconfig* files means you'll be preserving originals as backups with a *.orig* extension type. Add `*.orig` to your project *.gitignore* to avoid the annoyance of backup files sneaking into your repository.

So now you're all set to merge conflicts. After you've rebased and you've received the dreaded `Failed to merge in the changes` you type in `git mergetool` at the terminal and you'll receive a prompt like below (to call `git mergetool` you must be in the directory of your git repository):

```bash
Merging:
client/js/jadegrizzly.js
client/templates/create.html

Normal merge conflict for 'client/js/jadegrizzly.js':
  {local}: modified file
  {remote}: modified file
Hit return to start merge resolution tool (diffmerge): 
```

Hitting enter will pull up your awesome new merge tool and allow you to modify your files side by side (**BIG WARNING!!!** LOCAL and REMOTE in the GUI may be confusing identifiers. For the rebase scenario in DiffMerge the LOCAL side should represent the data that you've rebased to your local repository. That means the LOCAL side is the content you retrieved from master when you `git pull --rebase upstream master`).

Once you've saved and exited your gui merge tool, you need to take the next steps in git to complete your merge. But the hard part should be complete!

**This post was completed as a part of an extreme blogging session, so it may contain errors or technical innaccuracies. Git with caution!!**

**Don't code like this guy bus drives.**![Photo Credit: Scoop Empire](http://davidraleigh.io/content/images/2014/12/bottleneck1.jpg)
