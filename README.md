# git && github
- clean previous accounts
```bash
git config --global --list
git config --global --unset user.name
git config --global --unset user.email
git config --global --unset credential.helper
```

- clean ssh keys
```bash
rm -rf ~/.ssh
mkdir ~/.ssh
chmod 700 ~/.ssh
```

- generate ssh keys personal and work
```bash
ssh-keygen -t ed25519 -C "trishitchar@gmail.com" -f ~/.ssh/id_ed25519_personal
ssh-keygen -t ed25519 -C "trishit@work-email.com" -f ~/.ssh/id_ed25519_work
```

- start ssh agent
```bash
eval "$(ssh-agent -s)"

ssh-add ~/.ssh/id_ed25519_personal
ssh-add ~/.ssh/id_ed25519_work
```

- add into config
```bash
nano ~/.ssh/config

# Personal GitHub
Host github-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal
  IdentitiesOnly yes

# Work GitHub
Host github-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_work
  IdentitiesOnly yes
```

- view the keys
```bash
cat ~/.ssh/id_ed25519_personal.pub
cat ~/.ssh/id_ed25519_work.pub
```

- check
```bash
ssh -T git@github-personal
ssh -T git@github-work
```

- add globally
```bash
git config --global user.name "trishitchar"
git config --global user.email "trishitchar@gmail.com"
git config --global --list
```

- runnable script
```bash
nano ~/set-work-git.sh
#!/bin/bash

WORK_NAME="trishit Work"
WORK_EMAIL="trishitchar@gmail.com"

echo "Applying WORK Git config..."

# 1. Set local identity (repo-specific)
git config user.name "$WORK_NAME"
git config user.email "$WORK_EMAIL"

# 2. Ensure correct SSH remote (important)
REMOTE_URL=$(git remote get-url origin)

if [[ "$REMOTE_URL" == *"github.com"* ]]; then
  NEW_URL=$(echo "$REMOTE_URL" | sed 's/github.com/github-work/')
  git remote set-url origin "$NEW_URL"
  echo "Updated remote to use github-work SSH alias"
fi

echo "Done. Current config:"
git config user.name
git config user.email
git remote -v
```

```bash
chmod +x ~/set-work-git.sh
~/set-work-git.sh
-
/c/Users/trish/scripts/git-account-fix.sh
```
- 
```bash
git remote -v
git remote set-url origin git@github-personal:trishitchar/linux-mint-configs.git
```

# Out of Memory issue (OOM) - earlyoom
```bash
sudo apt install earlyoom
sudo systemctl enable --now earlyoom
sudo nano /etc/default/earlyoom
```
- EARLYOOM_ARGS="-r 3600" ----> EARLYOOM_ARGS="-m 20 -s 20 -r 60"
```bash
sudo systemctl restart earlyoom
systemctl status earlyoom
journalctl -u earlyoom -f
```
- Finding
```
Windows & Linux handles memory differently, windows allocates memory dynamically where linux defines swap space manually, so to prevent freez in linux you've to increase swap space manually (>=6GB)
```

Tools:
1. CopyQ
2. FlameShot
3. ncdu for visual df -h
   ```bash
   sudo apt install ncdu
   sudo ncdu /
   ```
4. Timeshift delete
   ```bash
   sudo timeshift --list
   sudo timeshift --delete-all
   sudo apt purge timeshift -y
   ```
5. some cleanups
   ```bash
   sudo apt-get clean
   sudo apt-get autoclean
   sudo apt-get autoremove -y
   journalctl --disk-usage
   sudo journalctl --vacuum-time=7d
   ```
   





- will add this to my blog page in linux section later
   
