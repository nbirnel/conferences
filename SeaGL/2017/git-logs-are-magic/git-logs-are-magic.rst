genehack

antipatterns
=============
collapse into 1 commit
rebase before merge
fast forward merge

patterns
========
branch
have a release branch (could be master)
merge into release = make a release


git config --global merge.ff false
git config --global pull.ff only

git push origin --delete branch

feature-name-ticketid

include branch name in merge - now you have a ticket

keep formatting commits separate from logic

git add --patch
git commit --amend

interactive rebase after lots of little "savepoint" commits
git rebase -i

git config --local commit.template ./.template

git log -S <string>

git lg (see the twitterverse for a pretty git tree)

force push to remote topic branches with one worker.



