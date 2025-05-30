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

## UI Framework Architecture
- **PPMUI** is the core UI framework that AcumenTouchstone extends and customizes
- Layout definitions are created using the following pattern:
  - TabDefinition creates tabs with ID, heading, and sequence
  - ComponentDefinition adds components to tabs with type, position, and options
  - Components are positioned in a grid using colPos (column) and rowPos (row)
  - ComponentDefinition.TYPES defines all valid component types (CHECKBOX, TEXT_BLOCK, etc.)
- **Responsive Design Approach:**
  - Components maintain their grid positions in the layout definition
  - CSS media queries are used to control responsive behavior
  - Components sharing the same colPos stack vertically
  - There is no built-in container component - visual grouping is achieved through:
    1. Sharing the same colPos with sequential rowPos values
    2. Using common CSS classes applied via additionalCSSClasses
    3. Using TEXT_BLOCK components as section headers
- **Common Component Patterns:**
  - Section headers use TEXT_BLOCK with HTML content
  - Groups of related checkboxes share the same colPos with different rowPos
  - DIVIDER components (type: ComponentDefinition.TYPES.DIVIDER) are used to separate sections
  - Static links (type: ComponentDefinition.TYPES.STATIC_LINK) for actions

## PPMUI Component System
- **Component Architecture:**
  - All components extend `BaseComponent` class
  - Components support three main modes: presentation, edit, and design
  - EJS templates are used for different view modes
  - Component Manager handles component registration and instantiation
  
- **Available Component Types:**
  - Basic Fields: STRING, NUMBER, CHECKBOX, RADIO, DROPDOWN
  - Special Fields: DATE, DATETIME, PASSWORD, URL, PHONE
  - Layout Elements: DIVIDER, TEXT_BLOCK, STATIC_LINK
  - Complex Components: GRID, TREE, TIMELINE
  
- **Common Component Options:**
  - `label`: Display label for the component
  - `labelPosition`: Position of label (TOP, LEFT, NONE)
  - `tooltip`: Help text shown on hover
  - `isRequired`: Whether the field is required
  - `additionalCSSClasses`: Custom styling classes
  - `compactHeight`: Reduces vertical padding (checkboxes)
  
- **Component Implementation Pattern:**
  1. Create layout definition with tabs
  2. Add components to tabs with proper positioning
  3. Configure component options
  4. Register event handlers in application

- **Grid System:**
  - Components positioned with colPos and rowPos (1-based, not 0-based)
  - Components can span multiple columns using colWidth
  - Components sharing the same colPos stack vertically
  - Visual grouping through sequential rowPos values

- **Example Component Usage:**
```javascript
layoutdef.addComponent(new ComponentDefinition({
    id: "MyComponent.Id",
    tabID: "myTabId",
    label: i18n("Translation.Key"),
    colPos: 1,
    rowPos: 2,
    type: ComponentDefinition.TYPES.STRING,
    otherOptions: {
        isRequired: true,
        maxLength: 100
    }
}));
```

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