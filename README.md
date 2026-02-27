# Polymarket Claimer 🤖

Automated browser bot that claims resolved winning positions on [Polymarket](https://polymarket.com) every hour using [OpenClaw](https://openclaw.ai) cron jobs.

## How It Works

1. Opens a browser (Chrome, `openclaw` profile) and navigates to `https://polymarket.com/portfolio`
2. If not logged in, clicks **Log In → Continue with Google**
3. Checks for a **"You won $X.XX — Claim"** banner
4. Clicks **Claim**, then confirms with the second **"Claim $X.XX"** button in the modal
5. If nothing to claim, finishes silently
6. Sends an iMessage notification when winnings are successfully claimed

## Setup

### Prerequisites

- [OpenClaw](https://openclaw.ai) installed and running
- A Polymarket account (logged in via Google)
- OpenClaw browser profile (`openclaw`) configured

### Cron Job Payload

```
Open the browser using profile='openclaw' and navigate to https://polymarket.com/portfolio.
If not logged in, click 'Log In' then 'Continue with Google' to sign in.
Once on the portfolio page, look for a 'Claim' button (there may be a banner like 'You won $X.XX — Claim').
If found, click it, then click the second 'Claim $X.XX' button in the modal to confirm.
If there is nothing to claim, finish silently.
Report the amount claimed (if any) in your response.
```

### Schedule

| Field    | Value              |
|----------|--------------------|
| Interval | Every 1 hour       |
| Target   | isolated session   |
| Notify   | iMessage (on claim only) |

## Notes

- Google login session persists in the browser profile's user data directory — only need to log in once
- If the browser restarts, a manual Google login may be required on the first run
- Works alongside [Polybot](https://github.com/raythomasai) for a fully automated Polymarket trading + claiming pipeline

## License

MIT
