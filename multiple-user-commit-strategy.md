This is possible using SSH aliases + per-commit Git configuration. Here’s how to set it up:

Step 1: Ensure SSH Keys for All Accounts

Make sure you already have:

~/.ssh/id_ed25519_anil
~/.ssh/id_ed25519_sushil
~/.ssh/id_ed25519_keshav


And each public key is added to its GitHub account.

SSH config (~/.ssh/config) looks like this:

Host github-anil
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_anil

Host github-sushil
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_sushil

Host github-keshav
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_keshav

Step 2: Configure Multiple Remotes in the Same Repo

Suppose your repo folder is ~/projects/myrepo:

cd ~/projects/myrepo
git remote remove origin  # remove existing origin if any

# Add remotes for each user using SSH alias
git remote add anil git@github-anil:anil/repo.git
git remote add sushil git@github-sushil:sushil/repo.git
git remote add keshav git@github-keshav:keshav/repo.git


Now the same repo has three remotes: anil, sushil, and keshav.

Step 3: Commit with Different Users

Before committing, set the user for that commit:

# Commit as Anil
git config user.name "anil"
git config user.email "anil@example.com"
git commit -m "Commit as Anil"

# Commit as Sushil
git config user.name "sushil"
git config user.email "sushil@example.com"
git commit -m "Commit as Sushil"

# Commit as Keshav
git config user.name "keshav"
git config user.email "keshav@example.com"
git commit -m "Commit as Keshav"


You can even do git config user.name per commit; it overrides the global or repo config temporarily.

Step 4: Push to the Correct Account

Use the remote corresponding to the user:

# Push Anil's commit
git push anil master

# Push Sushil's commit
git push sushil master

# Push Keshav's commit
git push keshav master


Each push uses the SSH alias, so the correct account is used automatically.

✅ Result

One repo can be shared among multiple GitHub accounts.

You choose the commit author and remote each time.

No need to switch global Git user or log in/out.