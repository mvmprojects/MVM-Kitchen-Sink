# GIT NOTES

## Basic:

Pushing your fresh solution to an empty repository on github (so create a completely empty repo with NO initialize and NO readme) using only Git for Windows:

1. Read up on personal access tokens on github, and generate a token. 
https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token
2. Install the latest version of Git for Windows (2.35 at time of writing) or look up the right command needed to update an older version of Git. Minimum version required here is 2.29.
3. Go to your project/solution folder, and right-click to open Git Bash in that directory ))

		echo "# Demo Project" >> README.md
		
		git init 
		git add -A
		git commit -m "first commit"
		git branch -M master
		
		git remote add origin https://your-git-server/your-repo.git
	
Where `your-git-server` and `your-repo` should be the server and the repo name. For example: https://github.com/mvmprojects/MVM-Kitchen-Sink.git for the kitchen sink server and repo.

	git push -u origin master
	
Git should open up an authentication window and there should be an option to enter the personal access token from Github.

Dealing with the error that states: "Your push would publish a private email address".
1. Look up your public (noreply) email address on github.
2. Enter commands in Git Bash to deal with the address:

		git config --global user.email "<number>+<name>@users.noreply.github.com"
		git commit --amend --reset-author
	
3. Try to push to the repo again:

		git push -u origin master

The usual pattern whenever you've made your local changes:

	git status
	git add -A
	git commit -m "small changes"
	git push origin master

(be careful when doing this) Add any new changes to last commit to clean up your commit history before pushing.

	git commit --amend	

" Git gives you another option: you can tack the new change on to the previous commit using git commit --amend. This keeps all of your related changes bundled together in one commit, so you can understand it more quickly when you're reviewing it later. "
https://think-like-a-git.net/sections/graphs-and-git/garbage-collection.html

Create test branches that exist only to attempt a merge, leaving you with the option to simply discard this test branch if something goes wrong.
( Remember to check, with a quick 'git status', that you are on branch master with the message "nothing to commit (working directory clean)" )

	git checkout -b test_merge
	git merge new_feature
	
To abort at this point:

	git reset --hard
After a merge, switch to a visualizer and see if the results are what you want.
If the results are satisfactory, switch to master and merge your test branch into it.

	git checkout master
	git merge test_merge
	
If something is wrong, switch to master and drop the failed test branch:

	git checkout master
	git branch -D test_merge

" (...) You're not sure if this will be a good idea, so you want to try out the merge, but be able to abort it if things don't go smoothly. "
https://think-like-a-git.net/sections/testing-out-merges/the-scout-pattern.html

## Advanced

HOW TO: remove a file from git repo without losing it on your machine:

	git rm --cached MySecret.txt

Now you can push and the file will be removed from your repo, while still being be left intact on your local machine.

From then on, you can leave it added to your .gitignore file.

To remove an entire folder, do it recursively:

	git rm -r --cached YourFolder

Another option is to "forget" a file, according to stack overflow:

	git update-index --skip-worktree YourFile.txt

## Opinionated

What follows is a plan for *using rebase as a merging strategy*:

- Create a new branch for a feature.
- Make changes and commit them.
- Check out the develop (main) branch and fetch/pull from upstream.
- Check back into the feature branch
- Rebase the current branch on top of develop, so that commits are re-played on top of develop.
- Create a PR to merge into develop.
- Do the merge.
The branch eventually gets deleted, but all the changes are now in develop and we're ready for the next one.

The official git documentation already compares merge to rebase.
https://git-scm.com/book/en/v2/Git-Branching-Rebasing

	$ git checkout experiment
	$ git rebase master
	First, rewinding head to replay your work on top of it...
	Applying: added staged command

The documentation page linked to above includes a **very thorough warning** against rebasing commits that have already been pushed:

"If you rebase commits that have already been pushed publicly, and people may have based work on those commits, then you may be in for some frustrating trouble, and the scorn of your teammates. (...) Now, to the question of whether merging or rebasing is better: hopefully you’ll see that it’s not that simple. (...) You can get the best of both worlds: rebase local changes before pushing to clean up your work, but never rebase anything that you’ve pushed somewhere."
