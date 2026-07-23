# Assignment 6 — Building an AI-Assisted Git Safety Net (PR Ready Check)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In Week 2 you built Claude Code hooks that block a dangerous action *before* it happens (`PreToolUse`), and a restricted skill that could look but not touch (`allowed-tools` without `Write`). In this assignment you will discover that Git has the exact same idea, decades older: a **pre-commit hook** that blocks a commit before it's created.

You will build both halves of a real "PR Ready" workflow:

1. A **Git hook that follows fixed rules** — scans staged changes for hardcoded secrets and oversized files and refuses the commit. No AI involved, no guessing, just a rule that gives the same answer every time.
2. A **restricted Claude Code skill** (`/pr-ready`) that reads your staged diff and drafts a Pull Request title, description, and a short list of things worth a second look — the kind of judgment a fixed rule can't make (mixed changes, missing context, unclear intent). The skill never commits, pushes, or opens the PR. You do that yourself, using its draft as a starting point.

This mirrors the Agentic Loop from Week 3's Linux triage assignment: **Gather → Analyze → Human Act → Verify**. The hook and the skill both gather and analyze; only you act.

---

# Task 0 — Confirm Your Fork and Create a Feature Branch

## Goal

Confirm you are working in your own fork, then create a dedicated branch for this assignment.

### Evidence

#### Screenshot 1 — Output of git remote -v and git branch showing the new branch

![screenshot-1](screenshots/gideon-omole-as6-scr1.png)

---

### Notes

**1. Why create a dedicated branch instead of doing this work on main?**

**Why create a dedicated branch instead of working on main?**

Working on a separate branch keeps `main` clean and stable. If something breaks or needs changes, it stays isolated on the feature branch and doesn't affect the main codebase. It also makes it easy to review the changes via a Pull Request, and lets me safely experiment or undo work without risk.

---

# Task 1 — Stage a Change With Realistic Risk

## Goal

