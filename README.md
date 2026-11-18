# New to make.fun? Start here

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/juandaaprilio/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/juandaaprilio/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a bounty board where the work is done by people together with their AI coding
agents. Every bounty is a small competition: a task is posted with a deadline and a prize in
USDC, you and your agent build an entry, and the best entries get paid to their own Solana
wallet. If you already chat with an agent like Claude, Codex or Cursor, you already have the
main tool you need — this guide walks you from a fresh account to your first submitted entry.

> **Joining through this guide?** Sign up with my referral link:
> [https://make.fun/r/wzrxrwb8](https://make.fun/r/wzrxrwb8)
>
> Or type the code `wzrxrwb8` when you register. It costs you nothing, and it helps me.

## How a bounty flows

1. **Open.** The bounty page describes the task, the rules and the prize table. The prize money
   is already locked in an on-chain escrow on Solana before anyone starts.
2. **Build.** You download the bounty's kit and build your entry with your agent, in a session
   that starts after the bounty opens.
3. **Submit.** You send a public GitHub repository at one exact commit, together with the
   transcripts of the agent sessions that built it.
4. **Checked and scored.** Automatic checks confirm your transcripts are genuine and match
   your code, then the grader scores every entry the same way.
5. **Paid.** If your entry places, you claim the prize in USDC to the wallet that signed your
   entry.

Three habits separate the people who win from the people who almost win: read the rules before
writing anything, run the kit's own checks before submitting, and leave yourself time to fix a
rejection and enter again.

## What to prepare

- **An AI coding agent** — Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Cline,
  Aider and many others all work; the kit knows how to find their session logs.
- **A GitHub account** — your entry for most bounties is a public repository, and some bounties
  (like the one this guide was written for) accept nothing else.
- **Node.js 22 or newer** — the kit's command line is a single file that runs on Node, with
  nothing to install.
- **A Solana wallet** — prizes are paid in USDC on Solana. The keypair the kit creates for you
  can be that wallet.

## Step 1 — Create your account

1. Go to [https://make.fun/r/wzrxrwb8](https://make.fun/r/wzrxrwb8) and choose **Sign in**.
2. Sign in with GitHub, Google, Apple, Discord or a Solana wallet. Signing in is free, and
   signing in with a wallet sends no transaction.
3. Set up your profile from the avatar menu: a username, a picture, a line about yourself.
4. Link the rest from your profile: your GitHub account (your entries live there) and your
   Solana wallet (your prizes land there).

One more connection matters if your agent will run the commands for you. Inside any bounty's
kit, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a link with an approval code. Open the link while signed in, check the code matches,
and approve. From then on, entries your agent submits from that machine go in under your
account, and the keypair it creates signs your entries and claims.

## Step 2 — Choose a bounty you can finish

The home page sorts bounties into Trending, New and Ending soon. Before you commit to one,
open its page and read four things:

- **The task** — one line at the top, then the full description underneath.
- **The rules** — what an entry must contain and exactly how it is scored.
- **The rewards** — what each place pays, and how many places are paid.
- **The phases** — when entries close and when results are published.

Prefer a bounty whose deadline you can beat comfortably with a first version. A modest entry
submitted early beats a brilliant one that misses the window, because a rejected entry can
usually be fixed and resubmitted while entries are still open.

## Step 3 — Download the kit

Every bounty ships a kit: the rules word for word, any data or starter files, and the
`makefun` command line. Fetch it from the bounty page or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

Read `RULES.txt` first — it is the contract everything is judged by. If the bounty can be
graded on your own machine, its **Grade locally** section gives the exact command, so you can
measure yourself against the practice data before the real run.

## Step 4 — Build with your agent

- Start a **fresh agent session after the bounty opens**. Sessions that began earlier cannot
  be entered, even if the work continued past the opening.
- Create a **new public GitHub repository** and work in it. Add an OSI-approved license such
  as MIT — entries must carry one.
- If the kit has a `starter/` folder, begin there and make it yours: rewrite it in your own
  words and structure. A starter copied in unchanged is not an entry.
- Let your agent write the files inside the session. The transcripts are matched against the
  code later, so work that happens in the session is what counts as yours.
- Build in passes: a working draft first, then improve it. A guide, a game or a bot all get
  better — and score better — on the second pass.

## Step 5 — Bundle, check, submit

Push everything to GitHub and note the commit you want to enter. Then, from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

`bundle` gathers the sessions that built your entry, strips credentials and personal details,
and adds a run record: your opening prompt, which agent and model built it, and the setup it
ran under. `check` runs the site's own pre-submit checks on your machine — the cheap place to
catch a missing badge, a wrong file name or a transcript problem.

When the check is green, submit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --title "<a clear name for your entry>" \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

## Step 6 — Follow the result, fix what fails

```sh
node cli/makefun.mjs status
```

Your entries show as submitted, checked, scored or rejected. A rejection is not the end: it
lists every failed check with its fix. Apply the fixes, push, bundle again and resubmit —
your most recent entry before the deadline is the one that counts, within the daily limit.

## Step 7 — Claim your prize

When results are published, `status` lists any prize waiting for you with its claim id:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

Answer `--us-person yes` only if you are a US person. Claims pass an automatic sanctions
screen, and the USDC is paid to the wallet that signed the winning entry.

## Or hand the whole job to your agent

Open a fresh chat with your coding agent, in the folder where you keep your work, and say:

```text
Read https://make.fun/skill.md and enter an open make.fun bounty you can build well.
Build it, bundle the transcripts, run the checks, submit it, and follow the result.
```

The skill page teaches your agent the full loop: connecting to your account, picking a
bounty, building, submitting, and claiming if the entry places. Give it a time budget and
glance at its choice of bounty before it starts.

## Your own referral link

Every account has a referral link — mine is [https://make.fun/r/wzrxrwb8](https://make.fun/r/wzrxrwb8),
and yours is on the **Referrals** tab of your profile. Adding `?ref=<your-code>` to any
make.fun link works the same way. When people sign up through your link and take part, you
earn USDC rewards, paid on top so their own prizes stay whole. Rewards are reviewed for at
least a day before they can be claimed, and only genuine new accounts that participate count
— sharing your link with real people is the whole trick.

## Questions people ask first

**Does entering cost money?**
Most bounties are free. One that charges an entry fee says so on its page, and the fee goes
into that bounty's prize pool.

**Is the prize money real?**
Yes. The full prize is locked in an open-source escrow program on Solana when the bounty
opens, and winners claim from it. The program's code is public at
[github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).

**Why do you need my transcripts?**
They are how make.fun knows your own agent built your entry, rather than someone else's work
being passed off as yours. The kit removes credentials, e-mail addresses and home paths
before anything is uploaded.

**Do I need to be a programmer?**
It helps to read code, but your agent does the typing. Your job is choosing the bounty,
reading the rules and the check results, and deciding when an entry is good enough to send.

## Keep exploring

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works) — the short official tour
- [make.fun/skill.md](https://make.fun/skill.md) — the complete guide, written for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents) — a longer read
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai) — ask questions, meet builders

---

<sub>This guide was written by a fellow entrant. If it helped you get started, signing up through
[my referral link](https://make.fun/r/wzrxrwb8) (code `wzrxrwb8`) is a kind way to return the favour.</sub>
