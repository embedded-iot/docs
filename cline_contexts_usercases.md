# Cline @Mention Features - Complete Use Cases Guide

## Overview of @Mention Context Tools

Cline provides several @mention tools to add external context:
- **@file** - Include specific file contents
- **@folder** - Include entire folder contents
- **@problems** - Include IDE problems/errors
- **@terminal** - Include terminal output
- **@url** - Include web content from URLs
- **@commit** - Include git commit information and changes

---

## 1. @file - Single File Context

### Use Case 1.1: Debugging Specific Component Error

**Scenario**: Component has a bug, need focused analysis

**Cline Prompt with @mention**:
```
@file src/components/UserProfile.tsx
@file src/hooks/useUser.ts
@file src/types/userTypes.ts

This component is throwing "Cannot read property 'name' of undefined" error. 
Please analyze the data flow and fix the issue.
```

**Why This Works**:
- Provides exact files Cline needs to see
- Keeps context focused and relevant
- Avoids including unnecessary files

---

### Use Case 1.2: Understanding Function Implementation

**Scenario**: Need to understand how a utility function works

**Cline Prompt**:
```
@file src/utils/formatters.ts
@file src/utils/validators.ts

Can you explain how the formatCurrency function works and show me 
examples of using it with different locales?
```

**When to Use @file**:
- Analyzing specific functions or components
- Code review of particular files
- Understanding implementation details
- Debugging focused issues
- When you know exactly which files are relevant

---

## 2. @folder - Bulk Directory Context

### Use Case 2.1: Refactoring Entire Feature Module

**Scenario**: Need to refactor authentication module consistently

**Cline Prompt with @mention**:
```
@folder src/features/auth
@file src/types/authTypes.ts
@file src/services/api.ts

Please refactor this authentication module to use React Query instead of 
custom hooks. Maintain all existing functionality and update all components 
consistently.
```

**Why Use @folder Here**:
- Multiple related files need simultaneous changes
- Ensures consistency across module
- Cline sees all component relationships

---

### Use Case 2.2: Creating Consistent Component Library

**Scenario**: Building UI components with consistent patterns

**Cline Prompt**:
```
@folder src/components/ui
@file tailwind.config.js
@file src/types/uiTypes.ts

I need to create a new Select component that follows the same patterns 
as the existing Input and Button components. Please generate it with 
variants, sizes, and accessibility features.
```

**When to Use @folder**:
- Working with related components
- Ensuring pattern consistency
- Bulk operations on multiple files
- Understanding module architecture
- Creating new components that match existing patterns

---

## 3. @problems - IDE Errors & Warnings

### Use Case 3.1: Fixing TypeScript Errors After Upgrade

**Scenario**: TypeScript upgrade caused multiple type errors

**Cline Prompt with @mention**:
```
@problems
@file tsconfig.json
@file package.json

After upgrading TypeScript to 5.3, I'm getting multiple type errors. 
Please analyze the problems and fix them while maintaining type safety.
```

**What @problems Includes**:
- All TypeScript errors
- ESLint warnings
- Build errors
- IDE diagnostics
- Line numbers and error codes

---

### Use Case 3.2: Resolving Linting Issues Before Commit

**Scenario**: Pre-commit hook failing due to lint errors

**Cline Prompt**:
```
@problems
@file .eslintrc.js
@file .prettierrc

I have 23 ESLint errors preventing my commit. Please fix all linting 
issues while following our project's code style guidelines.
```

**When to Use @problems**:
- Mass error fixing
- Understanding cascading errors
- Pre-commit/pre-push fixes
- After dependency upgrades
- When errors are related across files

**Benefits**:
- Cline sees all errors at once
- Can prioritize fixing root causes
- Understands error relationships
- More efficient than fixing one-by-one

---

## 4. @terminal - Command Output Context

### Use Case 4.1: Debugging Build Failures

**Scenario**: Production build failing with unclear errors

**Cline Prompt with @mention**:
```
@terminal
@file vite.config.ts
@file package.json

The build is failing with the error output shown in the terminal. 
Please analyze the webpack/vite errors and fix the configuration.
```

**What @terminal Captures**:
- Command output (stdout/stderr)
- Error stack traces
- Warning messages
- Build statistics
- Test results

---

### Use Case 4.2: Analyzing Test Failures

**Scenario**: Jest tests failing after refactor

