# github-action

[![Tests](https://github.com/a11ywatch/github-actions/actions/workflows/action.yml/badge.svg)](https://github.com/a11ywatch/github-actions/actions/workflows/action.yml)

A feature rich GitHub action that runs actionable accessibility reports on your website that can handle large workloads.

Some of the primary features include pass/fail testing, code fixes, and detailed reports.

When running locally the action uses A11yWatch Lite and installs the [a11ywatch-cli](https://github.com/a11ywatch/a11ywatch).

### Usage

```yaml
- uses: a11ywatch/github-action@v2.1.9
  with:
    WEBSITE_URL: ${{ secrets.WEBSITE_URL }}
    SITE_WIDE: true
    SUBDOMAINS: true
    TLD: false
    SITEMAP: true
    FAIL_ERRORS_COUNT: 15
    LIST: true
    FIX: false
    UPGRADE: false
    UPLOAD: true
    COMPUTER_VISION_SUBSCRIPTION_KEY: ${{ secrets.COMPUTER_VISION_SUBSCRIPTION_KEY }}
    COMPUTER_VISION_ENDPOINT: ${{ secrets.COMPUTER_VISION_ENDPOINT }}
  env:
    DEFAULT_RUNNERS: htmlcs,axe
    PAGEMIND_IGNORE_WARNINGS: true
    AI_DISABLED: false
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
| `FAIL_TOTAL_COUNT`                 | Determine whether to fail the CI if total issues warnings and errors exceed the counter. Takes precedence over the other FAIL inputs. Set to a value greater than 0 for CI failure.                                      | 0              |
| `FAIL_ERRORS_COUNT`                | Determine whether to fail the CI if total issues with errors exceed the counter. Set to a value greater than 0 for CI failure.                                                                                           | 0              |
| `FAIL_WARNINGS_COUNT`              | Determine whether to fail the CI if total issues with warnings exceed the counter. Set to a value greater than 0 for CI failure.                                                                                         | 0              |
| `EXTERNAL`                         | Use the A11yWatch remote API for fast results. If this is set `A11YWATCH_TOKEN` is needed.                                                                                                                               |                |
| `COMPUTER_VISION_SUBSCRIPTION_KEY` | [Computer Vision](https://azure.microsoft.com/en-us/services/cognitive-services/computer-vision/#overview) API key for image recognition on alts.                                                                        |                |
| `COMPUTER_VISION_ENDPOINT`         | Computer Vision url endpoint.                                                                                                                                                                                            | false          |
| `DISABLE_PR_STATS`                 | Prevent messaging to the pr results of test.                                                                                                                                                                             | false          |
| `TOKEN`                            | `GITHUB_TOKEN` (permissions `contents: write` and `pull-requests: write`) or a `repo` scoped [Personal Access Token (PAT)](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token). | `GITHUB_TOKEN` |
| `A11YWATCH_TOKEN`                  | The A11yWatch API token to use to identify a user.                                                                                                                                                                       |                |
| `SLIM`                             | Use the gRPC client to gather reports - only displays stats, useful for large websites (no code generation, no outputs, just pure stats)                                                                                 | false          |
| `UPGRADE`                          | Upgrade the docker images before testing to latest.                                                                                                                                                                      | false          |
| `UPLOAD`                           | Upload the data as an artifact to get better insight.                                                                                                                                                                    | false          |

### Action Outputs

| Name      | Description                       | Default |
| --------- | --------------------------------- | ------- |
| `issues`  | The amount of issues found.       |         |
| `results` | The results of the audit as json. |         |

### ENV Variables

Configure crawl wide settings for A11yWatch Lite (local) installs.

| Name                       | Description                                                                                           | Default      |
| -------------------------- | ----------------------------------------------------------------------------------------------------- | ------------ |
| `DEFAULT_RUNNERS`          | A comma separeted list of runners to use in Litemode for testing like `axe` and `htmlcs`.         | `htmlcs,axe` |
| `PAGEMIND_IGNORE_WARNINGS` | Enable to ignore all warnings from output. This could speed up runs and save on audit size.           | false        |
| `AI_DISABLED`              | Disable AI use to get missing resource props like alts. If enabled the speed may increase of the run. | false        |

If you set it to only `htmlcs` you will have really fast crawls with good coverage with our fork.

## Performance

On a larger website A11yWatch action runs over 60x-10,000x+ faster depending on CPUs/hardware. After the first installation you should have faster setup time between runs.
You can expect to handle at least 1k pages per minute on a 2-core CPU 7 GB of RAM memory shared github action. If you enable `RECORD` the output time may increase a bit.

## Common Issues

If you experience issues on your CI you may have to toggle the `UPGRADE` input to true in order to get the latest installs. If you see
a playwright error try adding `PLAYWRIGHT_VERSION` env variable with the newest version to install chrome.

## LICENSE

check the license file in the root of the project.
