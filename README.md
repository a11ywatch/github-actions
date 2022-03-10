# A11yWatch Github Action

**Description:** `A11yWatch` scans a website url to get a11ywatch insight and guides you to better handling your content.

**Functionality:** This GitHub action allows for running dedicated website inclusion scans on `Pull Requests`, `Labels`, or `Push`.

### Inputs

**`WEBSITE_URL:`**
Website domain to scan (Start with http or https).

**`EXTERNAL:`**
Use the A11yWatch external api for faster results. (Subjected to rate limits).

**`CI_FAIL_ERROR_COUNT:`**
Determine whether to fail the CI if issues has errors above theshold.

### Organization/Scoped project

1. Set the url of the website you want to scan to `WEBSITE_URL`

### Example -

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
        uses: a11ywatch/a11ywatch-github-bot@v0.0.13
        with:
          WEBSITE_URL: ${{ secrets.WEBSITE_URL }}
```