**Cline Prompt**:
```
@terminal
@folder src/components/__tests__
@file jest.config.js

The tests are failing as shown in the terminal output. Please analyze 
the failures and update the tests to match the new implementation.
```

**Example Terminal Output Context**:
```bash
FAIL src/components/UserList.test.tsx
  ● UserList › should render user data

    expect(received).toHaveLength(expected)

    Expected length: 3
    Received length: 0
    Received array:  []

      24 |     await waitFor(() => {
      25 |       const users = screen.getAllByRole('listitem');
    > 26 |       expect(users).toHaveLength(3);
         |                     ^
      27 |     });

Test Suites: 1 failed, 5 passed, 6 total
Tests: 1 failed, 23 passed, 24 total
```

---

### Use Case 4.3: Performance Analysis Output

**Scenario**: Understanding Lighthouse performance results

**Cline Prompt**:
```
@terminal
@file vite.config.ts
@folder src/components

Lighthouse shows poor performance as seen in terminal. Please analyze 
the metrics and suggest optimizations for bundle size and load time.
```

**When to Use @terminal**:
- Build/compile errors
- Test failures
- Performance metrics
- Dependency installation issues
- Git operation output
- Script execution results
- Server logs

---

## 6. @commit - Git Commit Context

### Use Case 6.1: Understanding Recent Changes

**Scenario**: Need to understand what changed in a specific commit

**Cline Prompt with @mention**:
```
@commit a1b2c3d
@file src/components/UserProfile.tsx

What changes were made in this commit? Please explain the modifications 
and why they might have caused the current bug in UserProfile.
```

**What @commit Includes**:
- Commit hash and message
- Author and date
- Files changed
- Diff of changes
- Related commits context

---

### Use Case 6.2: Analyzing Bug Introduction

**Scenario**: Bug appeared after a specific commit, need to investigate

**Cline Prompt**:
```
@commit d4e5f6g
@commit g7h8i9j
@problems
@file src/hooks/useAuth.ts

Users started experiencing auth issues after these two commits. Please 
analyze what changed and identify what's causing the authentication failures.
```

**Why Use Multiple @commit**:
- Compare changes across commits
- Track feature evolution
- Identify breaking changes
- Understand context of modifications

---

### Use Case 6.3: Code Review with Git Context

**Scenario**: Reviewing a commit before merging

**Cline Prompt**:
```
@commit feature-branch-HEAD
@file docs/coding-standards.md
@folder src/components/ui

Please review this commit for:
- Code quality issues
- Violations of our coding standards
- Potential bugs or edge cases
- Performance implications
```

**What Gets Analyzed**:
- All changed files in commit
- Additions and deletions
- Code patterns introduced
- Dependencies affected

---

### Use Case 6.4: Reverting Changes Safely

**Scenario**: Need to revert a commit but preserve some changes

**Cline Prompt**:
```
@commit j1k2l3m
@file src/services/paymentService.ts
@file src/components/CheckoutForm.tsx

This commit introduced a payment bug, but it also fixed styling issues. 
Please help me revert the payment logic changes while keeping the style fixes.
```

---

### Use Case 6.5: Understanding Feature Implementation

**Scenario**: Learning how a feature was implemented

**Cline Prompt**:
```
@commit m4n5o6p
@commit n6o7p8q
@commit o8p9q0r
@folder src/features/notifications

I need to understand how the notification system was implemented. 
I've included the three commits that built this feature. Please explain 
the architecture and implementation approach.
```

**When to Use @commit**:
- Bug investigation (when did it break?)
- Code archaeology (why was it done this way?)
- Learning from past implementations
- Reviewing changes before merge
- Reverting/cherry-picking decisions
- Understanding context of legacy code
- Tracking feature evolution

---

### Use Case 6.6: Merge Conflict Resolution with History

**Scenario**: Resolving complex merge conflicts with git history context

**Cline Prompt**:
```
@commit HEAD
@commit origin/main
@file src/components/Dashboard.tsx
@terminal

I have merge conflicts in Dashboard.tsx. Please analyze both commits 
to understand the intent of each change and help resolve the conflicts 
intelligently, preserving the logic from both branches.
```

**Terminal Output Context**:
```bash
$ git status
On branch feature/dashboard-redesign
You have unmerged paths.

Unmerged paths:
  both modified:   src/components/Dashboard.tsx

$ git log --oneline -3
a1b2c3d (HEAD) feat: add dashboard widgets
d4e5f6g refactor: improve dashboard performance
g7h8i9j fix: dashboard data loading
```

