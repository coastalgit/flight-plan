# README Template Fix - October 29, 2025

## Problem

When using `flight-plan init apply` to generate projects, the README.md file **sometimes** did not include the Flight Plan documentation section. Specifically, this section was missing:

```markdown
## Documentation

- **Project PRD:** `docs/project-prd.md`
- **Project Rules:** `docs/project-rules.md`
- **Flight Plan:** `.flight-plan/FLIGHT-PLAN-COMMANDS.md`
```

## Root Cause

The README.md was being **generated on-the-fly** from instructions in `GENERATOR.md` rather than from a template file. This made the generation inconsistent because:

1. No `README.md.template` file existed
2. The AI had to generate README content from markdown instructions in GENERATOR.md
3. The instructions were detailed (lines 479-555) but prone to interpretation
4. Sometimes the AI would simplify or omit sections during generation

## Solution

Created a proper **template-driven approach**:

1. **Created:** `framework/templates/README.md.template`
   - Contains all required sections with `{{VARIABLE}}` placeholders
   - Guarantees the Documentation section is always included
   - Consistent structure for all generated projects

2. **Updated:** `framework/GENERATOR.md`
   - Changed README generation instructions to use the template (line 479)
   - Removed inline markdown generation instructions (previously lines 479-555)
   - Added template variables to reference table (lines 521-523)
   - Updated project structure diagram to show template usage (line 353)
   - Updated "Last Updated" timestamp to 2025-10-29

## Files Changed

### Created
- `framework/templates/README.md.template`

### Modified
- `framework/GENERATOR.md`
  - Line 4: Updated last updated date
  - Line 353: Updated project structure comment
  - Lines 477-495: Changed from inline generation to template usage
  - Lines 521-523: Added new template variables
  - Line 667: Updated last updated date

## Testing

To verify the fix:

1. Run `flight-plan init apply` in a solution directory
2. Check generated project's `README.md`
3. Verify it contains the complete Documentation section:
   ```markdown
   ## Documentation
   
   - **Project PRD:** `docs/project-prd.md` - Complete specifications
   - **Project Rules:** `docs/project-rules.md` - How AI should work in this project
   - **Current Status:** `.flight-plan/current.md` - Current phase and progress
   - **Configuration:** `.flight-plan/config.json` - Project settings
   - **Commands Reference:** `.flight-plan/FLIGHT-PLAN-COMMANDS.md` - All Flight Plan commands (standalone)
   - **Phase Standards:** `.flight-plan/FLIGHT-PLAN-PHASES.md` - Phase workflow and standards (standalone)
   - **SpecKit Setup:** `.flight-plan/FLIGHT-PLAN-SPECKIT-SETUP.md` - SpecKit installation guide (standalone)
   ```

## Impact

- ✅ **No breaking changes** - Only affects new project generation
- ✅ **Existing projects unaffected** - No changes to generated projects
- ✅ **Consistent README generation** - All projects will have the same structure
- ✅ **Better maintainability** - Template is easier to update than inline instructions

## Related Files

- `framework/templates/README.md.template` - New template file
- `framework/GENERATOR.md` - Updated generator instructions
- `framework/templates/project-prd.md.template` - Other template (for reference)
- `framework/templates/project-rules.md.template` - Other template (for reference)

## Template Variables Added

| Variable | Description | Default if Missing |
|----------|-------------|--------------------|
| QUICK_START | Basic setup instructions | "See docs/project-prd.md for setup" |
| ARCHITECTURE_DESCRIPTION | Project architecture | From PRD Section 4 |
| ADDITIONAL_TECH | Extra technologies | From PRD or "None" |

## Future Improvements

Consider:
- Adding validation to ensure all template variables are filled
- Creating a template validator tool
- Adding more structured QUICK_START sections per project type

---

**Fix Date:** 2025-10-29  
**Reporter:** User  
**Fixed By:** AI Assistant  
**Status:** Complete

