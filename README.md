==============================
SLAYBLADE - REBALANCE MOD PACK
==============================

Version 2.0 | for Slayblade (Steam app 4117730), build sha256 1e94e999...12ac

A rules-and-numbers rebalance. No new art, no new content, no new systems - it
changes what the existing pieces are worth. The goal is a run where more than
one build is worth assembling, where the map has a fight you actually have to
think about, and where the last third of a run still has stakes.

Everything is a byte patch applied to your own copy of the game. Nothing is
redistributed and the installer keeps a pristine backup, so it is fully
reversible.

------------------------------------------------------------------------------

CONTENTS
========

1. Installing on Windows
2. Installing on Linux, SteamOS and the Steam Deck
3. Uninstalling and checking status
4. What changed
     - Rival Battles
     - The two legal battles
     - The match clock
     - The League Finals
     - The Shop
     - Modifiers
     - Parts
5. Design notes
6. Known issues
7. Advanced: installing only part of the pack
8. Disclaimers

------------------------------------------------------------------------------

There are two downloads. Slayblade-Rebalance-v2.0.zip is for Windows;
Slayblade-Rebalance-v2.0-Linux.zip is for Linux, SteamOS and the Steam Deck.
They apply exactly the same changes - only the launcher scripts differ.

INSTALLING ON WINDOWS
=====================

You need: a legitimately owned copy of Slayblade on Windows. Nothing else - no
Python, no extra runtimes. The installer uses the PowerShell that ships with
Windows.

1. Find your game folder. In Steam, right-click Slayblade -> Manage -> Browse
   local files. A folder opens containing Slayblade.exe, data.win and
   language.csv. That is the folder you want.

2. Extract this zip straight into that folder. When you're done, the patcher
   and the .bat files should be sitting next to Slayblade.exe, not in a
   subfolder. If you see a Slayblade-Rebalance folder inside your game folder,
   move its contents up one level.

3. Double-click Install (Windows).bat.

4. Say yes to the Administrator prompt. Steam installs under Program Files,
   which Windows protects; the installer needs those rights to write the
   patched files. If Windows SmartScreen warns you about the .bat, choose More
   info -> Run anyway - or skip the .bat entirely and see Advanced.

5. Read the output. You should see a list of patch groups and, at the end,
   Done: 106 byte patch(es) applied. Press a key to close.

6. Launch the game. That's it.

The first install copies Slayblade.exe -> Slayblade.exe.orig and language.csv
-> language.csv.orig. Do not delete those two files - they are your way back
to vanilla, and every re-install rebuilds from them, so patches can never
stack or half-apply.

  | If Steam updates the game, the patched files are replaced and you are back
  | to vanilla. Just run the installer again. If the update changed the game
  | itself, the patcher will say the build differs from the one it targets and
  | skip anything that doesn't match rather than corrupting your install - in
  | that case, wait for an updated version of this pack.

  | Verifying game files in Steam restores vanilla too, and will leave your
  | .orig backups behind untouched. Run the installer again afterwards.

INSTALLING ON LINUX, STEAMOS AND THE STEAM DECK
===============================================

Slayblade ships as a Windows-only build, so on Linux you are running it
through Proton - but the files sitting on your disk are the same Windows
Slayblade.exe and language.csv the Windows installer patches, so the same byte
patches apply. Nothing here touches Proton, Wine or your prefix.

  | Read this first. These scripts have not been run on a real Steam Deck or
  | SteamOS install - the author doesn't own a Linux machine. They were
  | written and tested on Linux, including against simulated Steam layouts for
  | the default path, a second library folder, and both Steam Deck SD-card
  | mount styles, and they produce a byte-identical result to the Windows
  | installer. But "tested on Linux" is not "tested on your Linux". Treat this
  | as unverified, keep the .orig backups, and please report anything that
  | goes wrong.

You need: Python 3. SteamOS and the Steam Deck ship with it, as does
essentially every desktop distribution. You do not need root or sudo - Steam
keeps your games in your own home directory. (On the Deck this means you do
not need to disable the read-only filesystem or set a sudo password.)

