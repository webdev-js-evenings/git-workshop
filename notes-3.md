# Webdev — Git 2018 p.3

[![Rebase](https://65.media.tumblr.com/4c5d2f68624a568985c59423a27fcb3c/tumblr_o5fadyZqP61ugyavxo1_1280.jpg)](Rebase)<br>
“Junior programmer's first `git rebase –interactive`“ - Salvador Dalí, 1936, Oil on Canvas, stolen from [@vojtatranta](https://git.io/fNHGk) and [classicprogrammerpaintings.com](http://classicprogrammerpaintings.com/post/142586036029/junior-programmer-learns-git-rebase#notes).


## Intro
- webdev
- summary


## Recap
- branches
- merging
- rebasing
- remotes

- man pages!

## Commit amend

- git commit --amend
- git commit --amend --no-edit

- push
- push --force
- push --force-with-lease

## Rebase -i

"Polish a feature branch, clean quick/bulk commits, fast fixes, fixups, review history.

- git rebase origin/master
- git rebase -i origin/master
- git rebase -i origin/master --exec "echo test pass"
- git rebase -i /hash/
- git rebase -i HEAD~4
- git rebase -i /hash/ --onto origin/master

### Instructions
- p, pick
- r, reword
- e, edit
- s, squash
- f, fixup
- x, exec
- d, drop


### Navigation
- skip
- continue
- abort
- edit todo
- autosquash


### More examples
- split commit to multiple
- onto
- force-with-lease config
- fm, ri, rs, ra, fp config
- pushed branch + rebase fails
- conflicts during rebase (rebase feature onto master frequently!)
- lost commits (dropped, badly resolved conflicts, -> `reflog`)


### Exec examples
  - git rebase --interactive master --exec "! grep -R SpecialistDocument *" --exec rake
  - git rebase --interactive master --exec "! git show | grep SpecialistDocument"
  - (source: http://jamesmead.org/blog/2017-04-12-git-interactive-rebase-with-the-exec-option)
  - git rebase -i --exec "git reset-authors" (source https://til.hashrocket.com/posts/2839b35a3b-execute-a-shell-command-on-every-commit-in-rebase)