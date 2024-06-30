### The Perfect Commit
The golden rule of committing is to commit all the related changes, one should not just cram every change in a single commit.
Commits can have granularity at not just file level but also at line level. the `-p` flag in `git add` lets you go through a file one change (or as git calls it hunk) at a time, and gives you the option to include it or exclude it from the current commit.
<span style="color:#e1db3d">Writing commit messages</span>: There are two sections, the head and the body, the head has just the highlight of the commit (if we cannot narrow it down to one specific message then maybe we have cluttered the commit). When a text editor window pops up to writer the commit msg, we first write the head, then a two newline characters tell git that what follows will be the body of the commit. The body should roughly addresses these questions about the commit:
- What is now different than before ?
- What is the reason of this change ?
- Is there anything to watch out for ? 

### Branching Strategies
