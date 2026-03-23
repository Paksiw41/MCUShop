# MCUShop — Plugin Reference

> **Version:** 1.0.0 · **API:** Paper 1.21.4 · **Java:** 21

---

## Table of Contents

1. [Dependencies](#dependencies)
2. [Commands](#commands)
   - [/plot](#plot)
   - [/shop](#shop)
   - [/shops](#shops)
   - [/mcushop (admin)](#mcushop-admin)
3. [GUIs](#guis)
   - [Plot Manager GUI](#plot-manager-gui-mcs-plots)
   - [Plot Renew GUI](#plot-renew-gui-plot-renew)
   - [Confiscated Items GUI](#confiscated-items-gui-plot-claim)
   - [Shop Creation GUI](#shop-creation-gui-shop-create)
   - [Shop Manage GUI](#shop-manage-gui-shop-manage)
   - [Plot Dashboard GUI](#plot-dashboard-gui)
   - [Shop List GUI](#shop-list-gui)
   - [Shop Edit GUI](#shop-edit-gui)
   - [Shop Transaction GUI](#shop-transaction-gui)
   - [Shops Browser GUI](#shops-browser-gui-shops)
   - [District List GUI](#district-list-gui-mcs-districts)
   - [District Edit GUI](#district-edit-gui)
4. [Permissions](#permissions)
5. [Configuration](#configuration)
6. [Messages & Placeholders](#messages--placeholders)
7. [PlaceholderAPI](#placeholderapi)

---

## Dependencies

| Plugin | Type | Purpose |
|--------|------|---------|
| **Vault** | Required | Economy — all rent payments, shop transactions, and balance checks |
| **WorldGuard** (+ WorldEdit) | Required | Plot region boundaries, block protection, and district region lookup |
| **PlaceholderAPI** | Optional (soft) | External placeholder expansion *(not yet implemented — see [PlaceholderAPI](#placeholderapi))* |

Any economy plugin that hooks into Vault works (EssentialsX, CMI, etc.).

---

## Commands

### `/plot`

**Alias:** `/p`
**Permission required for all subcommands:** see table below.

| Subcommand | Usage | Description |
|-----------|-------|-------------|
| `list` | `/plot list` | Lists every registered plot, its status, and the rent cost. |
| `rent` | `/plot rent <id>` | Rent a free plot. Deducts rent cost from your balance and teleports you in. |
| `leave` | `/plot leave` | Voluntarily vacate your plot. Stand inside it first. Removes all your shops. |
| `info` | `/plot info [id]` | Show details for a plot. Omit `id` to inspect the plot you're standing in. Displays delinquency status and remaining days if overdue. |
| `renew` | `/plot renew <id>` | Opens the **Plot Renew GUI** to manually pay overdue rent or manage auto-renew settings. |
| `claim` | `/plot claim` | Opens the **Confiscated Items GUI** to recover your blocks and shop items after a foreclosure. Requires paying the claim fee. |

**Tab completion:** subcommand names → plot IDs (free plots for `rent`, all plots for `info`, delinquent plots for `renew`).

---

### `/shop`

**Alias:** `/cs`
**All subcommands require the player to be in-game.**

| Subcommand | Usage | Description |
|-----------|-------|-------------|
| `create` | `/shop create` | Opens the **Shop Creation GUI** for the nearest free chest or barrel within 5 blocks. You must be standing inside your rented plot. Alternatively, right-click any chest/barrel in your plot directly. |
| `remove` | `/shop remove` | Opens the **Shop View GUI** for the nearest shop within 10 blocks. Confirm removal from within the menu. Admins (`mcushop.admin`) can remove any player's shop. |
| `info` | `/shop info` | Opens the **Shop View GUI** for the nearest shop within 10 blocks (read-only if not the owner). |
| `manage` | `/shop manage` | Opens the **Shop Manage GUI** — shows all your rented plots so you can manage shops, transactions, and teleport settings per plot. |
| `settp` | `/shop settp` | Enters **SetTP mode** — sends instructions, then waits for you to crouch + right-click any block to confirm the teleport destination. Requires the district to have **Shop SetTP** enabled. |
| `tp` | `/shop tp [plot-id]` | Teleports you to one of your rented plots. If you own one plot, teleports immediately. If you own multiple, opens the **Plot Picker GUI** to choose. |

**Tab completion:** subcommand names; `tp` also completes your rented plot IDs.

---

### `/shops`

**Permission:** none (open to all players)
**Player only.**

Opens the **Shops Browser GUI** — a public-facing paginated list of all currently rented plots. Players can browse shops and teleport to them if the district allows it.

---

### `/mcushop` (admin)

**Alias:** `/mcs`
**Requires:** `mcushop.admin`
**Console-safe** unless otherwise noted.

| Subcommand | Usage | Description |
|-----------|-------|-------------|
| `reload` | `/mcs reload` | Reloads `config.yml` and `messages.yml` without restarting the server. |
| `plots` | `/mcs plots` | *(Player only)* Opens the **Plot Manager GUI** — paginated list of all plots. Click any plot to open its edit screen. |
| `addplot` | `/mcs addplot <id> <region> [world]` | Registers an existing WorldGuard region as a rentable plot. The player's current location becomes the teleport point. Console must supply `[world]`. |
| `removeplot` | `/mcs removeplot <id>` | Unregisters a plot (evicts renter, removes shops). The WorldGuard region itself is **not** deleted. |
| `resetplot` | `/mcs resetplot <id>` | Evicts the current renter and removes all their shops without unregistering the plot. |
| `givemoney` | `/mcs givemoney <player> <amount>` | Deposits money into a player's economy account via Vault. |
| `adddistrict` | `/mcs adddistrict <region> <name>` | Links a WorldGuard region to a named district. The region must exist in the current world. |
| `districts` | `/mcs districts` | *(Player only)* Opens the **District List GUI** — paginated list of all districts. |

**Tab completion:**

| Argument | Suggestions |
|---------|-------------|
| Sub-command | `reload`, `plots`, `addplot`, `removeplot`, `resetplot`, `givemoney`, `adddistrict`, `districts` |
| `addplot` arg 1 | Next available plot ID |
| `addplot` arg 2 | WorldGuard region names in the executing player's world |
| `addplot` arg 3 | Loaded world names |
| `removeplot` / `resetplot` arg 1 | All registered plot IDs |
| `givemoney` arg 1 | Online player names |
| `givemoney` arg 2 | `100`, `500`, `1000` |
| `adddistrict` arg 1 | WorldGuard region names |

---

## GUIs

### Plot Manager GUI (`/mcs plots`)

A 54-slot paginated inventory. Supports up to 45 plots per page.

| Colour | Meaning |
|--------|---------|
| Green pane | Plot is free |
| Yellow pane | Plot is rented (active) |
| Red pane | Plot is rented but expired/grace period |

Click any plot pane → opens the **Plot Edit GUI**:

| Button | Action |
|--------|--------|
| **Set Rent Price** (gold ingot) | Prompts a chat value. Type a number (e.g. `250`). Overrides the global `economy.rent-cost` for this plot only. |
| **Set Rent Duration** (clock) | Prompts a chat value in `Xd Xh Xm` format (e.g. `1d`, `12h`, `1d12h30m`). Overrides `economy.rent-period-hours` for this plot only. |
| **Teleport** (ender pearl) | Teleports you to the plot's registered spawn point. |
| **Reset Plot** (lava bucket) | Evicts the current renter and clears all their shops. Only shown when the plot is rented. |
| **Back** (arrow) | Returns to the Plot Manager list. |
| **Close** (barrier) | Closes the GUI. |

---

### Plot Renew GUI (`/plot renew`)

A 45-slot GUI that lets the owner manage rent for a specific plot.

| Button | Action |
|--------|--------|
| **Plot Info** (written book, slot 4) | Displays plot ID, expiry date, rent cost, and current delinquency status. |
| **Pay Rent** (emerald / redstone, slot 22) | Manually pays the overdue rent including any late-payment penalty. Green = affordable, Red = insufficient funds. |
| **Auto-Renew Toggle** (clock / comparator, slot 31) | Toggles per-plot auto-renew ON or OFF. When ON, the system automatically charges rent before expiry. Lore shows the configured fee, trigger window, and retry interval. |
| **Close** (barrier, slot 40) | Closes the GUI. |

**Auto-Renew behaviour:**

1. When enabled, the system watches for plots whose time remaining falls within the trigger window (`economy.auto-renew-trigger-hours`, default 24 h).
2. If the owner's balance covers `rent + auto-renew fee`, rent is extended immediately.
3. If the owner cannot afford it, a warning is sent in chat and a retry is scheduled every `economy.auto-renew-retry-hours` (default 3 h) until the plot expires.
4. If the owner pays manually via **Pay Rent** before expiry, the pending retry is cancelled and auto-renew resets for the next cycle.

---

### Confiscated Items GUI (`/plot claim`)

A 54-slot paginated inventory showing all blocks and shop items recovered from a foreclosed plot.

| Button | Action |
|--------|--------|
| **Pay & Claim All** (emerald / redstone) | Deducts the claim fee and gives all items to your inventory. Items that don't fit drop at your feet. Green = affordable, Red = insufficient funds. |
| **Close** (barrier) | Close without claiming. Items remain stored until claimed. |
| Prev / Next arrows | Navigate pages. |

---

### Shop Creation GUI (`/shop create`)

A 45-slot GUI opened when a player runs `/shop create` while near a chest or barrel inside their rented plot.

All labels are from the **shop owner's point of view**:

| Slot | Button | Description |
|------|--------|-------------|
| 4 | **Item Preview** (trade item) | The item that will be traded. Shows amount per transaction. Read-only. |
| 19 | **Sell Price** (emerald) | Price you charge when *selling* items to players. Click to set; `-1` to disable. |
| 21 | **Buy Price** (gold ingot) | Price you pay when *buying* items from players. Click to set; `-1` to disable. |
| 23 | **Amount** (hopper) | How many items exchange per transaction. Click to set. |
| 25 | **Mode** (cycles) | Cycles through: **Sell Only** → **Buy Only** → **Buy & Sell**. Auto-syncs when prices are set. |
| 31 | **Notifications** (ender eye / paper) | Toggle whether you receive a chat message on every transaction. |
| 38 | **Create Shop** (lime wool) | Confirms creation — places a sign on the chest/barrel and registers the shop. |
| 42 | **Cancel** (barrier) | Discards all changes and closes. |

> Shop creation is blocked if the district's shop cap has been reached.

---

### Shop Manage GUI (`/shop manage`)

A 54-slot paginated hub showing all of the player's rented plots. Each plot icon displays the region name, expiry date, shop count, and delinquency status.

Clicking a plot opens the **Plot Dashboard GUI** for that plot.

| Button | Action |
|--------|--------|
| **Plot icons** (emerald / redstone block) | Click to open that plot's dashboard. |
| **Close** (barrier, slot 8) | Closes the GUI. |
| Prev / Next arrows (slots 45 / 53) | Navigate pages (up to 36 plots per page). |

---

### Plot Dashboard GUI

A 54-slot GUI opened when a plot is clicked in the **Shop Manage GUI**. Shows all shops in that plot and provides quick-action buttons.

**Top rows (slots 9–35):** All shops in the plot as clickable icons. Clicking a shop opens its **Shop Edit GUI**.

**Action row (slots 36–44):**

| Slot | Button | Action |
|------|--------|--------|
| 36 | **Back** (arrow) | Returns to the Shop Manage GUI. |
| 37 | **Transaction History** (book) | Opens the **Shop Transaction GUI** filtered to this plot's shops. |
| 39 | **Set Shop TP** (ender pearl / grey dye) | Enters SetTP mode for this plot. Shown as ender pearl if the district allows it, grey dye if not. When active, walk to the desired TP spot and crouch + right-click any block to confirm. The location is saved to all your shops in this plot. |
| 41 | **Teleport to Plot** (compass) | Teleports you to this plot's entrance (owner-only private use). |
| 43 | **Set Description** (writable book) | Prompts a chat message to set a short public description for this plot. Type `clear` to remove it. The description is shown to all players browsing `/shops`. |
| 44 | **Close** (barrier) | Closes the GUI. |

**Nav row (slots 45–53):** Prev / page info / Next when the plot has more than 27 shops.

> **SetTP mode** — after clicking Set Shop TP or running `/shop settp`, the system waits for you to crouch + right-click any block. Type `cancel` in chat to abort without changing anything.

---

### Shop List GUI

A 54-slot paginated list of all shops owned by the player. Each slot displays the trade item with lore showing:

- Item name and shop ID
- Sell price (you charge players) and Buy price (you pay players)
- Current stock

Clicking a shop opens its **Shop Edit GUI**. Back button returns to the Shop Manage GUI.

---

### Shop Edit GUI

A 45-slot GUI for editing an existing shop. Layout and labels mirror the [Shop Creation GUI](#shop-creation-gui-shop-create). Additional buttons:

| Button | Action |
|--------|--------|
| **Remove Shop** (TNT, slot 40) | Permanently removes the shop and its sign. Prompts confirmation. |

---

### Shop Transaction GUI

A 54-slot paginated transaction history, sorted newest-first (up to 200 entries). Can show transactions for all the owner's shops or filtered to a specific plot.

Each slot displays the traded item. Hover lore shows:

- Player who bought or sold
- Direction: **SOLD** (you sold to player) or **BOUGHT** (you bought from player)
- Shop ID
- Amount and total price
- Date and time of the transaction

---

### Shops Browser GUI (`/shops`)

A public-facing 54-slot paginated GUI showing all currently rented plots. Any player can open this with `/shops`.

**Sorting:** Plots are sorted by their district's **sort weight** descending (highest weight first). Plots not inside any district appear last.

**Each plot icon** displays:

- Owner name
- District name (or "None" if not in a district)
- Plot description (if the owner has set one)
- Number of shops in the plot
- Teleport availability: **✔ Click to teleport** or **✘ Teleport unavailable**

**Clicking a plot:**
- If the district has **Player TP** enabled → teleports the browsing player to the plot's entrance.
- If Player TP is disabled or the plot has no TP point → sends an error message, no teleport.

**Icon colour:**
| Material | Meaning |
|----------|---------|
| Gold Block | District with weight ≥ 10 (premium placement) |
| Emerald Block | Normal district with Player TP enabled |
| Grey Glass Pane | Player TP disabled for this plot |

---

### District List GUI (`/mcs districts`)

A 54-slot paginated list of all registered districts. Each slot shows district name, WorldGuard region, world, tax rate, shop cap, and enabled perks.

Click a district → opens the **District Edit GUI**.

---

### District Edit GUI

A 45-slot GUI for editing a specific district. All changes take effect immediately and are persisted to the database.

| Slot | Button | Action |
|------|--------|--------|
| 0 | **Back** (arrow) | Returns to the District List GUI. |
| 4 | **Info** (written book) | Displays district name, region, world, tax rate, shop cap, and perk status. |
| 19 | **Tax Rate** (gold ingot) | Click to set the tax percentage (0–100). Tax is deducted from the shop owner's payout on every BUY transaction. Type `0` to disable. |
| 22 | **Max Shops** (chest) | Maximum chest shops allowed inside this district. Type `-1` for unlimited (falls back to `economy.district-max-shops` in config). |
| 25 | **Shop SetTP** toggle (ender pearl / ender eye) | Toggle whether shop owners in this district can use `/shop settp` (crouch + right-click TP setter). Green = enabled, Red = disabled. |
| 28 | **Player TP** toggle (compass / grey dye) | Toggle whether any player browsing `/shops` can click this district's plots to teleport in. Green = enabled, Red = disabled. This is separate from the owner's private teleport. |
| 31 | **Sort Weight** (comparator) | Integer weight used to order this district's plots in the `/shops` GUI. Higher = shown first. Click to change. |
| 38 | **Remove District** (TNT) | Permanently removes this district. The WorldGuard region is not affected. |
| 44 | **Close** (barrier) | Closes the GUI. |

**District perks summary:**

| Perk | What it controls |
|------|-----------------|
| **Tax Rate** | % deducted from shop owner payout when a player buys from a shop in this district |
| **Max Shops** | Hard cap on total chest shops that can be created inside this district |
| **Shop SetTP** | Allows plot owners to set a custom teleport destination for their shops (`/shop settp` or GUI button) |
| **Player TP** | Allows any player browsing `/shops` to teleport to plots in this district |
| **Sort Weight** | Controls display order in `/shops` — higher weight districts and their plots appear at the top |

**Tax behaviour:** `owner receives = price × (1 − tax% / 100)`. Tax only applies to BUY transactions (a player purchasing from a shop).

---

## Permissions

| Node | Default | Description |
|------|---------|-------------|
| `mcushop.plot.rent` | `true` | Allows using `/plot rent`. |
| `mcushop.plot.leave` | `true` | Allows using `/plot leave`. |
| `mcushop.shop.create` | `true` | Allows creating chest shops via `/shop create` or right-click. |
| `mcushop.shop.remove` | `true` | Allows removing your own shops via `/shop remove`. |
| `mcushop.admin` | `op` | Full admin access — all `/mcs` commands, removing any player's shops, bypassing WorldGuard plot protection. |

> **Note:** `/plot info`, `/plot list`, `/plot renew`, `/plot claim`, `/shop manage`, `/shop tp`, and `/shops` have no explicit permission node and are usable by all players.

---

## Configuration

File: `plugins/MCUShop/config.yml`

The plugin ships a `config-version` key. On every startup, `ConfigMigrator` compares the bundled version with the live file and automatically adds any missing keys without overwriting admin-set values. The console logs whether the config was initialised, migrated, or already up to date.

```yaml
config-version: 1           # Bumped with each release that adds new config keys

database:
  type: sqlite              # sqlite or mysql
  host: localhost           # MySQL only
  port: 3306                # MySQL only
  name: mcushop             # MySQL only
  username: root            # MySQL only
  password: ''              # MySQL only

economy:
  currency-symbol: '$'
  rent-cost: 100.0          # Default rent cost per period
  rent-period-hours: 24     # Default rent duration in hours
  grace-period-hours: 6     # Hours after expiry before system auto-renew kicks in (legacy)
  max-shops-per-plot: 5     # Max chest shops a renter can create per plot
  district-max-shops: -1    # Global default shop cap per district (-1 = unlimited)

  # Delinquency & Foreclosure
  auto-renew: true                # Automatically attempt renewal when rent expires (system-wide)
  renewal-penalty-percent: 25     # % added to rent price on missed payment (e.g. 25 = +25%)
  delinquency-evict-days: 3       # Days of non-payment before foreclosure
  claim-penalty-percent: 50       # % of base rent cost charged to claim confiscated items

  # Per-plot auto-renew (player-controlled via /plot renew GUI)
  per-plot-auto-renew-fee: 0.0        # Extra fee charged on top of rent when auto-renew fires
  auto-renew-trigger-hours: 24        # Hours before expiry at which auto-renew first attempts
  auto-renew-retry-hours: 3           # Hours between retry attempts if balance was insufficient

plots:
  world: world              # (legacy field — world is per-plot via WorldGuard)
  max-plots-per-player: 2   # Maximum plots a single player can rent at once

shop:
  allow-buy-only: true      # Allow shops that only sell items (owner sells to players)
  allow-sell-only: true     # Allow shops that only buy items (owner buys from players)

notifications:
  low-stock-threshold: 5              # Alert owner when stock drops to or below this
  notify-owner-on-low-stock: true     # Toggle low-stock notifications
```

### Per-plot overrides

Per-plot price and duration can be set from `/mcs plots` → Plot Edit GUI. These override the global `economy.rent-cost` and `economy.rent-period-hours` for that plot only. Set to `-1` internally to revert to config defaults.

### Barrel support

Chest shops work with both **chests** and **barrels**. Right-clicking a barrel inside your rented plot opens the same Shop Creation GUI as a chest would. Stock counts, sign refresh on close, and buy/sell transactions all support both container types.

---

## Messages & Placeholders

File: `plugins/MCUShop/messages.yml`

All messages support `&` colour codes. Runtime values use `{placeholder}` tokens.

### `general`

| Key | Placeholders | Description |
|-----|-------------|-------------|
| `no-permission` | — | Sent when a player lacks permission. |
| `player-only` | — | Sent when console runs a player-only command. |
| `invalid-usage` | `{usage}` | Generic wrong-usage message. |
| `reload-success` | — | Sent to the admin who ran `/mcs reload`. |

### `plot`

| Key | Placeholders | Description |
|-----|-------------|-------------|
| `rent-success` | `{id}`, `{cost}` | Sent when a player successfully rents a plot. |
| `rent-fail-no-money` | `{cost}` | Insufficient funds for renting. |
| `rent-fail-occupied` | — | Plot already rented. |
| `rent-fail-max` | `{max}` | Player already owns the max number of plots. |
| `leave-success` | `{id}` | Player vacated their plot. |
| `leave-fail-not-owner` | — | Player doesn't own the plot they're standing in. |
| `info-header` | `{id}` | Header line of `/plot info`. |
| `info-owner` | `{owner}` | Owner name line. |
| `info-expires` | `{expires}` | Formatted expiry timestamp (`yyyy-MM-dd HH:mm`). |
| `info-shops` | `{shops}`, `{max}` | Current shop count vs. max. |
| `info-unowned` | `{cost}` | Shown when the plot has no renter. |
| `list-header` | — | Header of `/plot list`. |
| `list-entry` | `{id}`, `{cost}` | Free plot row. |
| `list-entry-taken` | `{id}`, `{owner}` | Rented plot row. |
| `renewal-reminder` | `{id}`, `{hours}` | Sent 1 hour before expiry. |
| `renewal-auto-success` | `{id}`, `{cost}` | Auto-renewal succeeded. |
| `renewal-auto-fail` | `{id}`, `{percent}`, `{cost}`, `{days}` | Auto-renewal failed — player marked delinquent. |
| `delinquent-retry-fail` | `{id}`, `{days}` | Retry payment still failed; shows remaining days. |
| `expired-evicted` | `{id}` | Sent on eviction when `auto-renew: false`. |
| `foreclosed` | `{id}`, `{fee}` | Sent when a plot is forcibly foreclosed. |
| `renew-success` | `{id}`, `{cost}`, `{expires}` | Manual renewal via `/plot renew` succeeded. |
| `renew-fail-no-money` | `{id}`, `{cost}`, `{percent}` | Insufficient funds for manual renewal. |
| `renew-fail-not-delinquent` | `{id}` | Plot is current — no overdue payment needed. |
| `renew-fail-not-owner` | — | Player doesn't own the plot. |
| `claim-none` | — | No confiscated items to claim. |
| `claim-success` | `{fee}` | Items successfully claimed. |
| `claim-fail-no-money` | `{fee}` | Insufficient funds to pay claim fee. |
| `auto-renew-success` | `{id}`, `{cost}` | Per-plot auto-renew charged successfully. |
| `auto-renew-fail` | `{id}`, `{cost}` | Per-plot auto-renew failed — insufficient balance. |

### `shop`

| Key | Placeholders | Description |
|-----|-------------|-------------|
| `create-success` | `{item}`, `{price}` | Shop created. |
| `create-fail-not-in-plot` | — | Player isn't in their own rented plot. |
| `create-fail-max` | `{max}` | Shop limit reached for this plot. |
| `create-fail-no-chest` | — | No chest or barrel found to attach a shop to. |
| `remove-success` | — | Shop removed. |
| `remove-fail-not-owner` | — | Player doesn't own the shop. |
| `buy-success` | `{amount}`, `{item}`, `{price}` | Successful purchase from a shop (player bought, owner sold). |
| `buy-fail-no-money` | `{price}` | Buyer can't afford the item. |
| `buy-fail-out-of-stock` | — | Shop chest/barrel is empty. |
| `buy-fail-inventory-full` | — | Buyer's inventory is full. |
| `sell-success` | `{amount}`, `{item}`, `{price}` | Successful sale to a shop (player sold, owner bought). |
| `sell-fail-no-item` | `{item}` | Seller doesn't have the item. |
| `sell-fail-shop-full` | — | Shop chest/barrel has no room. |
| `low-stock-notify` | `{id}`, `{amount}` | Sent to shop owner when stock is low. |
| `info-header` | — | Header of shop info view. |
| `info-item` | `{item}` | Item name. |
| `info-price-buy` | `{price}` | Buy price (owner pays players). |
| `info-price-sell` | `{price}` | Sell price (owner charges players). |
| `info-stock` | `{stock}` | Current stock count. |
| `info-owner` | `{owner}` | Shop owner name. |

---

## PlaceholderAPI

MCUShop lists PlaceholderAPI as a **soft dependency** but does **not** currently ship a PAPI expansion class. No `%mcushop_*%` placeholders are registered at this time.

Planned placeholders (not yet implemented):

| Placeholder | Expected value |
|------------|---------------|
| `%mcushop_plot_id%` | ID of the plot the player currently owns (or `none`) |
| `%mcushop_plot_expires%` | Formatted expiry time of the player's plot |
| `%mcushop_plot_status%` | `active`, `delinquent`, or `none` |
| `%mcushop_shop_count%` | Number of shops the player has in their plot |
| `%mcushop_balance%` | Player's Vault balance (formatted) |

To implement these, create a class that extends `me.clip.placeholderapi.expansion.PlaceholderExpansion` and register it in `MCUShop#onEnable`.
