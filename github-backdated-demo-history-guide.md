# Create a Backdated Git History (Demo Only)

> This creates **synthetic** commits for a demo/test repository. It does
> not represent real past work and should not be presented as genuine
> work history.

## 1. Create a new GitHub repository

Create an empty **public** repository on GitHub. Then open a terminal:

``` bash
mkdir github-history-demo
cd github-history-demo
git init -b main
git config user.name "gensintatizem-boop"
git config user.email "gensinta.tizem@gmail.com"
git remote add origin https://github.com/gensintatizem-boop/portfolio-log.git
```

## 2. Save this as `make_history.py`

``` python
import os
import subprocess
from datetime import datetime, timedelta, timezone
from pathlib import Path

log = Path("SYNTHETIC_DEMO_HISTORY.md")
log.write_text("# Synthetic demo history\\n\\n", encoding="utf-8")

today = datetime.now(timezone.utc)

for day in range(730, -1, -7):  # About one commit per week over two years
    date = today - timedelta(days=day)
    stamp = date.replace(hour=12, minute=0, second=0, microsecond=0).isoformat()

    with log.open("a", encoding="utf-8") as f:
        f.write(f"- Demo entry: {date.date()}\\n")

    subprocess.run(["git", "add", str(log)], check=True)
    env = os.environ.copy()
    env["GIT_AUTHOR_DATE"] = stamp
    env["GIT_COMMITTER_DATE"] = stamp
    subprocess.run(
        ["git", "commit", "-m", f"docs: add demo entry for {date.date()}"],
        env=env,
        check=True,
    )
```

## 3. Run the script and push

``` bash
python make_history.py
git push -u origin main
```

## 4. Check your profile

For eligible commits to appear in your contribution graph:

-   The commit email must be linked to your GitHub account.
-   The repository must not be a fork.
-   Commits must be on the repository's default branch (or qualifying
    `gh-pages` branch).
-   GitHub may take some time to update the graph.

See [GitHub's contribution
rules](https://docs.github.com/en/account-and-profile/concepts/contributions-on-your-profile).

**Note:** This produces a synthetic timeline, not proof that you worked
on the project during those dates. For a professional profile, use
genuine historical work where possible.
