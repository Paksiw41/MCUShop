# MCUShop — Player Guide

> A step-by-step guide to renting a plot and running your own chest shop.

---

## Table of Contents

1. [Quick Overview](#quick-overview)
2. [Step 1 — Find a Plot](#step-1--find-a-plot)
3. [Step 2 — Rent the Plot](#step-2--rent-the-plot)
4. [Step 3 — Set Up Your First Shop](#step-3--set-up-your-first-shop)
5. [Step 4 — Stock Your Shop](#step-4--stock-your-shop)
6. [Step 5 — Set a Teleport Point](#step-5--set-a-teleport-point)
7. [Step 6 — Set a Plot Description](#step-6--set-a-plot-description)
8. [Step 7 — Add Members to Your Plot](#step-7--add-members-to-your-plot)
9. [Managing Your Shops](#managing-your-shops)
10. [Keeping Your Rent Paid](#keeping-your-rent-paid)
11. [Teleporting to Your Plot](#teleporting-to-your-plot)
12. [Browsing Other Players' Shops](#browsing-other-players-shops)
13. [All Player Commands](#all-player-commands)

---

## Quick Overview

MCUShop lets you **rent a plot** in the market area and **create chest shops** inside it. Other players can browse your shops and buy or sell items through the signs you place on your chests.

**The flow at a glance:**

```
Find a free plot → Rent it → Place a chest → /shop create → Stock the chest → Profit
```

---

## Step 1 — Find a Plot

List all available plots and their rent prices:

```
/plot list
```

You will see a chat list of every plot. Free plots show their **ID** and **rent cost per period**. Already-rented plots show the current owner.

> **Tip:** Walk around the market area to find a plot you like before renting. Each plot is a protected region — you cannot build inside one until you rent it.

---

## Step 2 — Rent the Plot

Once you've chosen a plot, rent it with its ID:

```
/plot rent <id>
```

**Example:**
```
/plot rent 5
```

- The rent cost is deducted from your balance immediately.
- You are teleported inside the plot automatically.
- You are now the owner — you can build, place chests, and create shops.

To check the details of your rented plot at any time:
```
/plot info
```
*(Stand inside your plot and omit the ID, or type `/plot info <id>` from anywhere.)*

---

## Step 3 — Set Up Your First Shop

### What you need
- A **chest or barrel** placed inside your plot.
- The **item you want to trade** held in your main hand.

### Steps

**1.** Place a chest or barrel anywhere inside your plot.

**2.** Hold the item you want to sell (or buy) in your **main hand**.

**3.** Run:
```
/shop create
```

The **Shop Creation GUI** opens. Configure your shop:

| Button | What to do |
|--------|-----------|
| **Selling Price** (emerald) | Set the price *players pay you* to buy the item. Click → type a number → press Enter. Type `-1` to leave it disabled. |
| **Buying Price** (gold ingot) | Set the price *you pay players* when they sell the item to you. Click → type a number. Type `-1` to disable. |
| **Amount** (hopper) | How many items exchange per transaction (e.g. `1`, `16`, `64`). |
| **Mode** (cycles) | Click to cycle: **Selling Only** → **Buying Only** → **Selling & Buying**. This auto-updates when you set prices. |
| **Notifications** (ender eye) | Toggle ON to receive a chat message every time someone buys from or sells to your shop. |
| **✔ Create Shop** (lime wool) | Confirm. A sign is placed on the chest automatically. |
| **✘ Cancel** (barrier) | Discard and close. |

**4.** Click **✔ Create Shop** when you're happy with the settings.

A sign appears on the chest. It shows:
- The item name
- **Selling** price (if you sell) or **Buying** price (if you buy)
- The quantity per transaction
- Status: **AVAILABLE**, **OUT OF STOCK**, **ACCEPTING**, or **CHEST FULL**

> **Note:** You must set at least one price (selling or buying) before confirming.

---

## Step 4 — Stock Your Shop

Open your chest directly to put items in. You are the owner, so you can always open your own chests.

- If your shop is in **Selling** mode, fill the chest with the items you want to sell.
- If your shop is in **Buying** mode, make sure the chest has room to receive items from players.
- The sign status updates automatically when players interact.

---

## Step 5 — Set a Teleport Point

A teleport point lets players (and yourself) warp directly to your plot. This is especially useful so players browsing `/shops` can visit you in one click.

> **Requirement:** Your plot's district must have **Shop SetTP** enabled. If the button is greyed out in the GUI, ask an admin to enable it for your district.

### From the GUI (recommended)

**1.** Open your plot dashboard:
```
/shop manage
```

**2.** Click your plot → click **Set Shop TP** (ender pearl icon).

**3.** You'll see this message:
```
Walk to the spot you want as the shop teleport point.
Then Crouch + Right-Click any block to confirm.
Type cancel in chat to abort.
```

**4.** Walk to the exact spot you want players to land, then **crouch (Shift) and right-click** any block.

The teleport point is saved. All your shops in that plot now share this TP location.

### From the command line

```
/shop settp
```

Same flow — follow the on-screen instructions.

> **Cancel at any time** by typing `cancel` in chat.

---

## Step 6 — Set a Plot Description

Add a short description to your plot. It shows up when players hover over your plot in `/shops`.

**1.** Open your plot dashboard:
```
/shop manage
```

**2.** Click your plot → click **Set Description** (writable book icon).

**3.** Type your description in chat (e.g. `Best diamond shop in the market!`).

- Type `clear` to remove the description.
- Type `cancel` to go back without changing anything.

---

## Step 7 — Add Members to Your Plot

Members are trusted players who can help you run your plot. You control exactly what each member is allowed to do.

### Adding a member

**1.** Open your plot dashboard:
```
/shop manage
```

**2.** Click your plot → click **Members** (player head icon).

**3.** Click **Add Member** (green dye icon).

**4.** A list of currently online players is shown in chat. Type the player's name and press Enter.

**5.** The player is added as a member with default permissions:
- ✔ **Chest Access** — can open chests and barrels in your plot
- ✘ **Block Break** — cannot break blocks (off by default)
- ✘ **Block Place** — cannot place blocks (off by default)

### Editing member permissions

Click any member's head icon in the Members GUI to open their permission screen.

| Toggle | What it controls |
|--------|-----------------|
| **Chest Access** | Can open and stock chests/barrels in the plot |
| **Block Break** | Can break blocks in the plot |
| **Block Place** | Can place blocks in the plot |

Click each toggle to switch it on or off. Changes save instantly.

### Removing a member

Open the member's permission screen → click **Remove Member** (TNT icon).

---

## Managing Your Shops

### Open your shop dashboard

```
/shop manage
```

Shows all your rented plots. Click a plot to open its **Plot Dashboard**, where you can:

- See all your shops and click any to edit them
- View transaction history
- Set the teleport point
- Teleport to the plot
- Set a description
- Manage members

### Edit an existing shop

In the Plot Dashboard, click any shop icon → **Shop Edit GUI** opens.

| Button | Action |
|--------|--------|
| **Selling Price** | Change the price players pay you |
| **Buying Price** | Change the price you pay players |
| **Amount** | Change items per transaction |
| **Mode** | Cycle between Selling Only / Buying Only / Selling & Buying |
| **Notifications** | Toggle transaction alerts |
| **Sign Label** | Set a custom name for the shop sign |
| **Remove Shop** | Permanently delete this shop |

### View transaction history

In the Plot Dashboard → click **Transaction History** (book icon).

You'll see a list of every buy and sell, including the player name, item, amount, price, and timestamp.

### Remove a shop

Break the **sign** on any of your chests — the shop is removed automatically.

Or go to `/shop manage` → your plot → click the shop → click **Remove Shop**.

---

## Keeping Your Rent Paid

Your rent expires after each period (e.g. 24 hours). If it isn't renewed in time, you risk losing your plot.

### Check your plot status

```
/plot info
```

### Manually renew rent

```
/plot renew <id>
```

Opens the **Plot Renew GUI**:
- Click **Pay Rent** to pay the current balance (including any late-payment penalty).
- Toggle **Auto-Renew** ON to have the system automatically charge you before your rent expires.

### Auto-Renew

When Auto-Renew is ON:
- The system charges you automatically before your plot expires.
- If you don't have enough money, you'll get a warning and it will retry every few hours.
- If payment keeps failing, your plot eventually enters **delinquency** and may be foreclosed.

### If your plot is foreclosed

Your blocks and shop items are confiscated. Recover them (for a fee) with:

```
/plot claim
```

---

## Teleporting to Your Plot

### Teleport to your plot directly

If you only own one plot:
```
/shop tp
```

If you own multiple plots, a GUI opens — click the plot you want to visit.

You can also target a specific plot by ID:
```
/shop tp <plot-id>
```

### Teleport from the GUI

Open `/shop manage` → click your plot → click **Teleport to Plot** (compass icon).

---

## Browsing Other Players' Shops

```
/shops
```

Opens a public browser showing all rented plots in the market. Hover over any plot to see:
- The owner's name
- The district they're in
- Their plot description
- Whether you can teleport there

Click a plot to **teleport directly** to it (if the district allows player teleports).

---

## All Player Commands

| Command | Description |
|---------|-------------|
| `/plot list` | See all plots and their availability |
| `/plot rent <id>` | Rent a free plot |
| `/plot leave` | Voluntarily leave your rented plot (stand inside it first) |
| `/plot info [id]` | View details about a plot |
| `/plot renew <id>` | Pay overdue rent or manage auto-renew |
| `/plot claim` | Recover confiscated items after a foreclosure |
| `/shop create` | Create a shop on the nearest chest/barrel (hold your trade item) |
| `/shop manage` | Open your plot dashboard to manage shops, members, TP, and more |
| `/shop settp` | Enter SetTP mode to set your plot's teleport destination |
| `/shop tp [plot-id]` | Teleport to one of your rented plots |
| `/shops` | Browse all active player shops in the market |

> **Aliases:** `/plot` → `/p` · `/shop` → `/cs`

---

## Tips & Reminders

- You can own up to **2 plots** at a time (server limit).
- Each plot supports up to **5 shops** by default.
- Shops support both **chests and barrels**.
- The sign on your chest updates automatically — you don't need to do anything after stocking.
- Members you add can help stock chests but **cannot** create or remove shops — only you (the owner) can do that.
- Low stock alerts: if your chest drops to 5 items or fewer, you'll get a chat notification (if Notifications are on).
- Type `cancel` in chat any time you're prompted for input to go back.
