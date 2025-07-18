# Contributing to CopyKit

We welcome contributions! Please follow these guidelines to ensure a smooth collaboration process.

## Commit Messages

This project uses [Conventional Commits](https://www.conventionalcommits.org/) with commitlint enforcement.

**Valid scopes** (enforced by commitlint):
- `docs`: Documentation changes
- `pages`: Changes to page templates
- `scripts`: Changes to CLI scripts and development tools
- `stub:base`: Changes to base components and utilities
- `stub:[copy-point-name]`: Changes to specific copy points (e.g., `stub:advanced`, `stub:animations`)

**Common commit types**:
- `feat`: New features
- `fix`: Bug fixes
- `docs`: Documentation updates
- `style`: Code style changes (formatting, etc.)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

**Examples**:
```bash
git commit -m "feat(pages): add new themed page template"
git commit -m "fix(stub:base): resolve expand animation timing issue"
git commit -m "feat(stub:advanced): add new advanced button variants"
git commit -m "feat(scripts): add copykit CLI for copy-point distribution"
git commit -m "feat(scripts): add info command for detailed copy-point information"
git commit -m "fix(scripts): resolve CLI TypeScript compilation errors"
git commit -m "docs(scripts): add CLI development and testing guide"
git commit -m "docs: update component usage examples"
git commit -m "style(stub:base): format CSS utility classes"
git commit -m "chore: update dependencies"
```

## Development Process

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes following the project's code style
4. Ensure all linting and formatting passes (`pnpm run lint` and `pnpm run format`)
5. Commit your changes with conventional commit messages
6. Push to the branch (`git push origin feature/amazing-feature`)
7. Open a Pull Request

## Code Quality Standards

Before submitting a pull request, ensure your code meets these standards:

### Linting and Formatting
- Run `pnpm run lint` to check for linting issues
- Run `pnpm run lint:fix` to automatically fix all linting issues
- Run `pnpm run lint:js` to check JavaScript/TypeScript files only
- Run `pnpm run lint:js:fix` to automatically fix JavaScript/TypeScript linting issues
- Run `pnpm run lint:css` to check CSS files only
- Run `pnpm run lint:css:fix` to automatically fix CSS linting issues
- Run `pnpm run format` to format code with Prettier
- Run `pnpm run format:check` to verify formatting
- Run `pnpm run format:docs` to format README.md and CHANGELOG.md files
- Run `pnpm run format:json` to format tsconfig.json files
- Run `pnpm run format:package` to format package.json file

### Code Style
- Follow TypeScript strict mode conventions
- Use meaningful variable and function names
- Add JSDoc comments for public APIs
- Ensure proper ARIA attributes for accessibility
- Use CSS custom properties for theming

### Testing
- Run `pnpm test` to execute the full test suite
- Run `pnpm run test:coverage` to generate coverage reports
- Run `pnpm run test:ui` to use the interactive Vitest UI
- Run `pnpm run test:watch` to run tests in watch mode during development
- Ensure your changes don't break existing functionality
- Add tests for new features following the naming conventions
- Test across different browsers if making UI changes
- Verify accessibility compliance

#### Test File Naming Convention
Follow this strict naming convention for all test files:

**Unit Tests**: `[name].test.ts`
- Examples: `cookie.test.ts`, `expand.test.ts`, `accordion.test.ts`
- For testing individual functions, classes, or components

**Accessibility Tests**: `[name].accessibility.test.ts`
- Examples: `accordion.accessibility.test.ts`, `aria-fundamentals.accessibility.test.ts`
- For testing WCAG compliance, ARIA attributes, keyboard navigation, and axe-core scanning

#### Testing Structure
```
test/                                    # Global test utilities and infrastructure tests
├── setup.ts                           # Test environment configuration
├── utils.ts                           # Shared test utilities and helpers
├── aria-fundamentals.accessibility.test.ts    # Basic ARIA and accessibility tests
└── axe-core-integration.accessibility.test.ts # Automated accessibility scanning tests

stubs/_base/scripts/
├── utilities/
│   ├── cookie.test.ts                  # Unit tests for cookie utilities
│   ├── dom.test.ts                     # Unit tests for DOM utilities
│   ├── event-emitter.test.ts          # Unit tests for EventEmitter class
│   └── select.test.ts                  # Unit tests for selector utilities
└── services/
    └── expand.test.ts                  # Unit tests for expand service

stubs/accordion/scripts/services/
├── accordion.test.ts                   # Unit tests for accordion functionality
└── accordion.accessibility.test.ts    # Accessibility tests for accordion component
```

#### Test Coverage Requirements
- **Unit Tests**: Test all public APIs, edge cases, error handling
- **Accessibility Tests**: Verify WCAG compliance, ARIA attributes, keyboard navigation
- **Integration Tests**: Test component interactions and event handling
- **Automated Scanning**: Use axe-core for automated accessibility violation detection

#### Writing Tests
```typescript
// Unit test example
import { describe, it, expect } from 'vitest'
import { cookieRead, cookieWrite } from './cookie'

describe('Cookie Utilities', () => {
  it('should read and write cookies correctly', () => {
    cookieWrite('test', 'value')
    expect(cookieRead('test')).toBe('value')
  })
})

// Accessibility test example  
import { describe, it, expect } from 'vitest'
import { expectAccessible, createAccordionElement } from '@test/utils'

describe('Accordion Accessibility', () => {
  it('should pass axe-core accessibility scan', async () => {
    const accordion = createAccordionElement({ itemCount: 3 })
    await expectAccessible(accordion)
  })
})
```

## Project Structure Guidelines

When contributing, follow these structural guidelines:

### CSS Architecture
- Place styles in the appropriate layer (`01_defaults`, `02_components`, `03_utilities`, `04_layouts`)
- **Always use colorset variables** instead of direct color values for consistency and theme switching
- Use CSS custom properties for theming
- Follow the existing naming conventions
- Document new CSS variables with JSDoc comments

### Colorset Usage Guidelines
When working with colors in CSS, always use the colorset system:

**✅ Correct - Use colorset variables:**
```css
.button {
  color: var(--font-color);
  background-color: var(--accent-bg-color);
  border: 1px solid var(--border-color);
}

.button:hover {
  color: var(--accent-hover-font-color);
  background-color: var(--accent-hover-bg-color);
}
```

**❌ Incorrect - Direct color values:**
```css
.button {
  color: #000000;
  background-color: #cccccc;
  border: 1px solid #000000;
}
```

**Colorset Benefits:**
- Consistent theming across components
- Easy theme switching (light/dark modes)
- Systematic color relationships
- Maintainable color management

### CSS Documentation Guidelines
CSS documentation should only be added where it provides meaningful value for component showcases:

**When TO add CSS documentation:**
- **Main component**: Document the base component with basic usage example
- **Component variations**: Document meaningful variants that users would want to see showcased (e.g., `.accordion.flush`, `.btn.primary`)
- **Interactive features**: Document special interactive behaviors (e.g., `.control.chevron`, `.accordion.single-select`)
- **Animation variants**: Document components with special animation features
- **Complex usage patterns**: Document components that require specific HTML structure or data attributes

**When NOT to add CSS documentation:**
- **Basic structural elements**: Don't document simple containers or wrappers (e.g., `.accordion > .item`)
- **Helper classes**: Don't document basic utility or helper classes
- **State classes**: Don't document simple state classes like `:hover`, `:focus`, `[hidden]`
- **Internal implementation**: Don't document CSS that's purely for internal styling

**Documentation format:**
```css
/**
 * Component description focusing on when/why to use this variant
 *
 * @location components.accordion.flush Flush Accordion
 * @example
 * <div class="accordion flush">
 *   <div class="item">
 *     <button class="control">Header</button>
 *     <div class="content" hidden>Content</div>
 *   </div>
 * </div>
 */
.accordion.flush {
  /* styles */
}
```

**Key principles:**
- Each documented example should show a **complete, functional component**
- Focus on **variants and behaviors** that users need to understand
- Keep examples **minimal but complete** (show full structure, not fragments)
- Use **descriptive location paths** that group related variants
- **CRITICAL**: Each `@location` key must be **unique across the entire project** - never reuse the same location key twice

**@location Key Guidelines:**
```css
/* ✅ CORRECT - Unique location keys */
@location components.accordion Accordion
@location components.accordion.single Single-select Accordion
@location components.accordion.chevron Accordion with Chevron
@location components.button Button
@location components.button.primary Primary Button

/* ❌ INCORRECT - Duplicate location keys */
@location components.accordion Accordion
@location components.accordion Basic Accordion  // Duplicate key!
```

**Location key naming rules:**
- Use **dot notation** to create hierarchical organization (e.g., `components.accordion.single`)
- Keep keys **descriptive but concise**
- Follow pattern: `category.component.variant` (e.g., `components.button.primary`)
- **Never reuse** the same key, even across different files

### JavaScript/TypeScript
- Place reusable utilities in `stubs/_base/scripts/utilities/`
- Place feature services in `stubs/_base/scripts/services/`
- Use the `@/` alias for imports from `stubs/`
- Follow existing patterns for DOM manipulation and event handling

### HTML Templates
- Use semantic HTML elements
- Include proper ARIA attributes
- Follow accessibility best practices
- Use the established CSS class naming patterns

## Creating New Copy Points

CopyKit is organized around copy points that users can copy into their projects. Here's how to create a new copy point:

### Copy Point Structure

All copy points (except `_base`) follow this naming and structure:

```
stubs/
├── _base/                   # Foundation copy point (with underscore)
└── [new-copy-point]/        # New copy points (no underscore)
    ├── copy-point.json      # Metadata for CLI discovery and tooling
    ├── README.md            # Complete documentation
    ├── scripts/
    │   ├── services/        # Interactive functionality
    │   └── utilities/       # Helper functions
    └── styles/
        ├── 01_defaults/     # Foundation extensions/overrides
        ├── 02_components/   # New or modified components
        ├── 03_utilities/    # New or modified utilities
        └── 04_layouts/      # New or modified layouts
```

### Guidelines for New Copy Points

1. **Naming Convention**:
   - Use descriptive names: `advanced`, `animations`, `components-extended`
   - No underscores (only `_base` has an underscore)
   - Use kebab-case for multi-word names

2. **Content Strategy**:
   - **Extend, don't replace**: Build upon `_base` foundation
   - **Layer-based organization**: Follow the CSS layer structure
   - **Modular approach**: Users should be able to copy individual files
   - **No index.css**: Only `_base` includes the main `index.css` file

3. **CSS Layer Guidelines**:
   - `01_defaults/`: Extend or override base variables and defaults
   - `02_components/`: New components or enhanced versions of base components
   - `03_utilities/`: Additional utility classes or enhanced base utilities
   - `04_layouts/`: New layout patterns or modifications

4. **JavaScript Guidelines**:
   - `services/`: New interactive functionality or enhanced services
   - `utilities/`: Additional helper functions or enhanced base utilities
   - Maintain compatibility with base utilities when possible

5. **Copy Point Metadata** (`copy-point.json`):
   - Every copy point must include a metadata file for CLI discovery
   - Follow the standard JSON schema with required fields
   - Keep metadata accurate and up-to-date with actual features

### Copy Point Metadata System

Each copy point requires a `copy-point.json` file that provides structured metadata for the CopyKit CLI and other tools. This enables dynamic discovery and rich information display.

**Required Metadata Schema**:
```json
{
  "name": "copy-point-name",
  "title": "Human-readable Title",
  "description": "Detailed description of features and purpose",
  "version": "1.0.0",
  "features": [
    "List of key features",
    "What this copy point provides",
    "Technical capabilities"
  ],
  "dependencies": [],
  "author": "CopyKit",
  "keywords": ["relevant", "search", "terms"]
}
```

**CLI Integration**:
- `copykit list` - Shows name, title, and description for quick overview
- `copykit info <name>` - Shows complete metadata including features, version, keywords
- Automatic discovery by CLI tools for enhanced user experience

**Metadata Guidelines**:
- **Name**: Must match the directory name exactly
- **Title**: Descriptive, human-readable name for display
- **Description**: Clear explanation of what the copy point provides
- **Features**: Specific, user-focused list of what the copy point offers
- **Dependencies**: Usually empty (since _base is always required first)
- **Keywords**: Relevant terms for searchability and categorization

**Quality Standards**:
- Keep descriptions concise but informative
- List features from user perspective, not implementation details
- Use consistent terminology across all copy points
- Update metadata when adding or removing features

### CopyKit CLI Commands

Contributors should be familiar with the CLI commands for testing and validation:

**Core Commands**:
```bash
copykit list                    # List all available copy-points with basic info
copykit info <copy-point>       # Show detailed information about a specific copy-point
copykit init                    # Initialize project with _base copy-point
copykit add <copy-point>        # Add specific copy-point to project
copykit help                    # Show general help and command overview
```

**Testing Copy Points**:
```bash
# Test copy point discovery and metadata loading
copykit list
copykit info your-copy-point

# Test copy operations in temporary directory
mkdir test-copy-point && cd test-copy-point
copykit init
copykit add your-copy-point
cd .. && rm -rf test-copy-point
```

### Automated Script Benefits

The `create-copy-point` script provides several advantages:

- **Consistent Structure**: Automatically creates the standard directory layout
- **Template Files**: Generates starter files with proper JSDoc comments and structure
- **Validation**: Ensures naming conventions are followed (no underscores, kebab-case)
- **Error Prevention**: Checks if copy point already exists and validates input
- **Guidance**: Provides clear next steps after creation

### Step-by-Step Process

1. **Plan Your Copy Point**:
   - Define the purpose and scope
   - Determine what components/utilities it will provide
   - Ensure it complements existing copy points

2. **Generate with Script** (Recommended):
   ```bash
   # Create a new copy point with proper structure and templates
   pnpm run create-copy-point [copy-point-name]
   
   # Examples:
   pnpm run create-copy-point advanced
   pnpm run create-copy-point dark-theme
   pnpm run create-copy-point components-extended
   ```
   
   **Alternative: Manual Creation**:
   ```bash
   mkdir -p stubs/[copy-point-name]/scripts/{services,utilities}
   mkdir -p stubs/[copy-point-name]/styles/{01_defaults,02_components,03_utilities,04_layouts}
   # Manually create copy-point.json and README.md files
   ```

3. **Complete Metadata**:
   - Update the generated `copy-point.json` with accurate information
   - Write clear, user-focused features and description
   - Choose relevant keywords for discoverability
   - Verify the metadata accurately reflects the copy point's capabilities

4. **Develop Components**:
   - Follow existing naming conventions
   - Use CSS custom properties for theming
   - Ensure accessibility compliance
   - Add JSDoc comments for documentation

5. **Test Integration**:
   - Test copying individual files
   - Test copying the entire copy point
   - Test mixing with `_base` and other copy points
   - Verify CSS layer cascading works properly
   - Test CLI discovery: `copykit list` and `copykit info your-copy-point`

6. **Create Examples**:
   - Add demonstration pages in `pages/`
   - Show how components work with the `@/` alias
   - Document usage patterns

7. **Update Documentation**:
   - Add copy point to README.md
   - Update UI-Doc comments
   - Add examples and usage notes

### Commit Scope for New Copy Points

When creating a new copy point, use the appropriate commit scope:

```bash
# Creating a new copy point
git commit -m "feat(stub:[copy-point-name]): create [copy-point-name] copy point"

# Adding components to a copy point
git commit -m "feat(stub:[copy-point-name]): add advanced button variants"

# Fixing issues in a copy point
git commit -m "fix(stub:[copy-point-name]): resolve animation timing issue"
```

### Quality Checklist

Before submitting a new copy point:

- [ ] Follows the standard directory structure
- [ ] Includes accurate `copy-point.json` metadata file
- [ ] No `index.css` file (only in `_base`)
- [ ] All CSS uses appropriate layers
- [ ] JavaScript follows existing patterns
- [ ] Components are accessible (ARIA compliant)
- [ ] JSDoc comments are complete
- [ ] CLI discovery works: `copykit list` and `copykit info <name>` function correctly
- [ ] Integration testing completed
- [ ] Example pages created
- [ ] Documentation updated

## Pull Request Guidelines

When submitting a pull request:

1. **Title**: Use a clear, descriptive title following conventional commit format
2. **Description**: Explain what your changes do and why
3. **Testing**: Describe how you tested your changes
4. **Breaking Changes**: Clearly document any breaking changes
5. **Screenshots**: Include screenshots for UI changes

## Questions and Support

If you have questions about contributing:

1. Check existing issues and discussions
2. Review the project documentation
3. Open an issue for discussion before starting major changes

Thank you for contributing to CopyKit! 🎉