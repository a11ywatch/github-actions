# github-action

[![Tests](https://github.com/a11ywatch/github-actions/actions/workflows/action.yml/badge.svg)](https://github.com/a11ywatch/github-actions/actions/workflows/action.yml)

A feature rich GitHub action that runs actionable accessibility reports on your website that can handle large workloads.

Some of the primary features include pass/fail testing, code fixes, and detailed reports.

When running locally the action uses A11yWatch Lite.

### Usage

```yaml
- uses: a11ywatch/github-action@v2
  with:
    WEBSITE_URL: ${{ secrets.WEBSITE_URL }}
    SUBDOMAINS: true
    TLD: false
    SITEMAP: true
    FAIL_ERRORS_COUNT: 15
    LIST: true
    FIX: false
    UPGRADE: false
    RECORD: recordings
    COMPUTER_VISION_SUBSCRIPTION_KEY: ${{ secrets.COMPUTER_VISION_SUBSCRIPTION_KEY }}
    COMPUTER_VISION_ENDPOINT: ${{ secrets.COMPUTER_VISION_ENDPOINT }}
```

### Action inputs

All inputs are **optional** except $WEBSITE_URL.

| Name                               | Description                                                                                                                                                                                                              | Default        |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------- |
| `WEBSITE_URL`                      | Website domain to scan (Start with http:// or https://).                                                                                                                                                                 |                |
| `SITE_WIDE`                        | Site-wide scanning across all pages.                                                                                                                                                                                     | false          |
| `FIX`                              | Attempt to apply recommendations to code and commit to github.                                                                                                                                                           | false          |
| `SUBDOMAINS`                       | Include all subdomains (required SITE_WIDE=true).                                                                                                                                                                        | true           |
| `SITEMAP`                          | Extend crawl with sitemap links (required SITE_WIDE=true).                                                                                                                                                               | true           |
| `TLD`                              | Include all tld extensions (required SITE_WIDE=true).                                                                                                                                                                    | true           |
| `LIST`                             | Report the results to github as a pass or fail list or detailed report.                                                                                                                                                  | false          |
| `RECORD`                           | Record the audit as video to a directory.                                                                                                                                                                                |                |
| `FAIL_TOTAL_COUNT`                 | Determine whether to fail the CI if total issues warnings and errors exceed the counter. Takes precedence over the other FAIL inputs.                                                                                    | 0              |
| `FAIL_ERRORS_COUNT`                | Determine whether to fail the CI if total issues with errors exceed the counter.                                                                                                                                         | 0              |
| `FAIL_WARNINGS_COUNT`              | Determine whether to fail the CI if total issues with warnings exceed the counter.                                                                                                                                       | 0              |
| `EXTERNAL`                         | Use the A11yWatch remote API for fast results. If this is set `A11YWATCH_TOKEN` is needed.                                                                                                                               |                |
| `COMPUTER_VISION_SUBSCRIPTION_KEY` | [Computer Vision](https://azure.microsoft.com/en-us/services/cognitive-services/computer-vision/#overview) API key for image recognition on alts.                                                                        |                |
| `COMPUTER_VISION_ENDPOINT`         | Computer Vision url endpoint.                                                                                                                                                                                            | false          |
| `DISABLE_PR_STATS`                 | Prevent messaging to the pr results of test.                                                                                                                                                                             | false          |
| `TOKEN`                            | `GITHUB_TOKEN` (permissions `contents: write` and `pull-requests: write`) or a `repo` scoped [Personal Access Token (PAT)](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token). | `GITHUB_TOKEN` |
| `A11YWATCH_TOKEN`                  | The A11yWatch API token to use to identify a user.                                                                                                                                                                       |                |
| `SLIM`                             | Use the gRPC client to gather reports - only displays stats, useful for large websites (no code generation, no outputs, just pure stats)                                                                                 |                |
| `UPGRADE`                          | Upgrade the docker images before testing to latest.                                                                                                                                                                      |                |

### Action Outputs

| Name     | Description                | Default |
| -------- | -------------------------- | ------- |
| `issues` | The amount of issues found |         |


## Performance

On a larger website A11yWatch action runs over 60x-10,000x+ faster depending on CPUs/hardware. After the first installation you should have faster setup time between runs.
You can expect to handle at least 1k pages per minute on a 2-core CPU 7 GB of RAM memory shared github action. If you enable `RECORD` the output time may increase a bit.

When `AI_DISABLED` is set to true the run for `A11yWatch` may increase.

## Common Issues

If you experience issues on your CI you may have to toggle the `UPGRADE` input to true in order to get the latest installs. If you see 
a playwright error try adding `PLAYWRIGHT_VERSION` env variable with the newest version to install chrome.

## CLI

In order to get the results of the run in json run `results_json="$(a11ywatch -r)"` across any step after the action.

## LICENSE

check the license file in the root of the project.
