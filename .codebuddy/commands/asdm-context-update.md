# Instructions for asdm-context-update action

## Purpose
This instruction guides the AI model to update existing context files when workspace changes occur.

## Language Detection

Before updating context files, you must detect and use the current environment's response language:

1. **Detect Response Language**: Analyze the environment settings to determine the primary language:
   - Check system/user language settings or environment configuration
   - Identify the primary language used in project documentation and comments
   - Determine the language preference based on workspace context

2. **Apply Language Consistency**: Ensure all updated context files use the detected language:
   - Use the same language for all markdown files, comments, and documentation
   - Maintain language consistency across all updated context files
   - Follow the detected language's writing conventions and formatting

**IMPORTANT**: The language detection is the FIRST step before any context updates. All output must consistently use the detected language throughout the entire process.

## Steps to Update Context

### 1. Check Existing Context
First, check if `.asdm/contexts/` directory exists and contains context files. If not, follow the initial context generation process.

### 2. Analyze Workspace Changes
Identify what has changed in the workspace since the last context generation:
- New files or directories added
- Modified files
- Deleted files or directories
- Changes in technology stack
- Updated dependencies
- New API endpoints or changes
- Database schema modifications

### 3. Update Context Files
Update the relevant context files based on the detected changes:

1. **Update index.md** - Always update the main index to reflect current workspace state
2. **Update specific context files** based on the nature of changes:
   - For structural changes: update `standard-project-structure.md`
   - For code style changes: update `standard-coding-style.md`
   - For data model changes: update `data-models.md`
   - For deployment changes: update `deployment.md`
   - For API changes: update `api.md`
   - For architecture changes: update `architecture.md`

### 4. Maintain Consistency
Ensure all updates maintain consistency:
- Use the same language as existing context files
- Preserve existing structure and formatting
- Update navigation links and cross-references
- Maintain version tracking and change history

### 5. Add Change Log
Consider adding a change log or version history to track context updates over time.

### 6. Validate Updates
After updating, validate that:
- All context files are current and accurate
- Cross-references between files are correct
- Diagrams and examples reflect current state
- No broken links or outdated information

## Usage
To use this instruction, simply follow the steps above. The AI model should analyze workspace changes and update context files accordingly.