---

### Use Case 6.7: Commit Message Generation

**Scenario**: Need help writing descriptive commit messages

**Cline Prompt**:
```
@file src/components/UserProfile.tsx
@file src/hooks/useUserData.ts
@file src/types/userTypes.ts
@problems

I've fixed the user profile loading issue and optimized the data fetching. 
Please suggest a good commit message following conventional commits format 
that describes these changes.
```

**Cline Can Generate**:
```
feat(user-profile): optimize data fetching and fix loading issues

- Implement proper loading state management in useUserData hook
- Add error boundary for user profile component
- Fix race condition in data fetching
- Optimize re-renders with useMemo

Fixes #123
```

---

### Use Case 6.8: Comparing Branches with Commits

**Scenario**: Understanding differences between feature branch and main

**Cline Prompt**:
```
@commit feature/new-auth
@commit main
@folder src/features/auth

What are the key differences between the new authentication system 
on the feature branch and the current main branch implementation? 
Please highlight breaking changes and new features.
```

---

## 7. @url - External Documentation & Resources

### Use Case 5.1: Following Library Documentation

**Scenario**: Implementing feature from library docs

**Cline Prompt with @mention**:
```
@url https://react-hook-form.com/get-started
@file src/components/forms/ContactForm.tsx

Please implement form validation following the React Hook Form 
documentation. Include the patterns shown in the docs for our 
contact form.
```

**Why Use @url**:
- Get latest API documentation
- Reference official guides
- Include blog post instructions
- Follow specific tutorials
- Access GitHub README files

---

### Use Case 7.2: Debugging from Stack Overflow Solution

**Scenario**: Found solution on Stack Overflow, need implementation

**Cline Prompt**:
```
@url https://stackoverflow.com/questions/12345678/react-memory-leak-fix
@file src/components/DataStreamer.tsx

This Stack Overflow answer explains the memory leak issue I'm having. 
Please implement the solution in my component while adapting it to 
our codebase structure.
```

---

### Use Case 7.3: API Documentation Integration

**Scenario**: Implementing third-party API integration

**Cline Prompt**:
```
@url https://docs.stripe.com/payments/quickstart
@file src/services/paymentService.ts
@file src/components/CheckoutForm.tsx

Please implement Stripe payment integration following their official 
documentation. Include error handling and webhook setup.
```

**When to Use @url**:
- API documentation
- Library guides
- Tutorial implementations
- Error solution references
- GitHub issues/discussions
- Blog post techniques
- Official release notes

**Supported URL Types**:
- Documentation sites
- GitHub repositories/files
- Stack Overflow answers
- Blog posts
- API references
- Tutorial pages

---

## 8. Working with Documentation Files

### Use Case 8.1: Following Project Standards (Manual Include)

**Scenario**: Need to follow project coding standards

**Cline Prompt with @mention**:
```
@file CONTRIBUTING.md
@file docs/coding-standards.md
@file src/components/NewFeature.tsx

Please review this component and ensure it follows our project's 
coding standards and contribution guidelines from the included documentation.
```

**Alternative Approach**:
```
@folder docs
@file src/components/NewFeature.tsx

Review this component against all our documentation standards.
```

---

### Use Case 8.2: Understanding Architecture Decisions

**Scenario**: Implementing feature that requires architecture understanding

**Cline Prompt**:
```
@file docs/architecture.md
@file docs/state-management.md
@folder src/features/shopping-cart

I need to add a shopping cart feature. Please review the architecture 
documentation I've included and implement it following our established patterns.
```

**When to Include Documentation Files**:
- Following project conventions
- Understanding architecture
- Implementing per guidelines
- Onboarding context
- Compliance requirements

**Tip**: Use `@folder docs` to include all documentation at once, or be selective with specific `@file` mentions for focused documentation.

---

## 9. Finding Existing Patterns (Without @codebase)

### Use Case 9.1: Manual Pattern Discovery

**Scenario**: Need to implement feature similar to existing one

**Strategy**: Manually include relevant folders and files

**Cline Prompt with @mention**:
```
@folder src/features/auth
@file src/features/auth/LoginForm.tsx
@file src/types/authTypes.ts

I need to implement a password reset feature. I've included our authentication 
implementation. Please analyze these patterns and create a consistent password 
reset flow following the same structure.
```

