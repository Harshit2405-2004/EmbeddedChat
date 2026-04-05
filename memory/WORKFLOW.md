# 🔄 Git Workflow & Branch Strategy

**Last Updated:** 2026-04-05  
**Status:** ✅ ACTIVE

---

## 🌿 Branch Structure

### Primary Branches

1. **`develop`** (RocketChat/EmbeddedChat)
   - Upstream default branch
   - Always kept clean and synchronized
   - Source for all fix branches
   - **NEVER commit workspace files here**

2. **`local-workspace`** (Harshit2405-2004/EmbeddedChat)
   - Personal development workspace branch
   - Contains analysis tools and results
   - **NEVER merged to develop/main**
   - **NEVER used as base for PRs**
   - Updated periodically with new analysis data

3. **`fix/*`** branches
   - Clean feature/fix branches
   - Created from `develop` (not `local-workspace`)
   - Contain ONLY fix-related changes
   - Short-lived, deleted after merge

---

## 📁 Workspace Branch Contents

**Branch:** `local-workspace`  
**Purpose:** Local development, analysis, and planning  
**URL:** https://github.com/Harshit2405-2004/EmbeddedChat/tree/local-workspace

### Files Included (119 files, 40,475 lines)

#### `.github/` - GSD Framework
```
.github/
├── agents/          (11 files) - GSD agent definitions
│   ├── gsd-planner.agent.md
│   ├── gsd-executor.agent.md
│   ├── gsd-verifier.agent.md
│   ├── gsd-debugger.agent.md
│   ├── gsd-roadmapper.agent.md
│   ├── gsd-phase-researcher.agent.md
│   ├── gsd-project-researcher.agent.md
│   ├── gsd-research-synthesizer.agent.md
│   ├── gsd-plan-checker.agent.md
│   ├── gsd-integration-checker.agent.md
│   └── gsd-codebase-mapper.agent.md
│
├── instructions/    (11 files) - Guidelines for agents
│   ├── checkpoints.instructions.md
│   ├── continuation-format.instructions.md
│   ├── git-integration.instructions.md
│   ├── model-profiles.instructions.md
│   ├── planning-config.instructions.md
│   ├── questioning.instructions.md
│   ├── tdd.instructions.md
│   ├── ui-brand.instructions.md
│   ├── verification-patterns.instructions.md
│   └── ...
│
├── prompts/         (25+ files) - CLI commands
│   ├── plan-phase.prompt.md
│   ├── execute-phase.prompt.md
│   ├── verify-work.prompt.md
│   └── ...
│
├── skills/          (12 files) - GSD skills
│   ├── execute-phase/
│   ├── verify-work/
│   ├── map-codebase/
│   └── ...
│
└── FUNDING.yml      - Funding configuration
```

#### `.gsd/` - Configuration & Telemetry
```
.gsd/
├── telemetry/       (15+ files) - Performance tracking
│   ├── metrics.csv
│   ├── phase-summary.csv
│   ├── telemetry-functions.sh
│   ├── telemetry-functions.ps1
│   ├── model-selector.sh
│   ├── context-compression.sh
│   ├── result-cache.sh
│   ├── report.js
│   ├── report.ps1
│   └── README.md
│
└── templates/       (20+ files) - GSD templates
    ├── config.json              (v2-dynamic-models)
    ├── config.json.baseline     (backup)
    ├── phase-prompt.md
    ├── roadmap.md
    ├── project.md
    └── ...
```

#### `memory/` - Analysis Results
```
memory/
├── CRITICAL-ISSUES.md           (3 critical, 5 high severity)
├── ISSUES-DATABASE.md           (17 discovered issues)
├── issue-tracking.md            (GitHub issue & PR tracking)
├── STATUS.md                    (analysis status)
├── README.md                    (folder documentation)
├── WORKFLOW.md                  (this file)
│
├── analysis/
│   ├── issue_categorization.json
│   ├── recent_open_issues.json
│   └── detailed_code_quality_findings.json
│
└── status/
    ├── github_issues_summary.json
    ├── analysis_status.json
    └── final_analysis_status.json
```

#### Root Files
- `ANALYZE.md` (28 KB) - Comprehensive codebase analysis report

---

## ✅ Standard Workflow

### Step 1: Start on Workspace Branch

```bash
git checkout local-workspace
git pull fork local-workspace
```

