# A11yWatch Github Action

**Description:** `A11yWatch` scans a website url to get a11ywatch insight and guides you to better handling your content. Receive actionable items in your PR on what to fix and why.

**Functionality:** This GitHub action allows for running dedicated website inclusion scans on `Pull Requests`, `Labels`, or `Push`.

### Inputs

**`WEBSITE_URL:`**
Website domain to scan (Start with http or https).

**`FAIL_ERROR_COUNT:`**
Determine whether to fail the CI if issues has errors above theshold.

**`EXTERNAL:`**
Use the A11yWatch external API for faster results. (Subjected to rate limits).

**`DISABLE_PR_STATS:`**
Prevent the A11yWatch bot from posting to stats of scan to your PR.

**`TOKEN:`**
Set github api token of the user to send results to github. - defaults to github action token

### Organization/Scoped project

1. Set the url of the website you want to scan to `WEBSITE_URL`

### Example

A basic example adding the action to run accessibility and vitals.

```yaml
name: Run accessibility test across your website.
on: [push, pull]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: A11yWatch Scan
        uses: a11ywatch/github-action@v1.2.40
        id: a11ywatch-results-generator
        with:
          WEBSITE_URL: ${{ secrets.WEBSITE_URL }}
          EXTERNAL: false
          FAIL_ERROR_COUNT: 10
```

The following cron schedule expression will run `every 3 hours` to make sure your website is healthy.

```yaml
name: Run periodic website scans to make sure your page stays on par every three hours.
on:
  schedule:
    - cron: "*/3 * * * *"
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Website Cron Scan
        uses: a11ywatch/github-action@v1.1.1
        with:
          WEBSITE_URL: ${{ secrets.WEBSITE_URL }}
          EXTERNAL: false
          FAIL_ERROR_COUNT: 10
```