**Why This Works**:
- You explicitly show Cline the patterns to follow
- More control over what context is included
- Can include multiple feature folders for comparison

---

### Use Case 9.2: Exploring Similar Implementations

**Scenario**: Finding how specific functionality is implemented

**Cline Prompt**:
```
@folder src/hooks
@folder src/services
@file package.json

Do we have existing utilities for date formatting with timezone support? 
Please search through the included hooks and services folders and show 
me relevant implementations.
```

**Alternative - Two-Step Approach**:
```
Step 1: Ask Cline to suggest which files to look at
"What files should I include to understand our date handling approach?"

Step 2: Include those files
@file src/utils/dateUtils.ts
@file src/hooks/useDateTime.ts
"Now please show me how to use these for timezone conversion"
```

---

### Use Case 9.3: Understanding Project Structure

**Scenario**: New to the codebase, need to understand layout

**Cline Prompt**:
```
@file package.json
@file tsconfig.json
@folder src/types
@file src/main.tsx
@file README.md

I'm new to this project. Please explain the overall architecture, 
main technologies used, and how the project is structured based on 
these files.
```

**Tip for Finding Patterns Without @codebase**:
1. Start with high-level folders (`@folder src/features`)
2. Ask Cline which specific files are relevant
3. Include those specific files for detailed analysis
4. Use `@folder` for groups of related files

---

## 10. Combined @mention Strategies

### Use Case 10.1: Complete Feature Implementation

**Scenario**: Building new feature from scratch with all context

**Cline Prompt with @mention**:
```
@url https://docs.stripe.com/payments/accept-a-payment
@folder src/features/payments
@folder src/components/ui
@file src/services/api.ts
@file docs/feature-requirements.md
@problems

Implement a complete checkout flow with Stripe integration. Follow the 
official Stripe docs (included via URL), use our existing payment patterns, 
leverage our UI components, and fix any issues that arise.
```

**Why Multiple @mentions**:
- External docs for API reference
- Existing payment features for patterns
- UI components for consistency
- API service for structure
- Requirements for specs
- Problems for error prevention

---

### Use Case 10.2: Comprehensive Bug Fix

**Scenario**: Complex bug requiring multiple context sources

**Cline Prompt**:
```
@problems
@terminal
@file src/components/DataGrid.tsx
@file src/hooks/useVirtualization.ts
@folder src/utils/performance
@url https://github.com/tanstack/virtual/discussions/123

Users report performance issues with large datasets. The problems 
panel shows memory warnings, and terminal has performance logs. 
I've included our performance utilities folder. Please investigate and optimize.
```

---

### Use Case 10.3: Migration Task

**Scenario**: Migrating from one library to another

**Cline Prompt**:
```
@url https://tanstack.com/query/latest/docs/react/guides/migrating-to-v5
@folder src/hooks/api
@folder src/services
@file package.json
@problems

Migrate from React Query v4 to v5. I've included all our API hooks and 
services that use React Query. Update everything, fix breaking changes, 
and ensure tests pass. Follow the official migration guide.
```

---

## 10. Advanced @mention Patterns

### Use Case 9.1: Incremental Context Building

**Scenario**: Start small, add context as needed

**Initial Prompt**:
```
@file src/components/UserProfile.tsx

This component has a performance issue. Can you identify it?
```

**Follow-up After Cline's Response**:
```
@file src/hooks/useUserData.ts
@terminal
@problems

Thanks! Now I see the hook is the issue. Here's the terminal output 
showing the re-render count.
```

**Strategy**: Don't overwhelm with context initially; add as conversation progresses

---

### Use Case 9.2: Context Scoping for Security

**Scenario**: Security-sensitive code review

**Cline Prompt**:
```
@file src/middleware/auth.ts
@file src/utils/encryption.ts
@docs
@file docs/security-guidelines.md

Review this authentication middleware for security vulnerabilities. 
Check against our security guidelines and identify any issues.
```

**Note**: Only include security-sensitive files when necessary

---

### Use Case 9.3: Performance-Focused Context

**Scenario**: Optimizing specific performance bottleneck

**Cline Prompt**:
```
@terminal
@file src/components/VirtualizedList.tsx
@codebase memoization and optimization patterns
@url https://react.dev/reference/react/memo

The profiler shows this component re-renders 47 times per second. 
Please analyze the terminal performance data and optimize.
```