1. Find your game folder. In Steam, right-click Slayblade -> Manage -> Browse
   local files. The usual location is
   ~/.local/share/Steam/steamapps/common/Slayblade; on a Deck with the game on
   an SD card it will be somewhere under /run/media/. The folder contains
   Slayblade.exe, data.win and language.csv.

2. Extract the Linux zip into that folder, so the scripts sit next to
   Slayblade.exe. (On a Steam Deck, do this in Desktop Mode.)

3. Make the scripts executable. Some archive tools drop the executable bit:

       chmod +x *.sh

4. Run the installer from a terminal - Konsole on the Deck:

       ./install.sh

   You should see a list of patch groups and Done: 106 byte patch(es) applied.

5. Launch the game. Nothing about how you launch it changes.

If you would rather not put the files in the game folder, run the installer
from anywhere - it looks in the standard Steam locations, reads your
libraryfolders.vdf for second drives, and checks Steam Deck SD-card mounts.
Failing all that, ./install.sh --dir "/path/to/Slayblade" always works.

UNINSTALLING AND CHECKING STATUS
================================

On Windows:

  - Uninstall (Windows).bat - restores Slayblade.exe and language.csv from the
    .orig backups and removes them. Completely clean; Steam won't know
    anything happened.
  - Check status (Windows).bat - reports which patch groups are currently
    applied. Changes nothing, needs no admin rights.

On Linux / SteamOS:

  - ./uninstall.sh - the same restore-from-backup.
  - ./check status.sh - the same status report, changes nothing.

On either platform, Steam's Verify integrity of game files also puts you back
to vanilla, and a game update will replace the patched files. In both cases
just run the installer again.

------------------------------------------------------------------------------

WHAT CHANGED
============

Rival Battles (new)
-------------------

Vanilla had two flavours of crime on the map - Criminal Battle and Illegal
Battle - and a modifier to make one of them always available. Criminal Battle
has been rebuilt into a Rival: the recurring opponent who shows up to check
whether you're actually ready.

  - He always appears. No run-number gate, no modifier required. He is on the
    map from your first run.
  - He is you. While the League is closed, his level is exactly your Player
    Level - no random roll, no drift. A true mirror match.
  - He pulls ahead as the League opens. Once the League is running he is your
    Level +10, then +15 after you take the Quarter Final, then +20 after the
    Semi. The same ladder the League bosses climb.
  - No ceiling. Vanilla capped this node at level 35. That cap is gone - he
    scales with you for as long as your run lasts.
  - The stake is everything you own. Losing to him zeroes your money. That was
    already the vanilla Criminal Battle risk; it has not been softened.
  - The payout now matches the stake. Beating him pays whichever is larger:
    your current $$$, or his level x 25. Late in a run that is a straight
    double-or-nothing. Early on, when doubling nothing would be meaningless,
    the level floor carries it. (Vanilla paid level x 3 - $60 at Level 20,
    against your whole bankroll.)
  - EXP is unchanged at level, deliberately. He is already the best money in
    the game and money converts to levels; paying EXP on top would spiral.

The two legal battles
---------------------

The two ordinary battle nodes were visually identical and mechanically almost
identical. They are now clearly different fights:

  - CASUAL BATTLE! (NO RING-OUT) - practice. Being knocked out of the Slaydium
    drains Spin instead of instantly losing you the match. It does not advance
    the time of day, so practising costs you nothing but the match. In
    exchange it pays a fifth of the money, bet and EXP, and its clock icon is
    gone from the map node.
  - PRO BATTLE! (RING-OUT) - the real thing. Instant loss on a ring-out, full
    rewards, costs a time of day.

