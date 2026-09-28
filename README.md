# github-assignment

### Once, if you haven't already

1. Install git on your computer.
2. Set up git on your computer, configuring your name and email address (the email you use on GitHub):
   ```
   git config --global user.name "Becky Smith"
   git config --global user.email "smith.b@northeastern.edu"
   ```
3. Create a GitHub account (use a professional, recognizable, memorable, easy-to-type, username).
4. Use your .edu email address to sign up for student developer pack goodies from GitHub.
5. Let git sign in to GitHub. GitHub no longer accepts your password for `git push`, so do one of these:
   - in VS Code, sign in to GitHub from the Accounts icon (bottom left), then push from the Source Control panel;
   - install the [GitHub CLI](https://cli.github.com/) and run `gh auth login` once; or
   - set up an [SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) (the same idea as logging in to Explorer), and clone with the `git@github.com:…` address instead of `https://…`.

   On Windows, Git for Windows usually handles this for you: the first push opens a browser window to sign in.

### The assignment

You might need to look up some of the terminology (fork, repository, clone, branch...), but try to follow the steps carefully.

1. Fork this repository to your account.
2. Clone your fork to your computer.
3. Create a new branch called something like `Becky` (where you use your own name), and switch to (or check out) that branch. Don't make your changes on `master`.
4. Add your details in the table at the bottom of this README.md file. Copy the template line, and carefully follow an example that's already there, like Richard West. It is formatted in [Markdown](https://www.markdownguide.org/), or more specifically [GitHub Flavored Markdown](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax), so be careful with the pipes and dashes.
5. Save your changes.
6. Stage your edits.
7. Commit your changes, with a commit message, to your branch.
8. Push your branch to your fork on GitHub. The first time you push a new branch, tell git where it goes:
   ```
   git push -u origin Becky
   ```
   After that, plain `git push` is enough.
9. Check that it looks right on GitHub: on your fork, pick your branch from the branch menu, and check that your row and avatar show up in the table.
10. Open a Pull Request, asking to merge your `Becky` branch (or whatever it's called) into `master` of [CHME5137/github-assignment](https://github.com/CHME5137/github-assignment), the original repository, not your fork. Right after you push, GitHub usually offers a **Compare & pull request** button. Give it a short description: what it changes, and (since AI use is encouraged on this assignment) what you asked an AI assistant along the way, and what it told you.

### If your pull request has conflicts

Everyone is editing the same table, so if someone else's pull request is merged first, GitHub may say your branch *has conflicts that must be resolved*.
That's normal, and fixing it is part of the exercise:

1. On your fork's GitHub page, click **Sync fork**, to bring your fork's `master` up to date.
2. On your computer, on your branch:
   ```
   git fetch origin
   git merge origin/master
   ```
   (`git pull origin master` will refuse, with *Need to specify how to reconcile divergent branches*.)
3. Open README.md, find the lines between `<<<<<<<` and `>>>>>>>`, and keep **both** rows, in alphabetical order. Delete the `<<<<<<<`, `=======` and `>>>>>>>` lines.
4. Stage, commit and `git push`. The pull request updates itself.

Ask for help if you get stuck.


## 2026

People who have completed this assignment, in alphabetical order of last name.

To add yourself, copy this line into the table below, in the right place alphabetically, and replace every `LastName`, `FirstName`, `husky.id` and `githubid` (the GitHub username appears four times):

```
LastName  | FirstName  | husky.id   | [githubid](https://github.com/githubid) | ![githubid](https://github.com/githubid.png?size=40)
```

Last Name | First Name | husky id   | github id | avatar
----------|------------|------------|-----------|---------
Preston-Werner | Tom   | preston-werner.t | [mojombo](https://github.com/mojombo) | ![mojombo](https://github.com/mojombo.png?size=40)
Torvalds  | Linus      | torvalds.l | [torvalds](https://github.com/torvalds) | ![torvalds](https://github.com/torvalds.png?size=40)
West      | Richard    | r.west     | [rwest](https://github.com/rwest) | ![rwest](https://github.com/rwest.png?size=40)
