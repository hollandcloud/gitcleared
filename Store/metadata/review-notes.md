# App Review notes

The block between the `---` fences is what App Review reads. `Scripts/asc.py
review` sends it, with the token filled in from the `GITCLEARED_REVIEW_TOKEN`
environment variable. The token is never written into this file. The demo
account fields stay empty: the app signs in with a GitHub token, not a username
and password.

---

GitCleared is a menu bar app. It has no Dock icon and no main window. After
launch, look at the right-hand side of the menu bar for its icon and click it
to open the panel.

SIGNING IN. The app works against a GitHub account, so it needs a GitHub
personal access token. The token below belongs to the developer and is limited
to one demo repository, github.com/hollandcloud/gitcleared-demo, which exists
for this review. On first launch the panel shows "Connect with a personal
access token": paste the token into the field and click Connect.

TOKEN: <replace with a token for the review account before submitting>

WHAT YOU SHOULD SEE. Within a few seconds of connecting:

• Holding — "gitcleared-demo · Deploy", waiting at the production environment.
  Click Clear to approve it; the deployment then runs on GitHub (it only prints
  a line, nothing is really deployed) and leaves the list.
• Ready to merge — "Say when the status was last checked", with a Merge button.
  Merging it merges that pull request on GitHub.
• In flight — pull requests that cannot be merged yet, each with the reason:
  a draft in the demo repository, and possibly a pull request the developer has
  open on a public open-source project, shown without a Merge button.

Click any row to open it on GitHub. Right-click a row for Copy Link, and on a
held deployment, Reject.

START A WORKFLOW. Click the play icon at the bottom of the panel, choose
hollandcloud/gitcleared-demo, then "Deploy". The sheet shows the inputs the
workflow declares. Run starts it on GitHub; it then waits at production and
appears under Holding.

OTHER CONTROLS, along the bottom of the panel: refresh, Settings (the gear, or
Command-comma), and Quit (the power icon, or Command-Q).

NOTIFICATIONS. The app asks for notification permission after connecting. It
uses them only to say that a deployment has started holding. Declining changes
nothing else.

LAUNCH AT LOGIN is off by default and is only turned on by the switch in
Settings.

WHY THERE IS NO "USE MY GH CLI LOGIN" BUTTON. The version on GitHub can import
a login from GitHub's command-line tool. This version runs in the App Sandbox,
which does not allow that, so a pasted token is the only way to sign in.

The app talks only to api.github.com. It has no server of its own, no
analytics and no advertising.

---

## Before submitting

**1. Make the review token** — on GitHub, as `hollandcloud`:
https://github.com/settings/personal-access-tokens/new

| Setting | Value |
|---|---|
| Token name | GitCleared App Review |
| Expiration | 90 days — long enough to outlast a review and a resubmission |
| Resource owner | hollandcloud |
| Repository access | Only select repositories → `gitcleared-demo` |
| Actions | Read and write |
| Deployments | Read and write |
| Contents | Read and write |
| Pull requests | Read-only |

Store it in Infisical. It is the only copy outside GitHub.

**2. Put a deployment on hold.** Approving the held one uses it up, and GitHub
drops a waiting run after 30 days:

```sh
gh workflow run deploy.yml -R hollandcloud/gitcleared-demo
```

Run it again after any review that cleared the last one, and before a
resubmission.

**3. Send the notes with the token, then submit:**

```sh
GITCLEARED_REVIEW_TOKEN=<the token> Scripts/asc.py review
Scripts/asc.py submit      # refuses while the notes still have the placeholder
```

If the reviewer merges the ready pull request, open another one in the demo
repository before resubmitting, so there is still something to merge.

Revoke the token once the app is approved.