The match clock
---------------

  - The clock stops at zero instead of ending the battle. Matches are decided
    by Spin alone - blades bleed Spin passively, so they still end. The label
    switches to OVERTIME when it hits zero.
  - The clock stays in its HUD corner. In vanilla it slides to the middle of
    the screen over the last ten seconds and then blows up to fill the arena;
    now it stays put at normal size.
  - Once OVERTIME begins, Power Up Cubes stop spawning. Cubes are what set
    your abilities off, so this is the single biggest change in the pack: past
    the buzzer, nobody's abilities fire again. Whatever is on the floor when
    the clock dies stays there and can still be taken - it's the spawning that
    stops, not the physics - but after that it's two blades and their
    remaining Spin. Overtime becomes its own phase of the fight rather than
    just more time on the same fight.
  - Lawnmowing keeps its timer - the job still expires and ends normally.
  - Power Up Cube shadow telegraph doubled, 1.00s -> 2.00s, so you can
    actually contest it while the clock is still running.

The League Finals
-----------------

  - The bosses scale with you. Quarter Final is your Level +10, Semi +15,
    Grand Final +20. Vanilla pinned them to a fixed level 15-20 band, which
    made the whole tournament trivial in a long run.
  - They hit harder each round: x1.2 / x1.35 / x1.5 Spin.
  - Entry costs real money: $100 / $200 / $300 (was $40 / $60 / $80). The
    prize for each round matches the fee. You still cannot enter a round you
    can't pay for.
  - Losing a Final is a disaster, not a setback. You are knocked back to the
    Quarter Final and your money is floored at $0.

The Shop
--------

Refreshing the shop is no longer once per day. Instead each refresh costs
twice the last one - $5, $10, $20, $40, $80... - resetting each morning.
Reroll as hard as you like; just be able to afford it. (The refresh button
also correctly re-enables and re-prices itself now, which vanilla did not
always do.)

Modifiers
---------

  - Early Bird doubles your Abilities at any time of day, not only in the
    Morning.
  - Crime Spree -> Illegal Spree. Since Rival Battle is now permanently
    available, the old modifier had nothing to do. It now makes Illegal Battle
    available at any time of day instead of Nightime-only. Same unlock cost
    and level.
  - Time Wizard's Head's slow aura lasts 7 seconds instead of 3.

Parts
-----

33 parts changed. A = Acceleration, S = Max Spin, B = Balance, W = Weight. "-"
means the part has no modifier for that stat.

Heads
-----

Part                 Now                             What changed
------------------   -----------------------------   -------------------------
Starter Head         A High | S - | B - | 5g         +High Acceleration; 2g ->
                                                     5g
Catholic Head        A Low | S Low | B Low | 5g      +Low Acceleration;
                                                     Balance High -> Low
$$$ Head             A High | S - | B High | 5g      +High Acceleration; -Low
                                                     Max Spin; Balance Low ->
                                                     High; 1g -> 5g
Twisties Head        A - | S - | B High | 5g         1g -> 5g
Time Wizard's Head   A - | S High | B High | 5g      Max Spin Low -> High; 1g
                                                     -> 5g
Acursed Head         A High | S Low | B Low | 1g     +Low Max Spin; +Low
                                                     Balance
Children's Head      A High | S Low | B Low | 10g    +Low Max Spin
The Devil's Head     A - | S High | B High | 10g     -Low Acceleration; Max
                                                     Spin Low -> High; Balance
                                                     Low -> High
Accelerator Head     A High | S High | B High | 5g   Acceleration Low -> High
Iron Head            A - | S High | B High | 5g      1g -> 5g
Time Bandit's Head   A High | S - | B Low | 10g      -Low Max Spin
Web Head             A High | S - | B High | 2g      -Low Max Spin

Bods
----

Part              Now                              What changed
---------------   ------------------------------   ---------------------------
Starter Bod       A High | S - | B - | 5g          +High Acceleration; 2g ->
                                                   5g
Satanic Bod       A - | S - | B High | 10g         -Low Max Spin; +High
                                                   Balance
Catholic Bod      A Low | S Low | B Low | 1g       +Low Acceleration; Balance
                                                   High -> Low; 5g -> 1g
$$$ Bod           A Low | S Low | B Low | 1g       Acceleration High -> Low;
                                                   +Low Max Spin
Denial Bod        A Low | S High | B High | 10g    +Low Acceleration; 1g ->
                                                   10g
