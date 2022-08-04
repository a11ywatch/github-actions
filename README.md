# github-actions

[![A11yWatchBot](https://github.com/A11yWatch/github-action/actions/workflows/action.yml/badge.svg?branch=main)](https://github.com/A11yWatch/github-action/actions/workflows/action.yml)

A GitHub action that runs multithreaded actionable accessibility reports on your website that is around 100-1000 times faster than the rest.

This action installs the [A11yWatch CLI](https://github.com/A11yWatch/a11ywatch/tree/main/cli) onto your pipeline starting the suite locally or from a remote external connection.

### Usage

```yaml
- uses: a11ywatch/github-action@v1.10.7
  with:
    WEBSITE_URL: ${{ secrets.WEBSITE_URL }}
    FIX: true
    SUBDOMAINS: true
    TLD: true
    FAIL_ERRORS_COUNT: 15
    LIST: true
    UPGRADE: false
    COMPUTER_VISION_SUBSCRIPTION_KEY: ${{ secrets.COMPUTER_VISION_SUBSCRIPTION_KEY }}
    COMPUTER_VISION_ENDPOINT: ${{ secrets.COMPUTER_VISION_SUBSCRIPTION_KEY }}
```

### Action inputs

All inputs are **optional** except $WEBSITE_URL.

| Name                               | Description                                                                                                                                                                                                              | Default        |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------- |
| `WEBSITE_URL`                      | Website domain to scan (Start with http:// or https://).                                                                                                                                                                 |                |
| `SITE_WIDE`                        | Site-wide scanning across all pages.                                                                                                                                                                                     | false          |
| `FIX`                              | Attempt to apply recommendations to code and commit to github.                                                                                                                                                           | false          |
| `SUBDOMAINS`                       | Include all subdomains (required SITE_WIDE=true).                                                                                                                                                                        | true           |
| `TLD`                              | Include all tld extensions (required SITE_WIDE=true).                                                                                                                                                                    | true           |
| `LIST`                             | Report the results to github as a pass or fail list or detailed report.                                                                                                                                                  | false          |
| `FAIL_TOTAL_COUNT`                 | Determine whether to fail the CI if total issues warnings and errors exceed the counter. Takes precedence over the other FAIL inputs.                                                                                    | 0              |
| `FAIL_ERRORS_COUNT`                | Determine whether to fail the CI if total issues with errors exceed the counter.                                                                                                                                         | 0              |
| `FAIL_WARNINGS_COUNT`              | Determine whether to fail the CI if total issues with warnings exceed the counter.                                                                                                                                       | 0              |
| `EXTERNAL`                         | Use the A11yWatch remote API for fast results. If this is set `A11YWATCH_TOKEN` is needed.                                                                                                                               |                |
| `COMPUTER_VISION_SUBSCRIPTION_KEY` | [Computer Vision](https://azure.microsoft.com/en-us/services/cognitive-services/computer-vision/#overview) API key for image recognition on alts.                                                                        |                |
| `COMPUTER_VISION_ENDPOINT`         | Computer Vision url endpoint.                                                                                                                                                                                            | false          |
| `DISABLE_PR_STATS`                 | Prevent messaging to the pr results of test.                                                                                                                                                                             | false          |
| `TOKEN`                            | `GITHUB_TOKEN` (permissions `contents: write` and `pull-requests: write`) or a `repo` scoped [Personal Access Token (PAT)](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token). | `GITHUB_TOKEN` |
| `A11YWATCH_TOKEN`                  | The A11yWatch API token to use to identify a user.                                                                                                                                                                       |                |
| `UPGRADE`                          | Upgrade the docker images before testing to latest.                                                                                                                                                                      |                |

### Action Outputs

| Name      | Description                        | Default |
| --------- | ---------------------------------- | ------- |
| `results` | The results of the report as json. |         |
| `issues`  | The amount of issues found         |         |

## Benches

Using `https://a11ywatch.com` as test website that is fully static we compared our tool against the most popular tool Pa11y-CI.

Currently it takes our github action a bit longer to start if its a new run since we need to compile a couple of native tools before we can start.

```sh
----------------------
linux ubuntu-latest
2-core CPU
7 GB of RAM memory
14 GB of SSD disk space
-----------------------
```

### Benchmark Results

```sh
Test url: `https://a11ywatch.com`

25 pages
```

### crawl-speed

runs with 10 samples:

|                                   | `libraries`           |
| :-------------------------------- | :-------------------- |
| **`A11yWatch: crawl 10 samples`** | `5 s` (✅ **1.00x**)  |
| **`Pa11y-CI: crawl 10 samples`**  | `45 s` (✅ **1.00x**) |

### crawl-speed (Large Website)

```sh
Test url: `https://www.hbo.com`

30,000 pages.
```

On a larger website our parallel native action runs over 10,000x faster.

runs with 10 samples:

|                                   | `libraries`              |
| :-------------------------------- | :----------------------- |
| **`A11yWatch: crawl 10 samples`** | `20 mins` (✅ **1.00x**) |
| **`Pa11y-CI: crawl 10 samples`**  | `6 hr` (✅ **1.00x**)    |

Pa11y-CI actually could not finish the crawl as it exceeds the github action free limits.

## Common Issues

If you experience issues on your CI you may have to set the `UPGRADE` input to true in order to get the latest docker images.
We need docker in order to build the appliciation in quickly since we have some services that need to compile that may take awhile.

## LICENSE

check the license file in the root of the project.
