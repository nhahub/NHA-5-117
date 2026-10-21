# How we work on GitHub

We work with forks. The team repo is `Loai-Nour/NHA-5-117`. Loai reviews and merges.

## One-time setup (each teammate)

1. Fork `Loai-Nour/NHA-5-117` on GitHub.
2. Clone your fork and add the team repo as `upstream`:
   ```
   git clone https://github.com/<your-username>/NHA-5-117.git
   cd NHA-5-117
   git remote add upstream https://github.com/Loai-Nour/NHA-5-117.git
   ```
3. Set your identity:
   ```
   git config --global user.name "Your Name"
   git config --global user.email "your-github-email"
   ```

## For every task

```
git checkout main
git pull upstream main
git push origin main
git checkout -b feature/m1-short-task-name
# work, then
git add <files>
git commit -m "Add KITTI to YOLO label converter"
git push -u origin feature/m1-short-task-name
```

Then open a pull request from your branch into `Loai-Nour/NHA-5-117` `main`. After it is merged, delete your branch and sync again.

## Branch names

`feature/m<milestone>-<task>`, for example `feature/m2-sagemaker-train`.

## Commit messages

- Short, in the imperative: "Add mapping tests for BDD100K", "Fix box clipping in augmentation".
- Not: "fix", "update", "final", "final2".
- One logical change per commit.

## Pull requests

- One task per pull request, small enough to review in 10 minutes.
- Fill in the template: what changed, why, how you tested it.
- One teammate reviews before merge. The author does not merge their own pull request.
- Review comments are about the code, not the person.

## Merge conflicts

Sync with `upstream` often, keep commits small, and avoid editing the same file at the same time. If a conflict appears, ask in the group before forcing anything. Never use `git push --force` on `main`.

## Never commit

- AWS keys, tokens, passwords or `.env` files. Use IAM roles and GitHub Actions secrets.
- Datasets, model weights or large files. Use S3 and DVC.
- If a key is ever pushed, treat it as leaked and rotate it in AWS immediately. Deleting the file is not enough.

## Code expectations

- Readable, commented where the logic is not obvious, with clear names.
- Every script has a short header: what it does, inputs, outputs.
- Every member must be able to explain their own code in the final presentation.
