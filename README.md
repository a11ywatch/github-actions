# A11yWatch Github Action

A GitHub action that runs accessibility and vitals test on your website that goes beyond what a linter can catch.

The reports give detailed insight on WCAG2.1+ including other web accessibility issues with recommendations like missing alts (using machine learning and AI), color contrast, and much more.

Get actionable results rapidly with feedback posted into your PR.

Determine when to fail a pipeline by setting an error limit to help bring an inclusive experience across every commit.

### Usage

```yaml
- uses: a11ywatch/github-action@v1.5.1
  with:
    WEBSITE_URL: ${{ secrets.WEBSITE_URL }}
    FAIL_TOTAL_COUNT: 10
```

### Action inputs

All inputs are **optional** except $WEBSITE_URL.

| Name                  | Description                                                                                                                                                                                                              | Default        |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------- |
| `WEBSITE_URL`         | Website domain to scan (Start with http:// or https://).                                                                                                                                                                 |                |
| `SITE_WIDE`           | Site-wide scanning across all pages.                                                                                                                                                                                     | true           |
| `FAIL_TOTAL_COUNT`    | Determine whether to fail the CI if total issues warnings and errors exceed the counter. Takes precedence over the other FAIL inputs.                                                                                    | 0              |
| `FAIL_ERRORS_COUNT`   | Determine whether to fail the CI if total issues with errors exceed the counter.                                                                                                                                         | 0              |
| `FAIL_WARNINGS_COUNT` | Determine whether to fail the CI if total issues with warnings exceed the counter.                                                                                                                                       | 0              |
| `EXTERNAL`            | Use the A11yWatch remote api for fast results. If this is set `A11YWATCH_TOKEN` is needed.                                                                                                                               | false          |
| `DISABLE_PR_STATS`    | Prevent messaging to the pr results of test.                                                                                                                                                                             | false          |
| `TOKEN`               | `GITHUB_TOKEN` (permissions `contents: write` and `pull-requests: write`) or a `repo` scoped [Personal Access Token (PAT)](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token). | `GITHUB_TOKEN` |
| `A11YWATCH_TOKEN`     | The A11yWatch api token to use to identify a user.                                                                                                                                                                       |                |

### Action Outputs

| Name      | Description                        | Default |
| --------- | ---------------------------------- | ------- |
| `results` | The results of the report as json. |         |
| `issues`  | The amount of issues found         |         |

An example based on the above reference configuration creates a comment on pull requests that look like this:

![Example](https://raw.githubusercontent.com/A11yWatch/Project-Screenshots/master/gh-action.png?raw=true "A11yWatch Logo")
