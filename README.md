# Extract Jira Ticket ID Action

This GitHub Action extracts a Jira Ticket ID either from the PR title or the branch name. It's useful for automating workflows that require a Jira Ticket ID.

## Ticket ID Format
Make sure to prefix the PR title or the branch with the following format: `[A-Z]-[0-9]`  (capital letters, dash, numbers)  
Examples:  
`PRJ-123/my-feature-branch`  
`[PRJ-123] This is the PR title`

_anything before or after the correct format is valid_

## Inputs

This action currently does not require any inputs.

## Outputs

`jira_ticket_id`: The extracted Jira Ticket ID.  
This output can be accessed using `steps.<step_id>.outputs.jira_ticket_id`, where `<step_id>` is the ID of the step that ran this action.

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
      uses: geokats7/extract-jira-id-action@v1
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
      uses: geokats7/extract-jira-id-action@v1
    outputs:
      jira_ticket_id: ${{ steps.extract_id.outputs.jira_ticket_id }}

  use:
    needs: extract
    runs-on: ubuntu-latest
    steps:
    - name: Use Jira ID
      run: echo "Jira Ticket ID is ${{ needs.extract.outputs.jira_ticket_id }}"

# TEST