Shield Bod        A Low | S High | B High | 10g    +Low Acceleration; Max Spin
                                                   Low -> High; 5g -> 10g
Ghostly Bod       A High | S High | B - | 1g       +High Acceleration
Vampire Bod       A - | S High | B High | 10g      +High Max Spin; +High
                                                   Balance; 1g -> 10g
Da Bomb Bod       A High | S High | B High | 10g   +High Max Spin; Balance Low
                                                   -> High; 1g -> 10g
Dart Lord's Bod   A High | S High | B High | 10g   Balance Low -> High
Rival Bod         A - | S - | B Low | 5g           -High Acceleration; -High
                                                   Max Spin

Tips
----

Part             Now                           What changed
--------------   ---------------------------   -------------------------------
Starter Tip      A High | S - | B - | 5g       +High Acceleration; 2g -> 5g
Satanic Tip      A - | S High | B High | 10g   Max Spin Low -> High; +High
                                               Balance
Catholic Tip     A - | S High | B High | 5g    Max Spin Low -> High; 1g -> 5g
$$$ Tip          A - | S - | B - | 1g          -Low Acceleration; -Low Balance
Just the Tip     A High | S - | B High | 5g    +High Balance
Body Check Tip   A - | S Low | B High | 5g     -High Acceleration; 1g -> 5g
Flat Earth Tip   A Low | S - | B High | 10g    +Low Acceleration; +High
                                               Balance; 1g -> 10g
Pro Tip          A Low | S - | B High | 10g    +Low Acceleration; -High Max
                                               Spin
Healing Tip      A High | S High | B - | 5g    Acceleration Low -> High; -Low
                                               Balance
Damaging Tip     A Low | S Low | B - | 10g     Acceleration High -> Low; -High
                                               Balance

Every part's ability is untouched. Only stats and weights moved.

------------------------------------------------------------------------------

DESIGN NOTES
============

Spin-decrease parts were doing too much. Stack three -10rpm parts and one
Power Up Cube leaves your opponent halfway dead. Some are worse than that -
there are decreases that scale with your Level, and decreases that scale with
your Level x2.

Rather than nerf the whole category, the weaker flat decreases were left
completely alone, on the theory that bringing everything else up is itself a
nerf to them. The scaling ones took the real hit, sized by how hard they scale
- which is why Catholic Head and Catholic Bod (Level x2 decrease) end up Low
across the board, and why Rival Bod loses both of its High stats outright.

Gimmick parts got bought back into the conversation. Parts whose effect
doesn't scale should at least be usable as a pure beatdown stick, with the
effect as the little bit extra on top. Time Wizard's Head is the model for
this: with a 7-second aura and High Max Spin it is now genuinely strong, but
fair - the aura runs out just before the next Power Up Cube spawns, so the AI
gets its shot at the cube the moment the effect drops. It can absolutely be
made broken by doubling aura times or shortening the cube timer, and that is
exactly where the build crafting lives.

Overtime is a different game, not a longer one. Parking the clock at zero
already made Spin the only thing that decides a match, which quietly promoted
stamina builds. Cutting off cube spawns at the same moment finishes the
thought: everything you have when the clock dies is everything you get. No
more waiting out the timer for one more cube to bail you out, no more coin-
flip finishes decided by who happened to be standing closer to a spawn.
Whatever advantage you built in regulation is the advantage you have to close
with.

The Rival exists to be a checkpoint. He carries the risk of losing the entire
run to one fight - statistically he will be the most likely cause of death -
and now the reward is worth the exposure. Two kinds of criminal activity never
really justified themselves; a rival does, and it's more honest to what
battle-tops stories have always been about.

------------------------------------------------------------------------------

