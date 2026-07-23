---
name: push-skill-to-gh
description: Push an existing Codex skill folder to g-savitha/ai-stuff on GitHub. Use when the user asks to publish, sync, or push a skill to that repository; inspect the repository structure first, use gh for authentication and repository details, detect an existing skill before changing anything, and report clearly when the skill is already present.
---

# Push Skill to GitHub

Publish one existing skill directory to `git@github.com:g-savitha/ai-stuff.git` safely and in the repository's established layout.

## Inputs

Treat the user's argument as the source skill path or skill directory name. Resolve it to a directory containing `SKILL.md`; if it is ambiguous, stop and ask for the exact path. Do not publish a parent directory that merely contains multiple skills.

## Workflow

1. Validate prerequisites without changing files:

   ```sh
   command -v gh
   command -v git
   gh auth status
   gh repo view g-savitha/ai-stuff --json nameWithOwner,defaultBranchRef,sshUrl,url
   ```

   Use `gh` for authentication and repository metadata. If authentication is missing or invalid, tell the user to run `gh auth login -h github.com` and stop. Do not handle or print tokens.

2. Inspect the remote repository before choosing a destination. Prefer a temporary clone of the default branch:

   ```sh
   temp_dir="$(mktemp -d)"
   git clone --depth 1 git@github.com:g-savitha/ai-stuff.git "$temp_dir/repo"
   find "$temp_dir/repo" -maxdepth 3 -type f | sort
   ```

   Also inspect directories containing `SKILL.md`, existing `agents/openai.yaml` files, and the repository README or contribution guidance if present. Infer the destination root and naming convention from what is actually in the repository. Never assume a `skills/` directory if the repository uses another layout.

3. Validate the source skill. Require `SKILL.md` with YAML frontmatter containing `name` and `description`. Preserve the skill's directory contents, including `agents/`, `scripts/`, `references/`, and `assets/`; exclude unrelated parent files. Reject a source whose folder name does not match its frontmatter name unless the user explicitly says which name to use.

4. Check whether the skill already exists before copying. Compare the normalized skill name and candidate destination path against the remote checkout, including every directory containing `SKILL.md`. If it exists, make no changes and report its repository path and whether its contents appear identical or different. Do not overwrite, delete, or merge an existing skill automatically.

5. If it does not exist, copy the skill into the repository's inferred skill root. Review the resulting diff for accidental files, secrets, absolute local paths, generated caches, and oversized binaries. Keep `SKILL.md` concise and ensure `agents/openai.yaml` is valid when present.

6. Create a descriptive branch, commit only the new skill, push the branch with `git push -u origin <branch>`, and open a pull request with `gh pr create` unless the user explicitly requested a direct push to the default branch. Prefer a branch such as `codex/push-<skill-name>` and a PR title such as `Add <skill-name> skill`.

7. Report the repository, destination path, branch, commit, and PR URL. If any step fails, preserve the temporary checkout for diagnosis when practical, explain the exact failure, and do not claim the skill was pushed.

## Safety rules

- Never force-push, reset hard, overwrite an existing skill, or modify unrelated repository files.
- Before committing, run the skill validator if available:

  ```sh
  python3 /Users/savvy/.codex/skills/.system/skill-creator/scripts/quick_validate.py <skill-directory>
  ```

- Inspect `git diff --check` and `git status --short` before committing.
- Do not expose GitHub credentials, SSH private keys, environment secrets, or token values in output.
- If the repository structure is unclear or the source skill is invalid, stop and explain what needs clarification.
