# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Context
- **AcumenTouchstone** is the primary project being modified
- Other folders in the Development directory are primarily dependencies
- The codebase is a client-server architecture with C# backend services and JavaScript frontend
- **Main Framework:** Client-side uses CanJS 2.3.35 with jQuery, Bootstrap, and custom components
- **Important:** Claude Code runs in a WSL container, but the projects run on Windows with IIS
- File paths in `/mnt/c/` correspond to `C:\` on the Windows host
- **Directory Navigation:** When working with Git commits or operations, always navigate to the AcumenTouchstone directory
- **Search tools:** Use ripgrep (`rg`) as configured in the user-wide settings

## Build Commands
- Development build: `npm run build:dev`
- Quality Engineering build: `npm run build:qe`
- Performance build: `npm run build:pr`
- Run tests: `npm run test` or `karma start`
- Run single test: `karma start --grep="TestName"`
- Lint JavaScript: `npm run lint` or `esw src`
- Validate all: `gulp validateAll`
- **When asked to compile or build from WSL**: Run `/mnt/c/Windows/System32/cmd.exe /c "cd C:\Development\AcumenTouchstone\code\client\DeltekNavigator\Web && npm run build:local"` to build from WSL to Windows

## Code Style Guidelines
- **C#/.NET**: 
  - Follow .editorconfig conventions: 4-space indentation, CRLF line endings
  - Use PascalCase for classes, methods, properties
  - Prefix interfaces with "I"
  - Organize using statements with System namespaces first
  - Prefer explicit types over var keyword
  
- **JavaScript**:
  - ESLint for code style validation
  - Use JsDoc comments for documentation
  - Follow component patterns in existing code
  - Error handling: Use try/catch for async operations

## Testing
- C#: Use MSTest/NUnit framework with assertion patterns in existing tests
- JavaScript: Jasmine testing framework with Karma test runner

## Work Item Context (PBI / Bug)
- When a **PBI number or bug number** is provided (e.g., `1515203`), Claude should:
  - Retrieve the work item's context using the AzureDevOps Acumen MCP
  - When working on a PBI or bug, check the user's git branch in the AcumenTouchstone directory to find the branch name
  - Branch names are typically prefixed with "PBI/" or "Bug/" followed by the work item number (e.g., "PBI/1515203")
  - Use the branch name to find associated pull request(s) for additional context

## Pull Request
- When a **Pull Request number** is provided (e.g., `162894`), Claude should:
  - Retrieve the code, pull request context using the AzureDevOps Acumen Touchstone MCP
- When responding to PR comments:
  - Check for unresolved comments in the PR using the Azure DevOps MCP
  - Look at relevant files to understand what changes are needed
  - Make the requested changes but allow the user to verify before staging/committing

## CSS Styling
- The project uses SCSS for styling
- Custom styles for specific components are located in `scss` folders within their respective modules
- For example, platform-specific styles are in `/code/client/DeltekNavigator/Web/src/touchstone/app/generalAPI/scss/platforms.scss`
- Components can have custom CSS classes applied via the `additionalCSSClasses` option in component definitions
- **UI Component Structure:**
  - Components use a label wrapper pattern (label.checkbox-field-wrapper, label.radio-group-container)
  - Checkbox components support the `compactHeight: true` option to reduce vertical padding
  - Radio buttons support HORIZONTAL/VERTICAL orientation via RadioGroupField.ORIENTATION
  - Components position is controlled through the combination of colPos/rowPos in layout definitions

## Online Code Context
- Retrieve the code, pull request context using the AzureDevOps Acumen Touchstone MCP

## AcumenTouchstone Web Application Routes
- URL format: `localhost/AcumenTouchstone/app/#!{ApplicationID}/view/{area}/{tab}/{objectID}/{mode}`
- Routes defined in `app_manifest.json` which maps to application JS files
- Example: `#!GeneralSystemAPI/view/0/platforms/Dltk/presentation`
  - `GeneralSystemAPI`: Application ID defined in app_manifest.json
  - `view/0`: Default area
  - `platforms`: Tab ID
  - `Dltk`: Object ID (default record ID)
  - `presentation`: View mode (read-only)
- Key files for routing:
  - `route_manager.js`: Handles URL parsing and generation
  - `general_application.js`: Main handler for GeneralSystemAPI routes
  - `general_layout.js`: Creates layout with tabs
  - `platforms_layout.js`: Defines specific tab components