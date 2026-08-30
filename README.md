# Draft Board

An auction-draft command centre for a 10-team, $600 PPR keeper league. Single file, no
build step, no dependencies, works offline once loaded. Built for use on an iPad during
a live draft.

## What it does

- **Bid check** — type a player and the bid currently on the floor. The call is about
  the *next* bid, since that is the one you would actually make, and it tells you the
  highest legal bid worth staying in for. Your ceiling accounts for the money you must
  hold back to fill your remaining roster spots. An off-increment number is read as the
  nearest legal bid and flagged.
- **Live inflation** — recalculates the market after every pick. Cash still in the room
  divided by the value of the players still worth buying. Player values move with it.
- **Every pick, every team** — record who won each player and at what price. Rival
  budgets and their maximum possible bid update as you go, sorted so the teams that can
  actually outbid you sit at the top.
- **Next target** — three ranked suggestions, filterable by position, based on which
  starting slots you still have open, how much better each player is than the one you
  would settle for instead, and whether their bye collides with your starters. A marker
  on the position tabs shows where you still have a dedicated slot to fill.
- **Going rate** — the price a player should actually fetch. Base value, adjusted for
  live inflation, then adjusted again for positional scarcity in this specific league.
- **Roster and byes** — starters slot automatically, flex included, with a warning when
  a single bye week would take out two or more of your starters.
- **By NFL team** — everyone still available grouped by their real team, sorted
  alphabetically, by bye week, or by which byes already hit your starters hardest. Use
  it to stack a quarterback with his receiver, and to see a bye clash before you bid
  rather than in October.
- **Watch list** — tap the star beside any player to park him in a panel of your own.
  Watched players who get bought are struck through and show who took them.
- **Every roster, not just yours** — the roster panel has a team selector, so you can
  check anyone's lineup, bench and bye clashes without leaving the page. Keepers are
  marked K. Bench rows show position and bye week.
- **Undo and backup** — one tap to reverse a mis-tap, and a JSON export so a browser
  clearing its storage mid-draft doesn't cost you the day.
- **Export** — CSV of every pick with price against going rate, a printable report of
  all rosters and money, or a plain-text summary on the clipboard for pasting into a
  group chat.

## The league

10 teams, $600 budget, $5 minimum, 15 roster spots, full PPR.
Lineup: QB / 2 RB / 2 WR / TE / FLEX / K / DEF, plus 6 bench.

Eight teams carried keepers into the auction; two are new and start with the full $600
and all 15 spots open. Keeper salaries come out of the budget.

Tap **Fix details** to correct team names, keeper prices, or the starting lineup.
Everything already recorded survives the edit, so it is safe to use mid-draft — handy
when a keeper is announced late or someone's price turns out to be wrong.

Keepers use `Team | Player | Price`, one per line. Names not in the player pool are
still marked as drafted, they just carry no value.

## Injury and suspension risk

A short curated list of players carrying availability risk discounts their value for
games they may not play, so scarcity, inflation and every going rate account for it.
Flagged players carry a red marker in the pool and the reason appears in the bid check
and on the recommendation card. Where a discount applies, the undiscounted price is
shown alongside so you can see what was taken off.

These notes were checked on 29 August 2026 and are a snapshot, not a feed. Re-check
anything flagged on the morning of the draft — suspension rulings in particular can
land at any time. The list lives in the `RISK` object near the top of the script.

## Draft type

**Fix details** has an Auction / Snake toggle.

In **auction** mode everything runs on money: budgets, maximum bids, inflation and going
rates. The budget and minimum bid are editable, and player values rescale to whatever
you set.

In **snake** mode money disappears entirely. The board tracks pick number, round and who
is on the clock, following serpentine order — odd rounds run down the list, even rounds
back up. Set the order in the same panel; you can put yourself anywhere. The bid check
becomes a draft check: it shows where a player sits on the board and whether he is
likely to survive until your next turn, comparing how many picks happen before your turn
against how many better players remain. Prices become ranks, the outbid panel becomes a
coming-up list, and the sold sheet pre-selects whoever is on the clock so recording a
pick is two taps.

Switching modes keeps your keepers and rosters. Money is dropped going into snake mode
and cannot be recovered by switching back, so export first if you want the record.

## Exporting results

Three buttons under the draft log:

- **CSV** — one row per pick with position, NFL team, bye, buyer, price, going rate and
  the difference between the two, so you can sort by who overpaid.
- **PDF** — builds a print-only report and opens the print dialog. On an iPad choose the
  share icon, then Save to Files. Covers every roster laid out by slot with keepers
  marked and bye stacks noted, a money table, and every pick in order. Empty bench rows
  are collapsed so a half-finished draft does not print pages of blanks.
- **Copy text** — a plain-text roster summary on the clipboard.

Backup, further up the same card, is different: it saves machine-readable JSON for
restoring a draft in progress, not a record to read.

## Bid increments

The minimum bid is $5 and bids rise in $5 steps, so every price the app shows is a bid
you can actually make. Values, going rates, ceilings and recorded sale prices all snap
to the increment; nothing suggests a number you cannot say out loud.

## Player values

Sourced from FFToday's 2026 PPR auction board (8/20/26), which is published for a
12-team, $200, 18-spot league. Those numbers do not transfer directly to other formats,
so the page rescales them: the dollars above the league minimum are redistributed in
proportion to the discretionary money actually available in your league.

Bye weeks are the 2026 NFL schedule.

Values assume full PPR. If your league uses half-PPR or standard, treat pass-catching
backs and slot receivers as somewhat cheaper than shown.

### Scarcity

Because every team's roster is tracked, positional demand is counted rather than
guessed: the app knows how many teams still need a tight end and how many good ones are
left. A position where one good player remains per team that needs one is the balance
point; above that, the stragglers get bid up.

A flex counts as one roster spot split across the positions that can fill it, not as
full demand at each. The resulting multipliers are then normalised so the total the
model expects to be spent still equals the money actually in the room. Scarcity moves
dollars between positions, it does not invent them.

## Publishing

`index.html` is the whole application. Put it in the repository root, then
**Settings → Pages → Deploy from branch → `main` → `/ (root)`**.

Nothing leaves the browser. Draft state lives in local storage on the device you are
using, which is why the backup button exists.

Drafting on hotel or venue wifi is a bad bet. Open the page once beforehand and it will
keep working if the connection drops, since everything runs client-side and state lives
in browser storage.
