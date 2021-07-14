- [iSH](https://ish.app) is a _free_ and [open-source](https://github.com/ish-app/ish) iOS app that runs real-world [Alpine Linux](https://alpinelinux.org/about/) on your iDevice by emulating an Intel x86 CPU. It can run many native x86 programs (_not_ compiled for iOS!), including Git.
- It can also [`mount` iOS file providers](https://github.com/ish-app/ish/wiki/Mounting-other-file-providers), which lets you access your local vault in Obsidian.

These features allow you to sync your Obsidian vault with GitHub (private and public) without leaving your iDevice.

# Workflow

Assuming you have an _existing_ repo on GitHub. You can also create the repo right from iSH - the `git` command you'll be using is the exact same fully-featured `git` from Alpine Linux.

1. Create a new empty _local_ vault in Obsidian
2. In iSH:
	1. Create a directory called `obsidian` in your home directory by running `cd ~ && mkdir obsidian`.
	2. Mount your local vault folder into the `obsidian` folder
		1. Run `mount -t ios . obsidian`
		2. A file picker will show up
		3. Choose the folder with your local vault
	3. Clone your git repository into `obsidian`
		1. Change directory to `obsidian`: `cd obsidian`
		2. Delete the `.obsidian` folder: `rm -rf .obsidian`
		3. `git clone https://github.com/ForceBru/ObsidianVaultTest .` - use your own repository instead. Don't forget the period `.` - [this is what allows you to clone the repo _into the current folder_](https://stackoverflow.com/questions/9864728/how-to-get-git-to-clone-into-current-directory).
		4. (Optional) Run `ls -a` to see whether your files are there
	4. In Obsidian
		1. Restart Obsidian (might not be necessary)
		2. Open the file explorer
			1. Currently (Obsidian 1.0.3) it's empty (looks like a bug)
			2. Tap the "Sort" button (the rightmost one, with up/down arrows)
			3. Select any sorting order you like
			4. The files should appear
		3. Use your Obsidian vault!
3. To __push__ your changes, go back to iSH
	1. Change directory to `obsidian` in your home directory: `cd ~/obsidian`
	2. Run `git status` to confirm that there _are_ modified files
	3. _First-time setup_ - tell git who you are:
		1. [Set username](https://docs.github.com/en/get-started/getting-started-with-git/setting-your-username-in-git) for _this_ repository only (or set it globally with a slightly different command): `git config user.name ForceBruMobile`
		2. [Set e-mail address](https://docs.github.com/en/github/setting-up-and-managing-your-github-user-account/managing-email-preferences/setting-your-commit-email-address#setting-your-commit-email-address-in-git): `git config user.email "ForceBru@users.noreply.github.com"`
	4. Add files, commit and push. For example:
		1. Add _all_ files into the commit: `git add .`
		2. Commit: `git commit -m "Commit from mobile"`
		3. Push: `git push`
			1. This will ask you to enter your username and password
			2. [Git can use your personal access token to login automatically](https://docs.github.com/en/get-started/getting-started-with-git/why-is-git-always-asking-for-my-password)
		4. That's it! Take a look at [this example commit from iSH](https://github.com/ForceBru/ObsidianVaultTest/commit/f642c3334a870ca8ab2aa1355528403502433b2b)
4. To __pull__ changes from GitHub:
	1. In iSH:
		1. Change directory to `obsidian` in your home directory: `cd ~/obsidian`
		2. (Optional) Check whether there is stuff to pull
			1. Bring remote refs up to date: `git remote update`
			2. Check status: `git status`
				1. It should say something like `Your branch is behind 'origin/master' by 2 commits, and can be fast-forwarded` if there is something to be updated
		3. Actually update from the remote repo: `git pull`
	2. In Obsidian:
		1. Go to file explorer
		2. If you don't see your new notes, do the sort trick again:
			1. Tap the "Sort" button (the rightmost one, with up/down arrows)
			2. Select any sorting order you like
			3. The files should appear
		3. Updates to _existing_ notes seem to be reflected correctly