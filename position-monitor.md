# Polymarket Position Loss Monitor

Hourly automated check that scans all open Polymarket positions and alerts if any have lost more than 5%.

## How It Works

1. Opens the browser (`openclaw` profile) and navigates to `https://polymarket.com/portfolio?tab=positions`
2. If not logged in, clicks **Log In → Continue with Google**
3. Scrolls down and clicks **"Show more"** repeatedly until all positions are visible
4. Reads every row in the positions table, focusing on the **VALUE column** (rightmost)
   - Each row shows: `$XX.XX -$X.XX (X.XX%)` — the percentage is in parentheses
5. Flags any position where the loss **exceeds -5%**
6. If any breaches found: sends an iMessage alert with market name, value, and % loss
7. If all positions are within threshold: **finishes silently**

## Alert Format (iMessage)

```
⚠️ Polymarket Loss Alert

The following positions have dropped more than 5%:

• Cavaliers vs. Pistons — $70.12 (-6.3%)
• [market name] — $XX.XX (-X.X%)
```

## Cron Job Config

- **Name:** Polymarket Position Loss Monitor (Hourly)
- **Cron Job ID:** `bcce4f9d-88d8-4b68-8e33-c23bfd4a7d1a`
- **Schedule:** Every 1 hour
- **Session Target:** isolated
- **Notify:** iMessage → raymondhthomas@gmail.com (only when a breach is detected)

## Cron Job Prompt

```
Open the browser using profile='openclaw' and navigate to https://polymarket.com/portfolio?tab=positions.
If not logged in, click 'Log In' then 'Continue with Google' to sign in.

Once the positions table has loaded:
1. Scroll down to make sure all positions are visible
2. If there is a 'Show more' button, click it and repeat until all positions are shown
3. Read every row in the positions table — focus on the VALUE column (rightmost data column).
   Each row shows a dollar value and a percentage change in parentheses, e.g. '$74.42 -$0.55 (0.73%)'
4. Identify any position where the percentage change is NEGATIVE and exceeds -5% (e.g. '-6.2%' would trigger an alert)
5. If any such positions are found, send an iMessage alert to raymondhthomas@gmail.com listing: market name, value, and percentage loss
6. If all positions are within the -5% threshold, finish silently — do NOT send a message

IMPORTANT: Only alert if a position has lost MORE than 5%. Green/positive positions and small losses under 5% should be ignored silently.
```

## Notes

- The percentage is shown in parentheses in the Value column — red text = negative, green = positive
- The page may require scrolling and/or a "Show more" click to reveal all positions
- Runs alongside the [Hourly Claimer](./README.md) for full portfolio automation