**Purpose:**
- Access analysis files (memory/, ANALYZE.md)
- Review issues from CRITICAL-ISSUES.md
- Check issue-tracking.md for PR status
- Plan next fix using GSD tools

**What You Can Do Here:**
- Read analysis reports
- Update issue tracking
- Make notes and plans
- Run analysis tools
- Commit changes to workspace files

### Step 2: Create Clean Fix Branch

```bash
# Switch to clean develop branch
git checkout develop
git pull origin develop

# Create new fix branch (NOT from local-workspace!)
git checkout -b fix/issue-<number>-<short-description>
```

**Examples:**
```bash
git checkout -b fix/issue-1269-error-boundaries
git checkout -b fix/issue-1270-react-version-sync
git checkout -b fix/issue-1271-console-cleanup
```

**⚠️ CRITICAL:**
- Always branch from `develop`, **NEVER** from `local-workspace`
- This ensures clean branch without workspace files
- Prevents accidental commits of .github/, .gsd/, memory/

### Step 3: Implement the Fix

```bash
# Make your code changes
vim packages/react/src/components/ErrorBoundary.jsx

# Run tests
yarn test

# Stage ONLY fix files (not workspace files)
git add packages/react/src/components/ErrorBoundary.jsx
git add packages/react/src/components/App.jsx

# Commit with descriptive message
git commit -m "fix: add ErrorBoundary components (fixes #1269)

- Created ErrorBoundary class component with fallback UI
- Wrapped App root in ErrorBoundary
- Wrapped critical sub-components (MessageList, ChatInput)
- Added error logging to console.error
- Prevents full app crash on component errors

Fixes #1269"
```

**Best Practices:**
- Reference issue number in commit message
- Use conventional commits format (fix:, feat:, refactor:)
- List specific changes in commit body
- Test before committing

### Step 4: Verify & Push

```bash
# Verify commit contains ONLY fix files
git show --name-only HEAD

# ✅ Good (only fix files):
packages/react/src/components/ErrorBoundary.jsx
packages/react/src/components/App.jsx

# ❌ BAD (contains workspace files):
.github/agents/gsd-executor.agent.md
.gsd/templates/config.json
memory/issue-tracking.md

# If clean, push to fork
git push -u fork fix/issue-1269-error-boundaries
```

**Verification Checklist:**
- [ ] Only fix-related files in commit
- [ ] No .github/ files
- [ ] No .gsd/ files
- [ ] No memory/ files
- [ ] No ANALYZE.md
- [ ] Tests passing
- [ ] Code linted

### Step 5: Create Pull Request

```bash
# Create PR using GitHub CLI
gh pr create \
  --repo RocketChat/EmbeddedChat \
  --base develop \
  --title "fix: add ErrorBoundary components" \
  --body "## Description

Implements ErrorBoundary components to prevent full application crashes when individual components throw errors.

## Changes
- Created `ErrorBoundary` class component with fallback UI
- Wrapped app root and critical sub-components
- Added error logging with component stack traces

## Testing
- Tested error boundary catches component errors
- Verified fallback UI displays correctly
- Tested error recovery after boundary reset

## Related Issues
Fixes #1269

## Screenshots
[Add screenshots if applicable]"
```

**Or create via GitHub web UI:**
1. Go to: https://github.com/RocketChat/EmbeddedChat/compare/develop...Harshit2405-2004:EmbeddedChat:fix/issue-1269-error-boundaries
2. Click "Create pull request"
3. Fill in title and description
4. Submit

**Before submitting, verify:**
- [ ] PR shows only fix files (check Files Changed tab)
- [ ] No workspace files in diff
- [ ] Title follows convention
- [ ] Description is clear
- [ ] References issue number

### Step 6: Return to Workspace

```bash
# Switch back to workspace for next fix
git checkout local-workspace

# Update issue tracking
vim memory/issue-tracking.md

# Add PR information:
# - PR number and URL
# - Status: submitted
# - Timestamp

git add memory/issue-tracking.md
git commit -m "workspace: update tracking for #1269 PR"
git push fork local-workspace
```

**Purpose:**
- Document PR submission
- Review next issue to fix
- Keep workspace up to date
- Repeat cycle from Step 1

---

## 🚫 Critical Rules

### ❌ NEVER

1. **Merge `local-workspace` to develop/main**
   - This branch contains development files
   - Not meant for production codebase

