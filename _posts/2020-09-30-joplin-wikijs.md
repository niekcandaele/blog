---
tags: devops
---

I use Joplin for note taking but I also use Wikijs for more long-form text. Wikijs also has a lot of advanced features and makes it easier to share and collaborate. This has a major downside: my Joplin notes and Wikijs content live in different locations. This post is about how I set out to merge and sync both together.

# Rough plan

Before starting a project like this, I like to get a general idea of how it will work in the end. We are trying to make 2 unrelated projects play nicely with each other.

I already knew that Wikijs [supports syncing to Git repositories](https://docs.requarks.io/storage/git), after some googling I saw that there is an [open feature request](https://github.com/laurent22/joplin/issues/3670) for Joplin to add Git syncing. At the time I'm writing this, this is still far off from being implemented and ready. However, Joplin does support exporting content as markdown! An idea began to form in my head at this point: we can export the markdown from Joplin, add it to a git repo (which is shared with Wikijs) and push.

So the plan of action is:

- joplin sync # Make sure our local version is the same as the cloud version
- Export Joplin to md with CLI
- git pull # Make sure the local git repo has all the remote changes
- Move md files into git repo under special Joplin/notes folder
- git add
- git commit -m <something>
- git push

It's important to make sure everything is up to date so that we do not get any nasty syncing conflicts.

TODO:
To export the Joplin notes in a script, we have to use the Joplin terminal application.

# Inital script

With our game plan set, we can start implementing this method.

TODO: Wikijs syncing to git enable

```sh
#!/bin/bash

JOPLIN_BINARY=/usr/bin/joplin
WIKI_GIT_FOLER=/home/catalysm/code/joplin-wikijs/wiki


# Make sure all data is in sync
git -C $WIKI_GIT_FOLER pull
$JOPLIN_BINARY sync

$JOPLIN_BINARY export --format md "./tmp"


# Recreate the target folder
rm -r $WIKI_GIT_FOLER/joplin
mkdir -p $WIKI_GIT_FOLER/joplin

# Copy the exported files to the repo
cp -r tmp/* $WIKI_GIT_FOLER/joplin

# Delete the temp folder
# Otherwise joplin will export doubles
rm -r tmp


cd $WIKI_GIT_FOLER

git add joplin/
git commit -m "notes: Joplin export"
git push

```
Great, when I executed this script I saw new commits appearing in Github.