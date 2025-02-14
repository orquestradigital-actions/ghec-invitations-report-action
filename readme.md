# ghec-invitations-report-action

> GitHub Action to create a report of GitHub Enterprise Cloud invitations

[![Test](https://github.com/ActionsDesk/ghec-invitations-report-action/workflows/Test/badge.svg)](https://github.com/ActionsDesk/ghec-invitations-report-action/actions?query=workflow%3ATest) [![styled with prettier](https://img.shields.io/badge/styled_with-prettier-ff69b4.svg)](https://github.com/prettier/prettier)

## Usage

**Scheduled report example**

```yml
on:
  schedule:
    # Runs at 00:00 UTC on the first of every month
    - cron: '0 0 1 * *'

name: Scheduled invitations report

jobs:
  report:
    runs-on: ubuntu-latest

    steps:
      - name: Create invitations report
        uses: ActionsDesk/ghec-invitations-report-action@main
        with:
          token: ${{ secrets.ADMIN_TOKEN }}
          enterprise: 'my-enterprise'
          report-path: 'reports/enterprise-invitations.csv'
```

<details>
  <summary><strong>On-demand report example</strong></summary>

```yml
on:
  workflow_dispatch:
    inputs:
      enterprise:
        description: 'GitHub Enterprise Cloud account, if omitted the report will target the repository organization only'
        required: false
        default: 'my-enterprise'
      report-path:
        description: 'Path to the report file'
        default: 'reports/invitations.csv'
        required: false

name: Invitations

jobs:
  report:
    runs-on: ubuntu-latest

    steps:
      - name: Create invitations report
        uses: ActionsDesk/ghec-invitations-report-action@main
        with:
          token: ${{ secrets.ADMIN_TOKEN }}
          enterprise: ${{ github.event.inputs.enterprise }}
          report-path: ${{ github.event.inputs.report-path }}
```

</details>

### Action Inputs

| Name             | Description                                                                                                                               | Default                   | Required |
| :--------------- | :---------------------------------------------------------------------------------------------------------------------------------------- | :------------------------ | :------- |
| `token`          | A `admin:org`, `read:user`, `repo`, `user:email` scoped [PAT]                                                                             |                           | `true`   |
| `report-path`    | Path within the repository to create the report CSV file                                                                                  | `invitation-report.csv`   | `false`  |
| `enterprise`     | GitHub Enterprise Cloud account, will require `admin:org`, `read:enterprise`, `read:user`, `repo`, `user:email` scoped [PAT] for `token`. |                           | `false`  |
| `committer-name` | The name of the committer to be displayed in the history                                                                                  | `invitation-reporter[bot]`| `false`  |
| `committer-email`| The email of the committer to be displayed in the history                                                                                 | `invitation@reporter`     | `false`  |

Note: If the `enterprise` input is omitted, the report will only be created for the organization the repository belongs to.

### Action Outputs

| Name       | Description                                                                            |
| :--------- | :------------------------------------------------------------------------------------- |
| `base-sha` | Report base SHA, needed for https://github.com/ActionsDesk/ghec-report-reinvite-action |
| `head-sha` | Report head SHA, needed for https://github.com/ActionsDesk/ghec-report-reinvite-action |

## License

- [MIT License](./license)

[pat]: https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token 'Personal Access Token'