2. **Create PR from `local-workspace`**
   - PRs must be clean fix-only branches
   - Workspace files should never reach upstream

3. **Branch from `local-workspace` for fixes**
   - Always branch from `develop`
   - Branching from workspace inherits workspace files

4. **Use `git add .` carelessly**
   - Always specify exact files to stage
   - Prevents accidental commits

5. **Commit workspace files to fix branches**
   - .github/ folders
   - .gsd/ configuration
   - memory/ analysis results
   - ANALYZE.md report

### ✅ ALWAYS

1. **Branch from `develop` for fixes**
   ```bash
   git checkout develop
   git checkout -b fix/issue-XXXX-description
   ```

2. **Verify commit contents before pushing**
   ```bash
   git show --name-only HEAD
   ```

3. **Check PR files before submitting**
   - Review "Files Changed" tab
   - Ensure only fix files present

4. **Use `local-workspace` only for analysis**
   - Reading issues
   - Planning fixes
   - Tracking progress
   - NOT for creating PRs

5. **Stage files explicitly**
   ```bash
   git add path/to/specific/file.js
   # NOT: git add .
   ```

---

## 🔄 Updating Workspace Branch

### After Creating New Analysis

```bash
git checkout local-workspace

# Update memory files
vim memory/issue-tracking.md
vim memory/CRITICAL-ISSUES.md

# Commit updates
git add memory/
git commit -m "workspace: update issue tracking after PR #1269"
git push fork local-workspace
```

### After Optimization Changes

```bash
git checkout local-workspace

# Update GSD configuration
vim .gsd/templates/config.json

# Update telemetry
vim .gsd/telemetry/metrics.csv

# Commit
git add .gsd/
git commit -m "workspace: update telemetry and config"
git push fork local-workspace
```

### Pulling Latest Workspace

```bash
git checkout local-workspace
git pull fork local-workspace
```

---

## 📊 Branch Naming Conventions

### For Fixes
```
fix/issue-<number>-<short-description>
```

**Examples:**
- `fix/issue-1269-error-boundaries`
- `fix/issue-1270-react-version-sync`
- `fix/issue-1271-console-cleanup`
- `fix/issue-1263-password-storage`

### For Features
```
feat/issue-<number>-<short-description>
```

**Examples:**
- `feat/issue-1280-dark-mode`
- `feat/issue-1281-emoji-picker`

### For Refactoring
```
refactor/<description>
```

**Examples:**
- `refactor/extract-auth-logic`
- `refactor/split-large-components`

### For Documentation
```
docs/<description>
```

**Examples:**
- `docs/update-readme`
- `docs/add-api-examples`

---

## 🛡️ Protection Mechanisms

### `.gitignore` Protection

The following entries have been added to `.gitignore` to prevent accidental commits:

```gitignore
# Local development files
/.github/agents/
/.github/instructions/
/.github/prompts/
/.github/skills/
/.gsd/
/memory/
/ANALYZE.md
```

**Note:** This only works for untracked files. Always create branches from clean `develop`.

### Pre-Push Verification Script

Optional helper script to verify clean commits:

```bash
#!/bin/bash
# verify-clean-branch.sh

echo "Checking for workspace files in current branch..."

WORKSPACE_FILES=$(git diff develop --name-only | grep -E "^\.github/|^\.gsd/|^memory/|^ANALYZE\.md")

if [ -n "$WORKSPACE_FILES" ]; then
    echo "❌ ERROR: Workspace files detected in branch!"
    echo "$WORKSPACE_FILES"
    echo ""
    echo "This branch contains development files that should not be in PRs."
    echo "Please create a new branch from 'develop' and cherry-pick only fix commits."
    exit 1
else
    echo "✅ Branch is clean - no workspace files detected"
    exit 0
fi
```

---

## 📋 Checklist for Each Fix

### Planning Phase (on `local-workspace`)
- [ ] Review issue in `memory/CRITICAL-ISSUES.md`
- [ ] Check related discussions in GitHub issue
- [ ] Understand root cause and impact
- [ ] Plan implementation approach
- [ ] Estimate effort and complexity

### Implementation Phase (on `fix/*` branch)
- [ ] Create branch from clean `develop`
- [ ] Implement fix with tests
- [ ] Run linter and tests
- [ ] Verify fix solves the issue
- [ ] Stage ONLY fix files
- [ ] Write descriptive commit message
- [ ] Verify commit with `git show --name-only HEAD`

