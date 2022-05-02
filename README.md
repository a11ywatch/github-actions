# A11yWatch Github Action

A GitHub action that runs accessibility and vitals test on your website that goes beyond what a linter can catch. Get actionable results rapidly with feedback posted into your PR. Determine when to fail a pipeline by setting an error limit to help bring an inclusive experience across every commit. This action provides the packed and ready A11yWatch CLI that allows you to bring the entire suite to use during any of your CI steps.

### Usage

```yaml
- uses: a11ywatch/github-action@v1.2.43
  with:
    WEBSITE_URL: ${{ secrets.WEBSITE_URL }}
    FAIL_ERROR_COUNT: 10
```

### Action inputs

All inputs are **optional** except $WEBSITE_URL.

| Name               | Description                                                                                                                                                                                                              | Default        |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------- |
| `WEBSITE_URL`      | Website domain to scan (Start with http or https).                                                                                                                                                                       |                |
| `FAIL_ERROR_COUNT` | Determine whether to fail the CI if issues has errors above theshold..                                                                                                                                                   | 0              |
| `SITE_WIDE`        | Perform a site-wide scan on all pages ( Requires front-end client and authentication to see issues through websockets, ATM does not leave feedback to PR's).                                                             | false          |
| `EXTERNAL`         | Use the A11yWatch remote api for fast results (subjected to rate limits).                                                                                                                                                | false          |
| `DISABLE_PR_STATS` | Prevent messaging to the pr results of test.                                                                                                                                                                             | false          |
| `TOKEN`            | `GITHUB_TOKEN` (permissions `contents: write` and `pull-requests: write`) or a `repo` scoped [Personal Access Token (PAT)](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token). | `GITHUB_TOKEN` |
| `A11YWATCH_TOKEN`  | The A11yWatch api token to use to identify a user.                                                                                                                                                                       |                |

### Action Outputs

| Name      | Description                        | Default |
| --------- | ---------------------------------- | ------- |
| `results` | The results of the report as json. |         |
| `issues`  | The amount of issues found         |         |

An example based on the above reference configuration creates a comment on pull requests that look like this:

![Example](https://raw.githubusercontent.com/A11yWatch/Project-Screenshots/master/gh-action.png?raw=true "A11yWatch Logo")
