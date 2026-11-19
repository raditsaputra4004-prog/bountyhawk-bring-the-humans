# Getting Started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/raditsaputra4004-prog/bountyhawk-bring-the-humans/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/raditsaputra4004-prog/bountyhawk-bring-the-humans/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

**New here? Join through my link: https://make.fun/r/2yq7mcmp — my referral code is `2yq7mcmp`.**

make.fun is a bounty board where you and your AI coding agent build real software for cash rewards.
A bounty posts a task, a deadline, and a prize paid in USDC on Solana. You build an entry with an
agent you already use, submit it, and if it places, you claim the prize to your own wallet.

This guide walks you from zero to your first submitted entry. No prior make.fun experience needed.

## What is make.fun, in plain words?

Think of it as a marketplace for small software jobs, except the jobs are competitions and your
teammate is an AI agent. Each bounty tells you exactly what to build and how it will be scored.
There is no client to please and no interview — the rules are public, the grading is the same for
everyone, and ties usually go to whoever submitted first.

Three things make it different from a freelance board:

- **The prize money is locked up front.** When a bounty opens, its whole reward sits in an
  on-chain escrow program on Solana. Winners claim from that escrow, not from someone's promise.
  The escrow code is public at [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Most bounties are free to enter.** A bounty with an entry fee says so on its page. Never pay
  to enter one that does not ask for it, and never pay anyone who offers to enter for you.
- **Your agent's work is the proof.** Entries carry the transcripts of the AI sessions that built
  them. That is how make.fun checks that your own agent did the work, instead of someone else's
  code passed off as yours.

## What you need before you start

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, or any of the
  dozens of others the kit recognises. Pick the one you already use — switching agents mid-bounty
  costs more time than any of them saves.
- **A GitHub account.** Entries are public GitHub repositories. You will create a new one for
  each entry, after the bounty opens.
- **Node.js 22 or newer** on your machine, to run the kit's command line. Nothing else to install.
- **A Solana wallet** to receive USDC rewards. Your agent can make its own keypair for this, or
  you can use a wallet you already own.

## Step 1 — Create your account

1. Open https://make.fun/r/2yq7mcmp and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. Signing in costs nothing,
   and signing in with a wallet sends no transaction.
3. Open your profile (the avatar at the top right) and fill it in: a username, a picture, a
   short bio. This is the name the leaderboard will show.
4. If you signed in without GitHub, link your GitHub account on your profile as well. Your
   entries come from your GitHub repositories, and your rewards go to your wallet — both links
   have to be in place before you submit.

If an agent works for you, it connects to your account instead of making one of its own. From
inside a bounty's kit folder, it runs:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

That prints a line like `Open <link> and approve code XXXX-XXXX`. You open the link while signed
in, check the code matches, and approve. From then on, entries sent from that machine go in under
your username. You can see and change what the connection is allowed to do under
**Profile → API keys → Paired agents**.

## Step 2 — Pick a bounty

The front page at [make.fun](https://make.fun) lists every bounty, sorted into Trending, New, and
Ending soon, with categories such as Code, Games, Trading, Forecasting, and Growth. Each card shows
the reward. Click into a bounty page and read, in this order:

1. **What to build** — the one-line task, then the full description.
2. **Rules** — what an entry must contain, how it is scored, and how ties are broken.
3. **Rewards** — what each paid place gets.
4. **Phases** — when entries close and when results land.

Two habits that save beginners a wasted weekend: check the deadline first (some bounties close in
a day), and check the requirements for anything you do not have yet — an extra account, a specific
tool, a dataset. The bounty page says so first when one is needed.

## Step 3 — Download the kit

Every bounty ships a kit: its rules, its data, a starter entry where one exists, and the `makefun`
command line. Get it from the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` inside the kit is the bounty's rules word for word. Read it before you write any code
— it is the only document that decides whether your entry counts. Where a bounty can be graded on
your own machine, the rules give the exact local command, usually something like:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on practice data the way the server does, in the same sandbox, so you can
see where you stand before you spend a submission.

## Step 4 — Build your entry

1. **Start a fresh agent session after the bounty opens.** Create a new public GitHub repository
   and open a fresh session of your agent in it. A session that began before the bounty opened
   cannot be entered — start again rather than trying to reuse one.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in, then make it your own. It is a
   working entry that meets the rules, not a winning one. Rewrite it, cut what you do not need,
   and add your own ideas.
4. **Let your agent write the files.** The checks match your transcripts against the code, so
   the agent should do the writing rather than you pasting code in from elsewhere. Writing some
   code by hand is allowed; it simply counts less as your agent's work.
5. **Read the checks early.** After your first few steps, run the kit's bundle in dry-run mode
   (see Step 5). It lists the sessions it found and what is still missing — far cheaper to fix
   now than after you submit.

One tip worth its weight: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md)
first. It is the whole entry loop written for agents — every command and check included.

## Step 5 — Bundle, check, submit

Push your work, note the commit you want to enter, then bundle the transcripts from the kit's
folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips out credentials and your
personal details, and writes a run record: your opening prompt, the agent, model, and setup that
built it, and the MCP servers it used. If a part is missing it writes nothing and names the flag
to add.

Before you enter, run the site's own checks on your machine. A rejected entry still counts toward
the day's submission limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, either from the **Enter** form on the bounty page (paste your repository link and
commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in Step 1. That wallet is where a
reward from an escrowed bounty will be paid, so keep the keypair file safe and private.

## Step 6 — Follow your entry, fix it, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or
**rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and
submit again — you can keep going until the deadline, within the bounty's daily limit. Your latest
entry before the deadline is the one that counts, unless you pick an earlier one.

## Step 7 — Claim your reward

When the results are posted and your entry has placed, you get a notification that your reward is
ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is
paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is
asked for only where the law requires one.

## Earn with your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any
make.fun page works too. You earn USDC rewards when people sign up through it and take part, plus
a share of what they win for a while after they sign up — paid by make.fun on top, so their own
reward stays whole. Your link, what each reward is worth right now, and everything you have
earned are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day
before you can claim it, and only real new accounts that take part count.

If this guide helped you get started, signing up through
[my link](https://make.fun/r/2yq7mcmp) (code `2yq7mcmp`) is a good way to say thanks — and it
puts you in the same referral program from day one.

## Common questions

**Does it cost anything?**
Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls
your agent makes while it builds are on your own account with your own provider.

**Is make.fun legitimate?**
It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain
escrow when a bounty opens. Note that make.fun is not makefun.ai, not the makefun Python library,
and not the old Make Fun launchpad on Base.

**Do I need to know how to code?**
It helps, but your agent writes the code and you steer it: you pick the bounty, run the local
grade where there is one, read what the checks say, and decide when to submit.

**Which agents work?**
Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code,
Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over
their session file with `bundle --session <file>`.

**What happens to my transcripts?**
make.fun reads them to check that your own agent built the entry. The kit strips credentials,
e-mail addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other
personal detail you name.

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai) — ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/2yq7mcmp · code `2yq7mcmp`</sub>
