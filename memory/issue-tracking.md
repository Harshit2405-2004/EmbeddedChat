# Issue Tracking - Critical & High Priority Fixes

**Last Updated:** April 5, 2026 22:13 UTC

---

## 🔴 Critical Issues - GitHub Status

| ID | GitHub Issue | Title | Status | Priority | PR |
|----|--------------|-------|--------|----------|-----|
| CRITICAL-001 | [#1263](https://github.com/RocketChat/EmbeddedChat/issues/1263) | Password Storage Vulnerability (CWE-312) | ✅ FIXED | P0 | [#1266](https://github.com/RocketChat/EmbeddedChat/pull/1266) |
| CRITICAL-002 | [#1264](https://github.com/RocketChat/EmbeddedChat/issues/1264) | Silent Promise Failures in Auth | ✅ FIXED | P0 | [#1267](https://github.com/RocketChat/EmbeddedChat/pull/1267) |
| CRITICAL-003 | [#1265](https://github.com/RocketChat/EmbeddedChat/issues/1265) | Widespread 'any' type usage | ✅ FIXED (Phase 1) | P0 | Clean PR pending |

---

## 🟠 High Priority Issues - GitHub Status

| ID | GitHub Issue | Title | Status | Priority | PR |
|----|--------------|-------|--------|----------|-----|
| HIGH-001 | [#1270](https://github.com/RocketChat/EmbeddedChat/issues/1270) | Missing React Error Boundaries | 🟡 IN PROGRESS | P1 | Not created |
| HIGH-002 | [#1271](https://github.com/RocketChat/EmbeddedChat/issues/1271) | React Version Fragmentation | 🔴 NEW | P1 | Not created |

---

## 📝 Fix Progress

### ✅ Issue #1263 - Password Storage Vulnerability
**Status:** ✅ FIXED  
**PR:** [#1266](https://github.com/RocketChat/EmbeddedChat/pull/1266)  
**Branch:** `fix/issue-1263-password-storage`  
**Completed:** April 5, 2026

**Changes Made:**
- ✅ Removed password field from userStore (React and React Native)
- ✅ Created ephemeral totpCredentialsStore for TOTP flow
- ✅ Updated useRCAuth hook for auto-cleanup
- ✅ Modified TotpModal to use ephemeral credentials
- ✅ Added .gitignore protection for dev files

**Files Modified:** 7 files

---

### ✅ Issue #1264 - Silent Promise Failures
**Status:** ✅ FIXED  
**PR:** [#1267](https://github.com/RocketChat/EmbeddedChat/pull/1267)  
**Branch:** `fix/issue-1264-silent-errors`  
**Completed:** April 5, 2026

**Changes Made:**
- ✅ Modified googleSSOLogin() to return error objects
- ✅ Modified login() to return error objects for non-401 errors
- ✅ Modified load() to re-throw errors for caller handling

**Files Modified:** 2 files

---

### ✅ Issue #1265 - Type Safety (Phase 1)
**Status:** ✅ FIXED (Clean PR pending)  
**PR:** #1268 closed (workspace files), clean PR needed  
**Branch:** `fix/issue-1265-type-safety-clean`  
**Completed:** April 5, 2026

**Changes Made:**
- ✅ Created packages/api/src/types.ts (Message, User, ActionData interfaces)
- ✅ Created packages/auth/src/types.ts (CurrentUser, AuthToken interfaces)
- ✅ Fixed cloneArray to use generic types
- ✅ Typed EmbeddedChatApi callbacks
- ✅ Typed RocketChatAuth currentUser property

**Files Modified:** 9 files  
**Note:** Need to create clean PR without workspace files

---

### 🟡 Issue #1270 - Missing React Error Boundaries
**Status:** 🟡 IN PROGRESS  
**Priority:** HIGH  
**Timeline:** 2-4 hours  
**Started:** April 5, 2026 22:13 UTC

**Plan:**
- [ ] Create ErrorBoundary component with fallback UI
- [ ] Wrap application root in ErrorBoundary
- [ ] Wrap critical sub-components (MessageList, ChatInput, etc.)
- [ ] Add error logging
- [ ] Test error boundary catches errors
- [ ] Commit and push
- [ ] Create Pull Request

**Files to Create:**
- `packages/react/src/components/ErrorBoundary.jsx`

**Files to Modify:**
- `packages/react/src/views/EmbeddedChat.jsx` (or main entry point)
- `packages/react/src/components/index.js` (export ErrorBoundary)

---

### 🔴 Issue #1271 - React Version Fragmentation
**Status:** 🔴 NEW  
**Priority:** HIGH  
**Timeline:** 1-2 days  
**Started:** Not started

**Plan:**
- [ ] Audit all package.json files for React versions
- [ ] Identify React 18-specific code (if any)
- [ ] Update all packages to React 18 peerDependencies
- [ ] Update root rendering to use createRoot (if needed)
- [ ] Run tests to verify compatibility
- [ ] Verify single React instance with `yarn why react`
- [ ] Check bundle size reduction
- [ ] Commit and push
- [ ] Create Pull Request

**Files to Modify:**
- `packages/react/package.json`
- `packages/ui-kit/package.json`
- `packages/ui-elements/package.json`
- `packages/markups/package.json`
- `packages/htmlembed/package.json`
- `packages/layout_editor/package.json`
- `packages/e2e-react/package.json`
- `packages/react-native/package.json`
- Possibly rendering entry points if using ReactDOM.render

---

## 🚀 Deployment Pipeline

### Branch Strategy
- **Main Branch:** `develop`
- **Workspace Branch:** `local-workspace` (development files, never merged)
- **Fix Branches:** Created from `develop`, NOT from `local-workspace`
  - `fix/issue-1263-password-storage` ✅
  - `fix/issue-1264-silent-errors` ✅
  - `fix/issue-1265-type-safety-clean` ✅
  - `fix/issue-1270-error-boundaries` 🟡 IN PROGRESS
  - `fix/issue-1271-react-version` 🔴 NEXT

### Workflow (Per Fix)
1. Start on `local-workspace` - review issue
2. `git checkout develop` - switch to clean base
3. `git checkout -b fix/issue-XXXX-description` - create fix branch
4. Implement fix, test, commit
5. `git push -u fork fix/issue-XXXX-description`
6. Create PR, verify no workspace files
7. `git checkout local-workspace` - return to workspace
8. Update issue-tracking.md

### PR Checklist (Per Issue)
- [ ] Branch created from clean `develop`
- [ ] Only fix files committed (no .github/, .gsd/, memory/)
- [ ] Commit message references GitHub issue
- [ ] Tests pass locally
- [ ] Code follows project conventions
- [ ] PR description clear and complete
- [ ] PR linked to GitHub issue (Fixes #XXXX)
- [ ] Verified no workspace files in PR
- [ ] CI/CD checks passing

---

## 📊 Statistics

**Total Issues Tracked:** 5  
**Critical Issues:** 3 (all fixed ✅)  
**High Priority Issues:** 2 (1 in progress, 1 new)  

**Pull Requests:**
- ✅ #1266 - Password Storage (merged/pending)
- ✅ #1267 - Silent Errors (merged/pending)  
- ⏳ #1268 - Type Safety (closed, clean PR needed)
- 🟡 #XXXX - Error Boundaries (in progress)
- 🔴 Not created - React Version (not started)

**Completion:**
- Critical Issues: 100% (3/3) ✅
- High Priority: 0% (0/2) 🟡

---

## 🎯 Current Focus

**NOW:** Fixing Issue #1270 (Error Boundaries)  
**NEXT:** Issue #1271 (React Version Fragmentation)  
**THEN:** Create clean PR for Issue #1265 (Type Safety)

---

**Notes:**
- All issues created on April 5, 2026
- Priority: P0 (Critical)
- Assigned to: Current user
- Repository: RocketChat/EmbeddedChat
