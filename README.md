Step 1: Folder Structure

Open Git Bash (or VSCode terminal) and run:

# Create base projects folder
mkdir -p ~/projects/anil ~/projects/sushil ~/projects/keshav


Now you have three separate folders for each user.

Step 2: Generate SSH Keys for Each Account
# Anil
ssh-keygen -t ed25519 -C "anil@example.com" -f ~/.ssh/id_ed25519_anil

# Sushil
ssh-keygen -t ed25519 -C "sushil@example.com" -f ~/.ssh/id_ed25519_sushil

# Keshav
ssh-keygen -t ed25519 -C "keshav@example.com" -f ~/.ssh/id_ed25519_keshav


When prompted, press Enter to accept the default location.

Optionally, set a passphrase for extra security.

Step 3: Add Public Keys to GitHub

Display each public key:

cat ~/.ssh/id_ed25519_anil.pub
cat ~/.ssh/id_ed25519_sushil.pub
cat ~/.ssh/id_ed25519_keshav.pub


Copy each key and add it to the respective GitHub account:

GitHub → Settings → SSH and GPG keys → New SSH key → Paste → Save

Step 4: Configure SSH Config

Open the SSH config file:

code ~/.ssh/config


Paste:

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


Save and close the file.

This allows you to reference git@github-anil, git@github-sushil, and git@github-keshav in Git commands.

Step 5: Add Keys to SSH Agent
# Start agent
eval "$(ssh-agent -s)"

# Add keys
ssh-add ~/.ssh/id_ed25519_anil
ssh-add ~/.ssh/id_ed25519_sushil
ssh-add ~/.ssh/id_ed25519_keshav

Step 6: Initialize Repos and Set Git Config Per Folder
# Anil repo
cd ~/projects/anil
git init
git config user.name "anil"
git config user.email "anil@example.com"

# Sushil repo
cd ~/projects/sushil
git init
git config user.name "sushil"
git config user.email "sushil@example.com"

# Keshav repo
cd ~/projects/keshav
git init
git config user.name "keshav"
git config user.email "keshav@example.com"

Step 7: Clone GitHub Repos Using SSH Alias
# Replace `repo` with your repository name on GitHub

git clone git@github-anil:anil/repo.git ~/projects/anil
git clone git@github-sushil:sushil/repo.git ~/projects/sushil
git clone git@github-keshav:keshav/repo.git ~/projects/keshav


✅ Each repo will automatically use the correct GitHub account for commits and pushes.

Step 8: Optional — Automatic Switching with Git IncludeIf

If you want Git to automatically pick the user based on folder:

Global ~/.gitconfig:

[user]
    name = anil
    email = anil@example.com

[includeIf "gitdir:~/projects/sushil/"]
    path = ~/.gitconfig-sushil

[includeIf "gitdir:~/projects/keshav/"]
    path = ~/.gitconfig-keshav


~/.gitconfig-sushil:

[user]
    name = sushil
    email = sushil@example.com


~/.gitconfig-keshav:

[user]
    name = keshav
    email = keshav@example.com


Now any repo under ~/projects/sushil/ will automatically commit as Sushil, and so on.

✅ Result

Each folder/repo has a separate GitHub user.

Pushing, pulling, and committing works without switching global users.

SSH keys ensure secure authentication for each account.