### PR Phase
- [ ] Push branch to fork
- [ ] Check Files Changed tab on GitHub
- [ ] Verify no workspace files in PR
- [ ] Write clear PR title and description
- [ ] Reference issue number (Fixes #XXXX)
- [ ] Add screenshots if UI change
- [ ] Submit PR

### Tracking Phase (back on `local-workspace`)
- [ ] Update `memory/issue-tracking.md`
- [ ] Mark issue status as "submitted"
- [ ] Record PR number and URL
- [ ] Commit and push workspace updates
- [ ] Move to next issue

---

## 🆘 Troubleshooting

### Problem: Accidentally committed workspace files

**Solution:**
```bash
# Soft reset to undo commit (keep changes)
git reset --soft HEAD~1

# Unstage workspace files
git reset HEAD .github/ .gsd/ memory/ ANALYZE.md

# Re-commit only fix files
git add packages/react/src/components/ErrorBoundary.jsx
git commit -m "fix: add ErrorBoundary components (fixes #1269)"

# Force push (if already pushed)
git push -f fork fix/issue-1269-error-boundaries
```

### Problem: Created PR with workspace files

**Solution:**
```bash
# Close the PR on GitHub
gh pr close <PR-number>

# Delete remote branch
git push fork --delete fix/issue-1269-error-boundaries

# Delete local branch
git branch -D fix/issue-1269-error-boundaries

# Start fresh from develop
git checkout develop
git checkout -b fix/issue-1269-error-boundaries-clean

# Cherry-pick only the fix commit
git cherry-pick <fix-commit-hash>

# Verify clean
git show --name-only HEAD

# Push and create new PR
git push -u fork fix/issue-1269-error-boundaries-clean
gh pr create --base develop
```

### Problem: Local develop has bad commits

**Solution:**
```bash
# Reset local develop to match upstream
git checkout develop
git fetch origin
git reset --hard origin/develop

# Now develop is clean
# Create new branches from this clean state
```

### Problem: Need to sync workspace with latest develop

**Solution:**
```bash
# This is NOT necessary - workspace is separate
# Workspace branch intentionally diverges from develop
# It contains additional files not meant for upstream

# If you need latest develop code + workspace files:
git checkout local-workspace
git merge develop  # Only if you need develop updates
# Usually NOT needed
```

---

## 📈 Progress Tracking

Track fix progress in `memory/issue-tracking.md`:

```markdown
## Issue #1269 - Error Boundaries

**Status:** ✅ PR Submitted  
**Priority:** HIGH  
**Branch:** fix/issue-1269-error-boundaries  
**PR:** https://github.com/RocketChat/EmbeddedChat/pull/1270  
**Created:** 2026-04-05  
**Submitted:** 2026-04-05

**Changes:**
- Created ErrorBoundary class component
- Wrapped App root and critical components
- Added error logging and fallback UI

**Testing:**
- ✅ Error boundary catches errors
- ✅ Fallback UI displays
- ✅ Tests passing

**Next Steps:**
- Wait for review
- Address review comments
- Monitor CI/CD
```

---

## 🎯 Current Status

**Workspace Branch:** ✅ Created and pushed  
**URL:** https://github.com/Harshit2405-2004/EmbeddedChat/tree/local-workspace

**Recent PRs (Fixed):**
1. ✅ #1266 - Password Storage Fix (clean)
2. ✅ #1267 - Silent Promise Failures (clean)
3. ❌ #1268 - Type Safety (closed, had workspace files)
4. ⏳ Type Safety PR - Pending recreation

**Current Branch:** `local-workspace`

**Next Actions:**
1. Create clean PR for Issue #1265 (Type Safety)
2. Fix HIGH-001: Error Boundaries
3. Fix HIGH-002: React Version Fragmentation

---

## 📚 Additional Resources

- **Main Analysis:** `ANALYZE.md`
- **Critical Issues:** `memory/CRITICAL-ISSUES.md`
- **Issue Database:** `memory/ISSUES-DATABASE.md`
- **Status:** `memory/STATUS.md`
- **Telemetry:** `.gsd/telemetry/README.md`
- **GSD Docs:** `.github/skills/*/README.md`

---

**Version:** 1.0  
**Last Updated:** 2026-04-05  
**Status:** Active workflow in use
