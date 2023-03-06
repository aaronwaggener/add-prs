# Add User PRs to a Project
This GitHub Action adds all open pull requests in an organization that are authored by at least one of the supplied usernames to a project board.

## Inputs
Input Name | Required | Details 
:-|:-:|:-
`organization` | | The URL friendly name of the organization (or repository owner) where the project board is located. If not specified, the action will default to the current organization where the action is being run.
`project-id` | :heavy_check_mark: | The numerical ID of the project where the pull requests will be added. This ID is typically found in the project URL "[organization-name]/projects/[*project-id*]"
`usernames` | :heavy_check_mark: | The GitHub usernames whose open pull requests will be added to the project board. The usernames should be separated by spaces. For example: octocat mona-lisa

## Environment Variables
This action uses the `gh` CLI tool to make graphql queries. In order for `gh` to authenticate properly, you need to create a Personal Access Token with `project`, `read:org`, and `repo` scopes. After generating the token, add it repository secrets, and then reference it in your actions workflow.
```
env:
  GH_TOKEN: ${{ secrets.GH_TOKEN }}
```


## How to Use
To use this action, include the following code in your workflow file:

```
- name: Add User PRs to a Project
  uses: aaronwaggener/add-prs
  with:
    usernames: octocat mona-lisa
    project-id: [project ID]
    organization: [organization name]
  env:
    GH_TOKEN: ${{ secrets.GH_TOKEN }}
```

A full workflow example that checks for new pull requests every 20 minutes

```
name: Add team pull requests to the project board

on:
  schedule:
    - cron: '*/20 * * * *'

jobs:
  add-prs:
    runs-on: ubuntu-latest
    steps:
      - name: Add the PRs to the project
        uses: aaronwaggener/add-prs
        with:
          usernames: octocat mona-lisa
          project-id: 123
          organization: org-name
        env:
          GH_TOKEN: ${{ secrets.GH_TOKEN }}
```

## Triggers
This action was designed to run a `schedule`, but it can be used in any workflow regardless of the event trigger.
