# Python-Days

This repository includes a simple Git hook setup to help block accidental commits or pushes that contain secret-like content.

## What this setup does

The repository uses local Git hooks to scan staged content before a commit and scan the working tree before a push.

- `pre-commit` checks staged files for common secret patterns such as AWS keys, GitHub tokens, Google API keys, and private key blocks.
- `pre-push` performs a repository-wide scan and blocks the push if any suspicious secret-like text is found.

## How to enable this in a new repository

When you initialize a new Git repository, follow these steps:

1. Initialize Git:
   ```bash
   git init
   ```

2. Create the hook files in the repository hooks directory:
   ```bash
   mkdir -p .git/hooks
   ```

3. Add the hook scripts:
   - `.git/hooks/pre-commit`
   - `.git/hooks/pre-push`

4. Make them executable:
   ```bash
   chmod +x .git/hooks/pre-commit .git/hooks/pre-push
   ```

5. Ensure Git uses the repository-local hooks directory:
   ```bash
   git config core.hooksPath .git/hooks
   ```

6. Confirm the hook is active:
   ```bash
   git config --get core.hooksPath
   ```

## Project-specific usage

After enabling hooks, every new commit or push will be checked automatically.

If a secret-like value is detected:

- the `pre-commit` hook will block the commit
- the `pre-push` hook will block the push

You should then remove the secret from the file, stage the corrected content, and retry the commit or push.

## Notes

- This is a local repository safeguard and is not a replacement for secret management tools like environment variables, secret stores, or cloud vault services.
- The patterns are intentionally simple and should be treated as a first layer of defense.
- For production use, consider stronger scanning tools such as GitHub secret scanning, TruffleHog, or similar providers.

## Quick start

```bash
git init
chmod +x .git/hooks/pre-commit .git/hooks/pre-push
git config core.hooksPath .git/hooks
```

Where to get them
You do not download those hook scripts from anywhere. They are created locally inside your Git repository.

In this repo, the active hook scripts are already present here:

pre-commit
pre-push
If you are setting up a new repository, you can create them yourself with:

cat > .git/hooks/pre-commit <<'EOF'
#!/usr/bin/env sh
# hook logic here
EOF

cat > .git/hooks/pre-push <<'EOF'
#!/usr/bin/env sh
# hook logic here
EOF

chmod +x .git/hooks/pre-commit .git/hooks/pre-push
git config core.hooksPath .git/hooks

Meaning of “Add the hook scripts”
It means:

create the files in hooks
put your hook code inside them
make them executable
