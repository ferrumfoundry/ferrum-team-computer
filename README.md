# Ferrum Team Computer

A computer for the team that lives in the cloud — and costs nothing.

Press a button and GitHub boots a fresh PC for you: a **Linux** one, a
**Windows** one, or **both at the same time**. Each one has a real
desktop screen you can open on your phone and drive with your fingers.
There are **no passwords anywhere** — not for you, not for the bots —
and **no credit card is asked for at any point**.

## Setup (once)

1. Create a **public** repository on your GitHub account. (It has to be
   public — that's what makes the computers free.)
2. Upload the files from this folder into it, keeping the folders as
   they are (`.github/workflows/...`).
3. That's it. There is nothing else to set up — no keys, no secrets,
   no settings to fill in.

## Run a computer

1. Open your repo on GitHub and tap the **Actions** tab.
2. Pick **Team Computer — Linux** or **Team Computer — Windows**.
3. Tap **Run workflow**, then **Run workflow** again to confirm.
4. Wait a few minutes while it boots. Open the running job and look
   for the big banner — it shows an address like `bore.pub:12345`.
   (The address is also on the run's summary page.)

Want both machines? Run both workflows. They don't interfere with
each other.

## Connect to the screen

**Linux** — install a free VNC app on your phone (RealVNC Viewer or
bVNC both work). Add a connection with the `bore.pub:PORT` address
from the banner and connect. No password — just connect. You'll see
the desktop already logged in as the user **ferrum**.

**Windows** — install the free **Microsoft Remote Desktop** app. Add
a PC with the `bore.pub:PORT` address from the banner. User name:
**ferrum**. Password: **leave it empty** — there isn't one.

## The honest limits

- **A machine lives up to about 6 hours**, then GitHub shuts it down
  and it is gone. Run the workflow again whenever you want a fresh one.
- **Its disk is wiped when it ends.** Anything you want to keep has to
  be saved into the repo or downloaded before time runs out.
- While a machine is up, its address shows in the public run log and
  **there is no password on the door** — anyone who finds the address
  can walk in, so treat these as throwaway machines and never put
  anything precious on them.
- **No graphics card.** Fine for desktops, tools, and the team's work;
  not for heavy 3D or games.
- GitHub gives these runners free on public repos so people can build
  and test software. Using them as all-day everyday computers is
  outside that spirit, and heavy abuse can get a repo flagged — one
  more reason to keep nothing on them you can't afford to lose.

## What's next

These are just the computers — two empty, ready machines. The next
layer installs the team's brains onto the Linux machine: Echo's
gateway and the local model the bots think with, so the team can
actually move in and start working.