---

## 10. @mention Best Practices

### DO's ✅

1. **Be Specific with @file**
   ```
   ✅ @file src/components/auth/LoginForm.tsx
   ❌ @file src/components/LoginForm.tsx (ambiguous)
   ```

2. **Use @folder for Related Changes**
   ```
   ✅ @folder src/features/cart (when updating entire feature)
   ❌ @file src/features/cart/File1.tsx @file src/features/cart/File2.tsx...
   ```

3. **Combine @problems with Source Files**
   ```
   ✅ @problems @file src/components/BuggyComponent.tsx
   ❌ @problems (without relevant source code)
   ```

4. **Use @terminal for Error Context**
   ```
   ✅ @terminal @file vite.config.ts (build errors)
   ❌ Just pasting errors in chat
   ```

5. **Reference @url for Official Docs**
   ```
   ✅ @url https://react.dev/reference/react/useEffect
   ❌ Paraphrasing documentation
   ```

6. **Use @commit for Git History**
   ```
   ✅ @commit a1b2c3d @file src/buggy-file.ts
   ❌ Describing changes without showing commit
   ```

7. **Combine @commit with @problems for Bug Tracking**
   ```
   ✅ @commit last-working-commit @commit first-broken-commit @problems
   ❌ Guessing when bug was introduced
   ```

### DON'Ts ❌

1. **Don't Include Too Many Files**
   ```
   ❌ @folder src (entire source - too broad)
   ✅ @folder src/features/auth (specific module)
   ```

2. **Don't Mix Unrelated Contexts**
   ```
   ❌ @file src/auth.ts @file src/payment.ts (unrelated)
   ✅ Focus on one feature at a time
   ```

3. **Don't Forget Configuration Files**
   ```
   ❌ @file src/component.tsx (for build issues)
   ✅ @file src/component.tsx @file vite.config.ts @file package.json
   ```

4. **Don't Over-rely on @codebase**
   ```
   ❌ @codebase everything about authentication
   ✅ @codebase authentication flow @file src/auth/Login.tsx
   ```

---

## 11. Troubleshooting @mention Usage

### Issue: Context Too Large

**Problem**: "Context limit exceeded" error

**Solution**:
```
Instead of:
@folder src/components (100+ files)

Use:
@folder src/components/auth (specific subset)
@file src/components/types.ts (shared types)
```

---

### Issue: Wrong Files Included

**Problem**: Cline doesn't have the right context

**Solution**:
```
Be explicit:
@file src/hooks/useAuth.ts
@file src/contexts/AuthContext.tsx
@file src/types/auth.ts

Not just:
@folder src/hooks
```

---

### Issue: @url Not Loading

**Problem**: URL content not accessible

**Solution**:
- Use direct documentation URLs
- Avoid URLs behind authentication
- Use permalink/stable URLs
- Consider copying content if URL fails

---

## 12. @mention Workflow Examples

### Morning Development Routine

```
1. Check status:
   @problems
   @terminal
   "What issues accumulated overnight?"

2. Start feature:
   @codebase similar feature patterns
   @docs
   @folder src/features/new-feature
   "Begin implementing user dashboard feature"

3. Fix issues:
   @problems
   @file src/components/Dashboard.tsx
   "Fix TypeScript errors"
```

### Code Review Workflow

```
@folder src/features/pull-request-branch
@docs
@file docs/code-review-checklist.md
@problems

"Review this PR for code quality, standards compliance, and potential issues"
```

### Debugging Workflow

```
1. Identify issue:
   @problems
   @terminal
   @file src/components/BrokenComponent.tsx

2. Find patterns:
   @codebase error handling patterns
   @url [stackoverflow solution]

3. Test fix:
   @terminal
   @file src/components/BrokenComponent.test.tsx
```

---

## Summary: When to Use Each @mention

| @mention | Best For | Example |
|----------|----------|---------|
| **@file** | Specific file analysis | Single component debugging |
| **@folder** | Module-wide changes | Feature refactoring |
| **@problems** | Error fixing | TypeScript errors, lint issues |
| **@terminal** | Command output | Build errors, test results |
| **@url** | External docs | API documentation, tutorials |
| **@codebase** | Pattern discovery | Finding similar implementations |
| **@docs** | Project standards | Following guidelines |

The key to effective @mention usage is providing the right context at the right time—not too much, not too little, and always relevant to your specific task.