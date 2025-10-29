# Platform-Aware Commands Fix

**Date:** 2025-10-29  
**Issue:** AI agents use Unix-style commands that fail on Windows PowerShell  
**Status:** ✅ Resolved

---

## Problem

When using Flight Plan on Windows with PowerShell, AI agents would use Unix/Linux-style commands like:

```bash
mkdir -p src/components src/services
```

This fails in PowerShell with:
```
mkdir : A positional parameter cannot be found that accepts argument 'src/services'.
ParameterBindingException
```

This wastes time as the AI has to detect the error and retry with correct syntax.

---

## Root Cause

PowerShell's `mkdir` (alias for `New-Item -ItemType Directory`) has different syntax:
- Doesn't support `-p` flag
- Multiple directories must be comma-separated, not space-separated
- Different parameter structure than Unix

---

## Solution

Added **Platform-Aware Commands** section to `project-rules.md.template`:

```markdown
## Platform-Aware Commands

**Shell Type:** Detected from user_info at runtime

**PowerShell (Windows):**
```powershell
# Creating directories (use comma-separated list)
mkdir src/components, src/services, src/utils

# Or with Force flag (creates parent dirs)
New-Item -ItemType Directory -Path src/components, src/services -Force

# Multiple file operations
Remove-Item file1.txt, file2.txt
Copy-Item source.txt, source2.txt -Destination ./dest/
```

**Bash/Zsh (Unix/Linux/Mac):**
```bash
# Creating directories
mkdir -p src/components src/services src/utils

# Multiple operations work with space separation
rm file1.txt file2.txt
cp source.txt source2.txt ./dest/
```

**⚠️ Critical:** Always use platform-appropriate syntax. Check user_info shell type before running commands.
```

---

## Impact

- AI agents now have explicit guidance on platform-specific command syntax
- Saves time by avoiding command failures on first attempt
- Applies to all new projects generated with Flight Plan
- Improves Windows development experience significantly

---

## Files Modified

- `framework/templates/project-rules.md.template` - Added Platform-Aware Commands section

---

## Related Commands

### Common Platform Differences

| Operation | PowerShell (Windows) | Bash/Zsh (Unix/Mac) |
|-----------|---------------------|---------------------|
| Create dirs | `mkdir dir1, dir2` | `mkdir -p dir1 dir2` |
| Remove files | `Remove-Item f1, f2` | `rm f1 f2` |
| Copy files | `Copy-Item f1, f2 -Destination dest/` | `cp f1 f2 dest/` |
| List files | `Get-ChildItem` or `ls` | `ls` |
| Environment vars | `$env:VAR` | `$VAR` |
| Path separator | `\` (but `/` works) | `/` |

---

## Testing

When testing Flight Plan:
1. Generate a new project on Windows
2. Verify `project-rules.md` includes Platform-Aware Commands section
3. Have AI create multiple directories
4. Confirm it uses comma-separated syntax: `mkdir dir1, dir2, dir3`

---

## Future Enhancements

Potential additions:
- Environment variable syntax differences
- Path handling (backslash vs forward slash)
- Process management commands
- Network commands
- Package manager commands (npm, pip, etc. should work cross-platform)

---

**Fix verified and merged into template.**