On your own fork of this repository (the one you've been submitting your DMI work in since onboarding), create a new branch and stage a change that a real reviewer should catch: a hardcoded-looking secret and a leftover debug statement.

### Evidence

#### Screenshot 1 — Output of  `git status` showing the staged file on feature/ai-pr-ready

![screenshot-2](screenshots/gideon-omole-as6-scr2.png)

---

### Notes

**1. Why does this assignment use an obviously fake key instead of a real one?**

Add your answer here.

---

# Task 2 — Write a Real Git Pre-Commit Hook

## Goal

Create a tracked, shareable pre-commit hook that blocks a commit containing secret-like patterns or files over 1MB.

### Evidence

#### Screenshot 2 — `hooks/pre-commit` open in VS Code showing the full script

![screenshot-2](screenshots/gideon-omole-as6-scr2.1.png)

---

#### Screenshot 3 — Output of `git config core.hooksPath` confirming it points to `hooks`

![screenshot-3](screenshots/gideon-omole-as6-scr3.png)

---

### Notes

**1. Why is `hooks/pre-commit` tracked in the repo instead of living only in `.git/hooks/`?**

Because .git/hooks/ is local only — it never gets pushed to GitHub or shared with anyone else. If you put the hook there, it only protects your machine, and every teammate would have to manually recreate it themselves.

By putting the hook in a regular tracked folder like hooks/ and pointing Git to it with core.hooksPath, the hook becomes part of the project itself. Anyone who clones the repo and sets core.hooksPath gets the same protection automatically — it's shareable, version-controlled, and consistent for the whole team.

---

**2. Compare this to `PreToolUse` from Week 2 Assignment 6. What does each one intercept, and what do they have in common?**

PreToolUse intercepts an AI agent's action before it runs a tool (like before Claude executes a command or edits a file).
pre-commit hook intercepts a developer's action before Git finalizes a commit.

What they have in common:
Both are "gatekeeper" checkpoints — they pause right before something risky/irreversible happens (running a tool vs. committing code) and give you a chance to inspect, validate, and block it if it fails a safety rule. They both follow the same idea: catch problems before they happen, not after.

---

# Task 3 — Prove the Hook Blocks the Risky Commit

## Goal

Attempt to commit the staged file from Task 1 and show the hook rejecting it.

### Evidence

#### Screenshot 4 — Terminal showing `git commit` rejected with the hook's "BLOCKED" message naming the exact file

![screenshot-4](screenshots/gideon-omole-as6-scr4.png)

---

### Notes

**1. Which line in `hooks/pre-commit` matched your fake key, and why did it match?**

The match happened on this line:

if git diff --cached -- "$file" | grep -qE 'AKIA[0-9A-Z]{16}|-----BEGIN (RSA|OPENSSH|PRIVATE) KEY-----'; then

This line uses grep to search through the changes you staged, looking for a pattern that matches an AWS access key. AWS keys always start with the letters AKIA followed by 16 uppercase letters or numbers. Since the fake key in scripts/notify.sh followed that exact pattern, grep found the match and the script printed the "BLOCKED" message and stopped the commit.

---

**2. Could this hook have caught a poorly-named variable that stores a secret without the `AKIA` prefix? What does that tell you about the limits of a fixed rule like this?**

No, it could not catch that. The hook only knows how to recognize specific patterns like AKIA... for AWS keys or -----BEGIN RSA PRIVATE KEY----- for private keys. If a secret is stored in a variable with a random or unrelated name, and it does not match one of those exact patterns, the hook has no way of knowing it is sensitive information. It will let the commit through.

This shows the biggest limitation of rule based checks like this one. They can only catch what they are specifically told to look for. They are good at stopping known, predictable patterns, but they cannot understand context or intent. A smarter or more secret often slips past because the rule was written for a specific shape of data, not for the actual meaning behind it. This is why many real world setups combine simple pattern matching tools like this with more advanced secret scanning tools that use broader detection logic.

---

# Task 4 — Build the `/pr-ready` Skill

## Goal

Create a manually invoked Claude Code skill that reads your staged changes and produces a PR-readiness report and a draft PR description — without writing, committing, or pushing anything itself.

### Evidence

#### Screenshot 5 — `SKILL.md` frontmatter showing `allowed-tools: Bash, Read, Grep` (no `Write`) and `disable-model-invocation: true`

![screenshot-5](screenshots/gideon-omole-as6-scr5.png)

---

#### Screenshot 6 — `/pr-ready` output while the risky file is still staged, showing it flagged the secret and/or debug statement

![screenshot-6](screenshots/gideon-omole-as6-scr6.png)

---

### Notes

**1. Why does `/pr-ready` have `Bash` and `Read` but not `Write`?**

The whole point of /pr-ready is to review and report, not to change anything. Giving it Bash and Read lets it look at what's happening (run git diff --cached, git status, inspect file contents) but deliberately leaving out Write means it physically cannot create or modify any file, even by accident. This turns the skill's job description ("never commit, push, or edit files") into an enforced rule rather than just an instruction it might ignore. Even if the skill's prompt tried to sneak in a file edit, the tool permissions would block it. It is a safety boundary built into the permissions, not just something written in plain English.

---

**2. The pre-commit hook and `/pr-ready` both looked at the same staged diff. Did they flag the same things? What did one catch that the other didn't?**

Both caught: the hardcoded secret (the fake AWS key), since the pre-commit hook's regex specifically looks for patterns, and /pr-ready also flagged it as a credential-shaped string during its review.

What the pre-commit hook caught that /pr-ready likely didn't emphasize the same way: the file size check (oversized files), since that is a fixed, mechanical rule the hook applies that has nothing to do with reading code content.

What /pr-ready caught that the pre-commit hook could not: things like the debug echo statement, leftover TODO/FIXME comments, or a diff that mixes unrelated changes together. The pre-commit hook only understands rigid pattern matches (a regex and a byte count) — it has no idea what "debug code" or "unrelated concerns" even mean. /pr-ready can actually read and understand the code, so it catches quality and clarity issues that go beyond simple pattern matching.
---

# Task 5 — Fix the Issues and Re-Verify

## Goal

Remove the secret and debug statement, then prove both gates now pass clean.

### Evidence

#### Screenshot 7 — `git commit` succeeding after the fix (no BLOCKED message)

![screenshot-7](screenshots/gideon-omole-as6-scr7.png)

---

#### Screenshot 8 — Second `/pr-ready` run showing a clean risk report and a drafted PR title + description

![screenshot-8](screenshots/gideon-omole-as6-scr8.png)

---

### Notes

**1. What exactly did you change to satisfy the pre-commit hook?**

I edited scripts/notify.sh to remove the two lines that triggered the hook's checks. Specifically, I removed the hardcoded fake AWS key, which matched the hook's pattern, and I removed the debug echo statement (echo "DEBUG: token is $AWS_ACCESS_KEY_ID") that printed the key's value to the terminal. The file now only contains the shebang line, an explanatory comment, and a harmless status message (echo "Sending notification..."). Once those two lines were gone, the diff no longer matched any secret-shaped pattern, so git commit succeeded with no BLOCKED message from the hook.

---

# Task 6 — Push and Open a Pull Request Using the AI Draft

## Goal

Push your branch and open a real Pull Request, using `/pr-ready`'s drafted title and description as your starting point — read it critically and edit before you use it.

**Important:** Open this Pull Request with base repository set to **your own fork** — not the shared upstream `pravinmishraaws/devops-micro-internship-pravinmishra` repository. This assignment's hook and skill files are your own practice work, not a change meant for the shared class repo.

### Evidence

#### Screenshot 9 — Your Pull Request showing the base repository is your own fork, plus the title and description, with the `/pr-ready` draft visible for comparison (paste it in the PR conversation or your notes below)

![screenshot-9](screenshots/gideon-omole-as6-scr9.png)
---

#### PR Link

https://github.com/Gideon-Omole/devops-micro-internship-pravinmishra/pull/1

---

### Notes

**1. What, if anything, did you edit in the AI's drafted PR description before using it? Why?**

I edited the draft because it labeled the change as "chore: remove placeholder notification script," which technically described what happened but missed the real story. The actual reason notify.sh was changed was to demonstrate the pre commit hook and the /pr ready skill catching a hardcoded secret and a debug echo statement, not just to clean up a placeholder. I rewrote the title and description to explain why the file changed, not just what changed, and made sure the script still did something useful instead of ending up empty.
---

**2. If you had blindly copy-pasted the AI's draft without reading it, what could go wrong?**

If I had copy pasted the draft as is, the PR would have completely misrepresented the change. It framed this as a routine cleanup chore when it was actually a security fix that removed a hardcoded credential and a debug statement leaking it. A reviewer reading a title like "remove placeholder notification script" would have no idea a secret was ever involved, which defeats the point of having a clear history. It also missed that the resulting script would output nothing at all, which could let a broken script slip through unnoticed. This is the same kind of problem the assignment scenario warns about, where a PR description says "minor copy fix" while the real change is something much bigger.
---

**3. Why does this PR need to target your own fork instead of the shared upstream repository?**

This work is coursework and personal practice, not something meant to become part of the original pravinmishraaws/devops micro internship project. Opening it against upstream would mean pushing demo scripts and fake credentials into a shared repository that other students and the maintainer rely on, which is not appropriate. Keeping the PR inside my own fork lets me practice the full Git and GitHub workflow, branching, committing, pushing, and opening a PR, in a space that belongs to me, without affecting the shared upstream codebase.

---

# Task 7 — Map the Workflow to the Agentic Loop

## Goal

Explain this assignment's workflow using the same Gather → Analyze → Human Act → Verify structure from Week 3.

### Notes

**1. Which step(s) represent Gather?**

The pre commit hook running git diff --cached and checking staged files represents Gather. It collects the raw facts about what is about to be committed, the file contents, the file sizes, and whether any pattern matches a secret, without making any judgment calls yet.

---

**2. Which step(s) represent Analyze?**

The /pr-ready skill represents Analyze. It reads the same staged changes and interprets them, looking for things a fixed rule cannot detect on its own, like a debug statement, a mixed concern, or a change with no clear explanation. It then produces a risk report and a draft PR title and description based on that interpretation.

---

**3. Which step is Human Act, and why must a human — not Claude — run `git commit`, `git push`, and open the PR?**

Human Act is me actually running git commit, git push, and opening the Pull Request myself. This has to be done by a human because these are the steps that permanently change the shared repository. The AI can gather facts and give advice, but it does not have the judgment, accountability, or context to decide when a change is truly ready to go out. If Claude were allowed to commit or push on its own, a wrong or incomplete AI judgment could reach the codebase with no human checkpoint in between. Keeping the human in charge of the action ensures someone is actually responsible for what gets merged.

---

**4. Which step is Verify?**

Verify is the second commit attempt after fixing scripts/notify.sh, along with running /pr-ready again on the clean staged changes. This confirms that the fix actually worked, the pre commit hook no longer blocks the commit, and /pr-ready reports a clean risk report instead of flagging issues.

---

**5. In one or two sentences: why do you need *both* the fixed-rule pre-commit hook and the AI skill? Isn't one enough?**

The pre commit hook is fast, consistent, and catches specific known patterns like secrets or oversized files every single time with no room for error, but it cannot understand context or meaning. The AI skill can understand context, like mixed concerns or misleading descriptions, but it cannot be trusted to reliably block anything on its own, so both are needed together, one for hard rules and one for judgment calls.

---

# Task 8 — LinkedIn Post

## Goal

Publish a LinkedIn post summarizing what you built and what you learned about combining fixed-rule safety checks with AI-assisted review.

### Evidence

#### LinkedIn Post URL

https://www.linkedin.com/posts/gideon-omole-5ba318180_git-github-agenticai-ugcPost-7486185387531411456-NBQL/?utm_source=share&utm_medium=member_desktop&rcm=ACoAACrC7l4BK-z0pGwSRQMO8ZJ5pFZyqybbIk4

---

## Key Learnings

Add 3-5 bullet points on what you learned this week.

- Fixed rule checks and AI review serve different purposes and neither one can replace the other. A pre-commit hook enforces hard, predictable rules like blocking secrets or oversized files, while an AI skill like /pr-ready understands context and can catch things a rigid pattern never will, like mixed concerns or leftover debug code.

- Tracking a hook inside the repository under hooks/ and pointing Git at it with core.hooksPath makes the safety check shareable across an entire team, instead of living only on one person's machine in .git/hooks/.

- Restricting a Claude Code skill's allowed-tools to just Bash, Read, and Grep, with no Write access, turns a safety boundary into something enforced by permissions rather than something that just relies on the AI following instructions correctly.

- AI is genuinely useful for drafting and reviewing, but it should never be the one executing irreversible actions like git commit, git push, or opening a Pull Request. A human has to stay in charge of every action that actually changes the shared codebase.

- Reading and editing an AI generated PR draft instead of copy pasting it blindly matters. I saw firsthand how an AI draft can misdescribe a change, calling a security fix a simple cleanup, which shows why a human review step before publishing is essential.

---

# Submission Instructions

- Ensure `hooks/pre-commit` and `.claude/skills/pr-ready/SKILL.md` are committed to your GitHub repository
- Add all required screenshots to your submission
- All written answers must be in your own words
- Do not use a real secret or credential anywhere in your submission — the fake key in Task 1 is intentional and must stay clearly fake
- Open your Pull Request against your own fork, not the shared upstream repository
- Push your final changes to your forked repository
- Include your PR link and LinkedIn post URL

---

## GitHub Repository URL

Paste your forked repository URL here:

`https://github.com/Gideon-Omole/devops-micro-internship-pravinmishra`

---

# Completion Checklist

- [ ] Branch `feature/ai-pr-ready` created with a staged file containing a fake secret and a debug statement
- [ ] `hooks/pre-commit` created and tracked in the repo (not only in `.git/hooks/`)
- [ ] `core.hooksPath` configured to point at `hooks/`
- [ ] Pre-commit hook shown blocking the risky commit
- [ ] `.claude/skills/pr-ready/SKILL.md` created with correct `allowed-tools` (no `Write`) and `disable-model-invocation: true`
- [ ] `/pr-ready` run against the risky diff and shown flagging issues
- [ ] Risky file fixed; `git commit` succeeds cleanly
- [ ] `/pr-ready` re-run showing a clean report and drafted PR title/description
- [ ] Pull Request opened using the AI draft as a starting point, with your own fork as the base repository (not upstream), PR link included
- [ ] Agentic Loop mapping (Task 7) completed in your own words
- [ ] LinkedIn post published and URL submitted
- [ ] All required screenshots added
- [ ] GitHub repository URL provided

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://pravinmishra.com/dmi  
- 🎓 DevOps for Beginners (Udemy): https://www.udemy.com/course/devops-for-beginners-docker-k8s-cloud-cicd-4-projects/  
- 🎓 Agentic AI DevOps with Claude Code: https://www.udemy.com/course/ultimate-agentic-ai-devops-with-claude-code/  
- 🎓 DevOps with Claude Code: Terraform, EKS, ArgoCD & Helm: https://www.udemy.com/course/devops-with-claude-code-terraform-eks-argocd-helm/  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