KNOWN ISSUES
============

  - The AI ring-outs itself in Pro Battles at high level. As you level up and
    the opponent scales with you, its Acceleration outruns its own ability to
    control the blade, and it will occasionally throw itself out of the
    Slaydium. Other modes with ring-outs disabled are unaffected.

    Removing ring-outs everywhere was considered and rejected - they make for
    more engaging play, and unless you rolled a broken money build and
    levelled to absurd heights early, a Pro Battle is still a fight worth
    watching.

  - Non-English text is machine-translated. The mod rewrites a handful of
    language.csv rows (Early Bird, Time Wizard's Head, the battle node labels,
    OVERTIME, Rival Battle, Illegal Spree). The English is written; the other
    13 languages are best-effort machine translation and have not been checked
    by a native speaker.

  - The League Finals node isn't greyed out when you can't afford it. The
    node's draw code doesn't read the affordability flag, so it looks
    selectable even when the entry fee is out of reach. You simply can't
    enter.

------------------------------------------------------------------------------

ADVANCED: INSTALLING ONLY PART OF THE PACK
==========================================

Don't want all of it? Every change lives in a named group and you can install
any subset. From a PowerShell prompt in the game folder:

    .\slaymod_patcher.ps1 -Only parts,shop
    .\slaymod_patcher.ps1 -Verify
    .\slaymod_patcher.ps1 -Revert

Or, if you have Python 3 and would rather use it:

    python slaymod_patcher.py --only parts,shop
    python slaymod_patcher.py --verify
    python slaymod_patcher.py --revert
    python slaymod_patcher.py --dir "D:\Games\Slayblade"

Both produce byte-for-byte identical results. Applying always rebuilds from
the pristine backup first, so -Only leaves exactly what you asked for and
nothing else.

The groups: ringout, earlybird, cube, parts, shop, timewizard, timer,
hudclock, nodes, overtime, boss, amateur, finals, finalsfee, finalsloss,
rival, illegalspree, overtimecubes. Run the patcher with no arguments for a
one-line description of each.

------------------------------------------------------------------------------

DISCLAIMERS
===========

How this was made
-----------------

Every line of this mod - the reverse engineering, the patch authoring, the
installers and these notes - was produced through Claude Cowork (Anthropic's
Claude). Slayblade is a GameMaker YYC build, which means all of its game logic
is compiled to native machine code inside the executable; there is no script
chunk to edit and existing GameMaker modding tools cannot reach it. Every
change here is a hand-verified byte patch to compiled x64 assembly, located by
disassembling the binary. Each patch was checked by re-reading the patched
machine code back and confirming it computes what was intended.

That said: this is machine-code surgery on a program that was never designed
to be modified. It is offered as-is.

Ownership
---------

Slayblade is the property of its creators - Henry's House / Oscar Brittain.
This mod is an unofficial, unaffiliated fan project. It is not endorsed by,
associated with, or supported by the developer or publisher, and any bug you
encounter while it is installed is this mod's problem, not theirs. Please do
not report modded behaviour to the developer. Uninstall first, reproduce, then
report.

All game names, characters, artwork, audio and code remain the property of
their respective owners. Any other trademarks mentioned belong to their owners
and are used for identification only.

What is and isn't in this download
----------------------------------

No game assets are included. There is no copy of Slayblade here - no
executable, no data.win, no art, audio or text from the game. Nothing in this
download will do anything at all unless you already own and have installed the
game.

The patcher does contain the short byte sequences it overwrites, because it
verifies every site against the bytes it expects before touching them; that is
what makes it safe to run and refuse cleanly on a build it doesn't recognise.
Those excerpts exist to identify locations, not to reproduce the work.

Do not redistribute a patched Slayblade.exe or language.csv, or the .orig
backups. Share the patcher, not the game. Distributing the patched binaries
would be distributing the game itself.

Use at your own risk
--------------------

This software is provided "as is", without warranty of any kind, express or
implied. The authors accept no liability for lost saves, broken installs,
corrupted files, failed runs or any other damage arising from its use. It
edits an executable you own; keep the .orig backups, and remember that Steam's
Verify integrity of game files will always put you back to a clean install.

You must own a legitimate copy of Slayblade. This mod does not circumvent any
copy protection, DRM or licensing check - Slayblade ships without any - and
must not be used to enable or assist unauthorised copying.

Feedback
--------

Balance is opinion, and this pack is one player's. If a number feels wrong,
say so - and remember you can always install just the groups you agree with.
