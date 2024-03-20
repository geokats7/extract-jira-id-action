# Extract Jira Ticket ID Action

This GitHub Action extracts a Jira Ticket ID from a PR title, branch name, or commit messages. It's useful for automating workflows that require a Jira Ticket ID.

## Inputs

This action currently does not require any inputs.

## Outputs

`jira_ticket_id`: The extracted Jira Ticket ID. This output can be accessed using `steps.<step_id>.outputs.jira_ticket_id`, where `<step_id>` is the ID of the step that ran this action.

This action also sets an environment variable `JIRA_TICKET_ID` with the extracted Jira Ticket ID.

## Usage

You can use this action by referencing it in your workflow file with the `uses` keyword.  
Let's see a couple of examples:

### Within the same job

```yaml
name: Extract and Use Jira ID
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Extract Jira ID
      id: extract_id
      uses: your-github-username/extract-jira-id-action@v1
    - name: Use Jira ID
      run: echo "Jira Ticket ID is ${{ steps.extract_id.outputs.jira_ticket_id }}"
```

### Across different jobs

```yaml
name: Extract and Use Jira ID
on: [push, pull_request]

jobs:
  extract:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Extract Jira ID
      id: extract_id
      uses: your-github-username/extract-jira-id-action@v1
    outputs:
      ticket_id: ${{ steps.extract_id.outputs.ticket_id }}

  use:
    needs: extract
    runs-on: ubuntu-latest
    steps:
    - name: Use Jira ID
      run: echo "Jira Ticket ID is ${{ needs.extract.outputs.ticket_id }}"


TEST!!!!@@@
