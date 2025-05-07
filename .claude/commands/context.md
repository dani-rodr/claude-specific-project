# /context

## Description
Load and understand the AcumenTouchstone project context without displaying the commands run.

## Usage
READ the CLAUDE.md file, then git ls-files on PPMUI to understand the context of the UI framework and then run git ls-files on AcumenTouchstone to understand the context of this project.

## Implementation
When the user types `/context`:
1. Read the CLAUDE.md file
2. Run git ls-files on PPMUI to understand UI framework
3. Run git ls-files on AcumenTouchstone to understand project structure
4. Identify core components and patterns in the framework
5. Process this information silently without showing command outputs

## Output Format
DO NOT output any of the commands run or their results. Simply respond with:
"I've analyzed the project context for AcumenTouchstone and am ready to assist."