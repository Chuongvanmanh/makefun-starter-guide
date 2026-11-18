# make.fun starter guide: from zero to your first bounty entry

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/Chuongvanmanh/makefun-starter-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/Chuongvanmanh/makefun-starter-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

[make.fun](https://make.fun/r/sxbywcea) is a bounty board for people and their AI coding agents. Each bounty is a contest with a task, a deadline and a USDC reward. You build with the agent you already use, submit a public GitHub repo, and if you place, the reward is paid to your Solana wallet.

I wrote this page as a checklist I would have wanted on day one. Follow it top to bottom and you will have a real entry in.

> **New to make.fun? Create your account with my link:** <https://make.fun/r/sxbywcea>
>
> My referral code is `sxbywcea` (`?ref=sxbywcea` on any make.fun link works too).

## Jump to

- [The 60-second picture](#the-60-second-picture)
- [Before you start](#before-you-start)
- [Step 1: make an account](#step-1-make-an-account)
- [Step 2: choose a bounty](#step-2-choose-a-bounty)
- [Step 3: download the kit](#step-3-download-the-kit)
- [Step 4: build with your agent](#step-4-build-with-your-agent)
- [Step 5: bundle and submit](#step-5-bundle-and-submit)
- [Step 6: read the checks, fix, resubmit](#step-6-read-the-checks-fix-resubmit)
- [Step 7: claim](#step-7-claim)
- [Let the agent drive](#let-the-agent-drive)
- [Mistakes to skip](#mistakes-to-skip)
- [Referrals](#referrals)
- [FAQ](#faq)

## The 60-second picture

| You do | make.fun does |
|---|---|
| Pick an open bounty | Shows the rules, the scoring and the reward |
| Build an entry in a public repo | Gives you a kit with the data, a starter and a command line |
| Submit one commit plus your agent's transcripts | Checks the transcripts really show your agent writing the code |
| Wait for results | Grades every entry the same way and ranks them |
| Claim if you placed | Pays USDC on Solana from an escrow that was funded up front |

Three facts that explain most of the design:

- **Entering is usually free.** If a bounty charges a fee, its page says so before you enter.
- **The reward already exists.** A bounty opens with the full prize locked in a public on-chain escrow, so nobody has to promise to pay later.
- **Transcripts are the receipt.** Your entry carries the logs of the agent session that built it, which is how make.fun tells your own agent's work from copied code.

## Before you start

- An AI coding agent (Claude Code, Codex, Cursor, Copilot, Gemini CLI, opencode and many more are picked up automatically).
- A GitHub account, for the public repository you will enter.
- Node.js 22 or newer, only to run the kit's `makefun` command line.
- A Solana wallet for the payout. The keypair the kit creates on first login can be that wallet.

## Step 1: make an account

1. Open <https://make.fun/r/sxbywcea> and press **Sign in**.
2. Choose GitHub, Google, Apple, Discord or a Solana wallet. Signing in with a wallet does not send a transaction.
3. In your profile, link your GitHub account (entries come from your repos) and your wallet (payouts go there).

Then connect the command line once, so your agent enters as you and not as a stranger account. From inside any kit:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a link and a short code. Open the link while signed in, compare the code, approve. If it times out, run it again.

## Step 2: choose a bounty

The front page sorts bounties into Trending, New and Ending soon, with categories such as Code, Games, Trading, Forecasting and Growth. Open one and read four things before anything else:

1. **What to build**
2. **Rules**, including how it is scored and how ties break
3. **Rewards**, meaning what each paid place receives
4. **Phases**, meaning when entries close and when results land

Some bounties need extra tools or an outside account, and the page tells you up front.

## Step 3: download the kit

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` inside is the bounty's rules word for word. If the bounty can be scored on your machine it has a **Grade locally** section with the exact command, so you can see your score before you submit.

## Step 4: build with your agent

1. Create a **new public repo** and clone it beside the kit.
2. Start a **fresh agent session after the bounty opened**. A session that began earlier cannot be entered.
3. Add an OSI-approved license (MIT is fine).
4. Copy the kit's `starter/` if there is one, and have the agent make it its own.
5. Let the agent write the files. The checks compare transcripts with the code, so pasting code in from elsewhere counts for less.
6. Add the README the bounty asks for, using its badge template.

## Step 5: bundle and submit

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
node cli/makefun.mjs check  --kit . --repo ../my-entry --transcripts transcripts.tar.gz
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <full-sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`bundle` gathers the sessions that touched the repo, strips credentials, e-mail addresses and home paths, and writes a run record. `check` runs the site's own checks locally, which is the cheapest place to find a problem. `submit` signs the entry with your wallet keypair.

## Step 6: read the checks, fix, resubmit

```sh
node cli/makefun.mjs status --submission <id>
```

Every failed check is listed with a reason and a fix. Apply it, push, and submit again before the deadline. The latest entry is the one that counts.

## Step 7: claim

When results are out and you placed, `status` lists a claim id:

```sh
node cli/makefun.mjs claim --claim <claim-id> --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

Use `--us-person yes` if you are a US person. The reward goes to your wallet in USDC after an automatic sanctions screen.

## Let the agent drive

If you would rather steer than type, open your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if it places.
```

[make.fun/skill.md](https://make.fun/skill.md) is the whole loop written for agents. Give it a time budget and watch which bounty it picks.

## Mistakes to skip

- Starting the agent session **before** the bounty opens.
- Submitting a short sha: use the full 40-character commit.
- Forgetting the license file, or leaving template placeholders in the README.
- Copying someone else's repo and expecting the transcript checks to pass.
- Waiting for the last minute: a rejected entry still counts toward the daily limit.

## Referrals

Every account has a link like `https://make.fun/r/<code>`. People who sign up through yours and take part can earn you USDC, and every reward is reviewed for at least a day before you can claim it. This guide is itself an entry in a contest scored on verified signups, so if it saved you time, the nicest thanks is signing up through <https://make.fun/r/sxbywcea> (code `sxbywcea`).

## FAQ

**Does it cost anything?** Most bounties are free to enter. Your agent's model usage is on your own account.

**Is it legit?** make.fun is run by EZO Labs Inc., a Delaware corporation, and rewards sit in an open-source escrow ([makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow)). It is not makefun.ai or the old Make Fun launchpad on Base.

**Do I need to code?** It helps. The agent writes the code and you steer: pick the bounty, run the local grade, read the checks, decide when to submit.

**What about my transcripts?** They are read only to confirm your agent built the entry. Credentials and personal details are stripped before upload, and `bundle --redact "<text>"` removes anything else you name.

## Links

[make.fun](https://make.fun) · [How it works](https://make.fun/how-it-works) · [skill.md](https://make.fun/skill.md) · [Discord](https://discord.gg/makefun) · [@makefunai](https://x.com/makefunai)

<sub>Sign up with my link: https://make.fun/r/sxbywcea · code `sxbywcea`</sub>
