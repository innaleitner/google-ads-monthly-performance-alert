# google-ads-monthly-performance-alert
A Google Ads Script that tracks your monthly performance against goals, shows pacing progress, labels campaigns by health, and sends you a daily HTML email summary.

What It Does
Account-Level Summary
Compares your Cost, Revenue, and ROAS (Conv. value / Cost) against your monthly goals — not just as raw numbers, but with full pacing context:

Current Actual - what you've spent/earned so far this month
Expected at Day X - what you should have at this point in the month
Variance vs Pace — how far ahead or behind you are right now
Projected Month-End - where you'll finish if current pace continues
Status Badge — ON PACE / OVER PACE / UNDER PACE / UNDER TARGET

Campaign-Level Breakdown
Every campaign (active, paused, and removed) is listed with its performance metrics and an actionable label:
1. 🚀 SCALE UP: High ROAS + Revenue ahead of pace, Increase budget
2. ✅ ON TRACK: Good ROAS + Revenue on pace, Maintain
3. ⚠️ NEEDS ATTENTION: ROAS or revenue off target, Investigate
4. 🔴 UNDERPERFORMING: Poor ROAS + Revenue below pace, Review or pause
5. ⏸️ PAUSED: Campaign paused, had activity this month, Consider reactivating if ROAS was good
6. 🗑️ REMOVED: Campaign removed, historical data only, Reference only
Campaigns are sorted by priority - your best opportunities first, your biggest problems last.

Key Alerts Section
The email highlights the most important issues clearly:

Budget exceeding by X amount
Revenue behind pace with projected shortfall
ROAS below target with percentage gap

Email Report
Sends a clean, formatted HTML email with all of the above. Works as a daily or weekly digest.

Data Coverage

Uses yesterday's data (today minus 1 day) to avoid reporting on incomplete intraday numbers
Captures all campaigns regardless of status — enabled, paused, and removed — so your monthly totals are always accurate
Works in both single accounts and MCC (manager accounts)


Setup
1. Open Google Ads Scripts
Go to your Google Ads account → Tools & Settings → Bulk Actions → Scripts
2. Create a New Script
Click the + button to create a new script. Paste the full script into the editor.
3. Configure Your Goals
At the top of the script, find the CONFIG block and update it with your numbers:

  goals: {
    cost: 50000,       // Your monthly budget goal (in your account currency)
    revenue: 100000,   // Your monthly revenue goal
    roas: 2.0          // Your target ROAS (Conv. value / Cost)
  },
  email: {
    recipient: "your.email@example.com",  // Where to send the report
    ccRecipients: "",                      // Optional CC (comma-separated)
    subject: "Google Ads Monthly Performance Alert"
  },
  minCampaignCost: 100,  // Minimum spend to include a campaign in the report
};

4. Authorize the Script
Click Authorize when prompted. The script needs permission to:

Read your Google Ads data
Send email via your Google account (Gmail)

5. Run a Test
Click Run to preview the output. Check your inbox — the email should arrive within a minute.
6. Schedule It
Click Frequency and set a schedule. 

Adjusting Thresholds
The campaign labeling uses configurable thresholds you can tune to your account:
javascriptthresholds: {
  revenueAheadPace: 1.10,     // 10% above expected pace = SCALE UP candidate
  revenueOnPace: 0.90,        // Within 10% of pace = ON TRACK
  roasOnTarget: 0.95,         // Within 5% of ROAS goal = acceptable
  roasUnderperforming: 0.85   // Below 85% of ROAS goal = UNDERPERFORMING
}
For example, if your ROAS goal is 2.0x:

A campaign at 1.95x ROAS (97.5% of target) → still considered ON TARGET
A campaign at 1.65x ROAS (82.5% of target) → flagged as UNDERPERFORMING

Currency
The script defaults to € (Euro). To change the currency symbol, find the formatCurrency function and update the symbol:
javascriptfunction formatCurrency(value) {
  return "€" + value.toFixed(2)...  // Change € to $, £, etc.
}

Notes

The script only counts campaigns that had at least €100 in spend (configurable via minCampaignCost) to keep the report clean
Paused and removed campaigns that had activity this month are included in totals and shown separately in the campaign table
ROAS is calculated as Conversion Value ÷ Cost, matching the "Conv. value/cost" column in Google Ads
The pacing model assumes linear distribution — it does not account for weekday/weekend seasonality
