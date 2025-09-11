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

-------------------------------------------------------
✅ Recommended Approach (Clear Separation)

Clone the repo 3 times in 3 separate folders

~/projects/repo-anil
~/projects/repo-sushil
~/projects/repo-keshav


For each repo clone:

Configure identity

git config user.name "Anil Gupta"
git config user.email "anil@example.com"


Use separate SSH keys for each GitHub account and map them in ~/.ssh/config:

Host github-anil
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_anil

Host github-sushil
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_sushil

Host github-keshav
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_keshav


Clone using the right alias, e.g.:

git clone git@github-anil:your-org/repo.git repo-anil
git clone git@github-sushil:your-org/repo.git repo-sushil
git clone git@github-keshav:your-org/repo.git repo-keshav


🔑 Why this is best:

Keeps identities isolated.

You won’t accidentally commit as the wrong user.

Mirrors a real team setup (3 users, same repo).

⚡ Alternate Quick Approach (not recommended for long-term)

Keep one repo clone.

Before committing, switch identity with:

git config user.name "Sushil"
git config user.email "sushil@example.com"


Push using different SSH key with:

GIT_SSH_COMMAND="ssh -i ~/.ssh/id_rsa_sushil" git push origin branch-name


👉 Works, but easy to make mistakes (you might forget to switch users).

🎯 Conclusion

For best practice (learning + close to real-world) →
✅ Use 3 clones of the repo, each with its own GitHub account + SSH key.