# Azure DevOps PR Comments Command

This command fetches active comments for a specified Pull Request ID in Azure DevOps.

## IMPORTANT INSTRUCTIONS FOR CLAUDE

- This command already handles the API call to fetch PR comments
- DO NOT use Azure DevOps MCP tools to fetch PR comments when this command is used
- Simply execute the curl command that's generated from this template
- The command uses stored credentials from ~/.env

## Example Usage

```
Run the command with a PR ID:
/command project:devops-pr-comments 163482

This will execute:
source ~/.env && curl -s -u :"$AZURE_DEVOPS_PAT" "https://tfs.deltek.com/tfs/Deltek/_apis/git/repositories/f7dcee47-fb25-43fa-b9c9-c67056eb87bb/pullRequests/163482/threads?api-version=7.1"  < /dev/null |  jq '.value[] | select(.status == "active")'
```

## Output Format

The command outputs JSON data for each active comment, including:
- File path and line number
- Comment author
- Comment content
- Thread status

To get context about the changed file, go to the AcumenTouchstone Directory and get the diff from the local master branch
