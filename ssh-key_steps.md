# Add SSH Key (Simple Steps)

## 1. Generate SSH Key (Terminal)

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Press **Enter** for default path, then set a passphrase if you want.

## 2. Start SSH Agent

```bash
eval "$(ssh-agent -s)"
```

## 3. Add SSH Key to Agent

```bash
ssh-add ~/.ssh/id_ed25519
```

## 4. Copy Public Key

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the whole output.

## 5. Add SSH Key to GitHub

* Go to **GitHub → Settings → SSH and GPG keys**.
* Click **New SSH key**.
* Paste your public key.
* Save.

## 6. Test Connection

```bash
ssh -T git@github.com
```

Done.
