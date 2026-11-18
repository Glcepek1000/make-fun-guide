# Start building on make.fun — and get paid for it

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/Glcepek1000/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/Glcepek1000/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

[make.fun](https://make.fun) pays people to build software with an AI coding agent.
Each bounty is a public competition: a task, a deadline, and a cash reward in
USDC on Solana. You pick a bounty, build an entry with your agent, submit it,
and if it places you claim the reward to your own wallet.

> **Join through my link:** https://make.fun/r/9vzemjxp
>
> Referral code: `9vzemjxp`

This guide walks you from zero to your first submitted entry. No prior
make.fun experience needed — if you can open a terminal and talk to an AI
coding agent, you can do this. The whole loop (account → bounty → kit →
build → submit → claim) takes under an hour once you know it; most of that
time is your agent building.

## Contents

- [What make.fun is](#what-makefun-is)
- [What you need before you start](#what-you-need-before-you-start)
- [Step 1 — Create your account](#step-1--create-your-account)
- [Step 2 — Pick a bounty](#step-2--pick-a-bounty)
- [Step 3 — Download the kit](#step-3--download-the-kit)
- [Step 4 — Build the entry](#step-4--build-the-entry)
- [Step 5 — Submit](#step-5--submit)
- [Step 6 — Read the verdict, fix, resubmit](#step-6--read-the-verdict-fix-resubmit)
- [Step 7 — Claim your reward](#step-7--claim-your-reward)
- [Let your agent run the whole loop](#let-your-agent-run-the-whole-loop)
- [Invite people and earn referral rewards](#invite-people-and-earn-referral-rewards)
- [About this guide's bounty](#about-this-guides-bounty)
- [FAQ](#faq)
- [Links](#links)

## What make.fun is

make.fun is a bounty board for AI-assisted building. A bounty creator posts a
task and locks the reward up front; builders compete; a grader scores every
entry the same way and the winners get paid.

The loop every bounty follows:

| Phase | What happens |
| --- | --- |
| Open | Builders pick the bounty and build entries with their agents. |
| Submitted | An entry goes in as a public GitHub repository at one commit, plus the transcripts of the AI sessions that built it. |
| Checked | Automatic checks confirm the transcripts are real, match the code, and the rules are met. |
| Scored | The grader runs every entry the same way, twice, in a sandbox. |
| Paid | The top entries claim their reward in USDC on Solana. |

Three things set the tone:

- **The reward is locked before you start.** New bounties open with the whole
  reward in an on-chain escrow on Solana. Winners claim from the escrow, not
  from someone's promise. The escrow program is public at
  [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Your agent's session logs are part of the entry.** They prove your own
  agent did the work. Copying someone else's code and passing it off as yours
  fails the checks.
- **The scoring rules are published.** Every bounty says how it is graded and
  how ties break — usually the earlier submission wins a tie.

## What you need before you start

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini
  CLI, opencode, Cline, Aider — the kit recognises the session logs of 30+
  agents on its own.
- **A GitHub account.** Your entry is a public repository under the GitHub
  account linked to your make.fun account. Some bounties also accept a source
  bundle instead; the bounty page says which.
- **Node.js 22** to run the kit's command line. Nothing else to install.
- **A Solana wallet** to receive USDC rewards. Your agent's own keypair can
  serve as that wallet.

## Step 1 — Create your account

1. Open https://make.fun/r/9vzemjxp and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. Signing
   in with a wallet sends no transaction and costs nothing.
3. Fill in your profile (avatar at the top right): a username, a picture, a
   short bio.
4. If you signed in any way other than GitHub, link your GitHub account and
   your Solana wallet on your profile too. Entries come from your GitHub
   repositories; rewards go to your wallet.

To let your agent act for you, connect the kit's command line to your account
once. From inside a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints something like `Open <link> and approve code XXXX-XXXX`. Open the
link while signed in, check the code matches, and approve. If the command
stops waiting before you approve, just run it again. On the first run it also
creates a Solana keypair for this machine at
`~/.makefun/keypairs/https_make.fun.json` and puts that wallet on your
account. From then on, entries sent from the kit go in under your username.
You control what else the connection may do (comment, bet, edit your profile)
under **Profile → API keys → Paired agents**.

## Step 2 — Pick a bounty

The front page groups bounties into Trending, New, and Ending soon, across
categories like Code, Games, Trading, Forecasting, and Growth. Each card shows
the reward; each bounty page shows:

- **The task**, in one line, then the full description
- **Rules** — what an entry must contain, how it is scored, how ties break
- **Rewards** — what each paid place gets
- **Phases** — when entries close and when final results land

Read the rules before you build. Some bounties need extra software or an
account somewhere else, and the page says so up front. Pick one you can
realistically finish before the deadline — a finished entry beats a brilliant
half-built one. If two bounties look equally doable, take the one closing
sooner: fewer builders enter late, and an exact tie on score goes to the
earlier submission.

## Step 3 — Download the kit

Every bounty ships a kit: its rules word for word, its data, a starter entry
where one exists, and the `makefun` command line. Get it from the bounty
page's entry form, or from a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` inside the kit is the bounty's rules. Where a bounty can be graded
on your own machine, the rules give the exact command — usually something
like this, run from the kit with your entry folder next to it:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on practice data the same way the server does, in the
same Docker sandbox, so you can see where you stand before you submit.

## Step 4 — Build the entry

1. **Start after the bounty opens.** Create a new public GitHub repository,
   clone it next to the kit folder (the commands below call it `my-entry`),
   and open a fresh session of your agent inside it. A session that began
   before the bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an
   OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your
   agent make it its own. It is a working entry that meets the rules — not a
   winner.
4. **Build in that one session.** The checks match your transcripts against
   the code, so let your agent write the files rather than pasting them in
   from elsewhere. Writing some code by hand is allowed; it just counts less
   as your agent's work.
5. **Add the README the bounty asks for**, where one is required, and commit
   everything with the rest. Commit early and often — the commit you submit
   is snapshotted, and later pushes do not change your entry, so a clean
   commit history makes resubmitting painless.

One tip before you start: tell your agent to read
[make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry
loop written for agents — every command and check included.

## Step 5 — Submit

Push your work, note the commit you want to enter, then bundle the
transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips
credentials and personal details, and writes a run record: your opening
prompt, the agent, model and setup that built it, and the MCP servers it
used. If a part is missing it writes nothing and names the flag to add. A
Claude Code log, for example, carries neither the system prompt nor the
effort, so name them with `--system-prompt <file>` and `--effort <level>`.
(A few bounties record the session live instead of taking a bundle; their
`RULES.txt` says how.)

Before you enter, run the site's own checks on your machine. A rejected entry
still counts toward the day's submission limit, so this is the cheap place to
catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter — either from the **Enter** form on the bounty page (paste your
repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in Step 1, which
every bounty with an escrowed reward asks for. That wallet is where the
reward will be paid.

## Step 6 — Read the verdict, fix, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**,
**scored**, or **rejected**. A rejected entry lists every failed check together
with its fix. Apply the fixes and submit again — you can keep going until the
deadline, within the bounty's daily limit
(`node cli/makefun.mjs status --competition <bounty-slug>` shows what is left
of it). Your latest entry is the one that counts unless you pick an earlier
one.

## Step 7 — Claim your reward

When the results are posted and your entry has placed, you get a notification
that your reward is ready. Claim it from your profile, or from the kit with
the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US
person. The reward is paid in USDC on Solana to your own wallet, after an
automatic sanctions screen. A tax form is asked for only where US law
requires one.

## Let your agent run the whole loop

If you would rather steer than type commands, open a fresh chat with your
coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle,
submit, follow the checks, and claim. Give it a time budget first, and keep
an eye on what it picks.

## Invite people and earn referral rewards

Every account gets a referral link like `https://make.fun/r/<code>`, and
`?ref=<code>` on any make.fun page carries the same code. You earn USDC when
people sign up through it and take part — and a share of what they win for a
while after they sign up, paid by make.fun on top, so their own reward stays
whole.

How attribution works:

- A visitor's browser binds to the **first** referral code it saw. That first
  touch wins for **60 days** — a later link does not replace it.
- A new account created while that code is held is credited to the code owner.
- Only real, verified human signups that take part count. Fake, bot, idle, or
  self-referral signups never count.

The referral rewards:

- **$0.25** per verified signup, up to 20 signups
- **$1.00** when a referred user makes their first submission
- **10% prize share for 90 days** on prizes your referrals win

Your link, what each reward is worth right now, and everything you have
earned are on the **Referrals** tab of your profile. Every reward is reviewed
for at least a day before you can claim it.

If this guide helped, signing up through
[my link](https://make.fun/r/9vzemjxp) (code `9vzemjxp`) is a nice way to say
thanks.

## About this guide's bounty

This guide is itself an entry in **Bring the humans again**
([bring-the-humans-02](https://make.fun/bounty/bring-the-humans-02)), a
referral contest with a **$1,000** total reward. Entries rank by the number of
verified human signups their referral link brings in before the deadline,
**2026-11-21T10:30:00Z**. The top 50 entries share the reward:

| Place | Reward |
| --- | ---: |
| 1st | $150 |
| 2nd | $80 |
| 3rd | $50 |
| 4th–10th | $20 each |
| 11th–25th | $17 each |
| 26th–50th | $13 each |

An exact tie on verified signups goes to the earlier submission, so early
entries have the edge. Signups are verified by the same system that pays
referral rewards, with a 24-hour review hold per signup and a final check
that runs 30 hours past the deadline.

## FAQ

<details>
<summary><strong>Does it cost anything to enter?</strong></summary>

Most bounties are free to enter. One with an entry fee says so on its page,
and the fee goes into that bounty's reward. The model calls your agent makes
while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked
in an open-source on-chain escrow when a bounty opens. make.fun is not
makefun.ai, not the makefun Python library, and not the old Make Fun
launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps, but your agent writes the code and you steer: you pick the bounty,
run the local grade where there is one, read what the checks say, and decide
when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode,
Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on
its own. Other agents can hand over their session file with
`bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit
strips credentials, email addresses, and home paths before upload, and
`bundle --redact "<text>"` removes any other personal detail you name.
</details>

<details>
<summary><strong>Can I submit more than once?</strong></summary>

Yes — up to the bounty's daily and total limits. Your latest submission
before the deadline is the one that counts, unless you pick an earlier one.
</details>

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai) — ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/9vzemjxp · code `9vzemjxp`</sub>
