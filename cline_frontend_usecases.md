# Cline Frontend Development - Real Use Cases with Prompts & Contexts

## Table of Contents
1. [Component Development](#1-component-development)
2. [Bug Fixing & Debugging](#2-bug-fixing--debugging)
3. [Feature Implementation](#3-feature-implementation)
4. [Code Refactoring](#4-code-refactoring)
5. [Testing Implementation](#5-testing-implementation)
6. [Performance Optimization](#6-performance-optimization)
7. [Git Workflow Integration](#7-git-workflow-integration)
8. [Build & Deployment Issues](#8-build--deployment-issues)
9. [API Integration](#9-api-integration)
10. [Responsive Design](#10-responsive-design)

---

## 1. Component Development

### Use Case 1.1: Creating a Reusable Button Component

**Developer Scenario**: Need to create a consistent button component with variants, sizes, and accessibility features.

**Cline Prompt**:
```
I need to create a reusable Button component for our design system. It should support different variants (primary, secondary, outline), sizes (sm, md, lg), and be fully accessible. Please analyze our existing UI components and create a consistent implementation.
```

**Context Setup**:
```bash
# Design system context
@folder src/components/ui
@file src/components/ui/Input.tsx        # Existing component for pattern
@file src/types/uiTypes.ts               # Component prop types
@file tailwind.config.js                 # Design tokens
@file src/styles/components.css          # Component styles

# Testing context
@file src/components/ui/Input.test.tsx   # Test pattern reference
@file src/test-utils/render.tsx          # Test utilities
```

**File Code Context**:
```typescript
// src/types/uiTypes.ts
export interface BaseComponentProps {
  className?: string;
  children: React.ReactNode;
  disabled?: boolean;
  loading?: boolean;
}

export type ComponentSize = 'sm' | 'md' | 'lg';
export type ComponentVariant = 'primary' | 'secondary' | 'outline';
```

**Terminal Output**:
```bash
$ npm run dev
✓ Ready in 1.2s
Local: http://localhost:3000

$ npm run storybook
info @storybook/react-vite v7.0.0 started
info => Serving static files from ./public at /
Local: http://localhost:6006/
```

**Expected Cline Actions**:
- Create `src/components/ui/Button.tsx` with TypeScript interfaces
- Generate Storybook story file
- Create test file with accessibility tests
- Update exports in `src/components/ui/index.ts`

---

### Use Case 1.2: Converting Class Component to Functional Component

**Developer Scenario**: Legacy class component needs modernization to hooks.

**Cline Prompt**:
```
Please convert this class component to a functional component using modern React hooks. Maintain all existing functionality including state management, lifecycle methods, and error boundaries.
```

**Context Setup**:
```bash
# Legacy component
@file src/components/UserProfile.jsx      # Class component to convert
@file src/components/UserProfile.test.js # Existing tests

# Modern patterns
@file src/hooks/useUser.ts               # Similar custom hook
@file src/components/ModernComponent.tsx # Pattern reference
@file package.json                       # React version info
```

**File Code Context**:
```jsx
// src/components/UserProfile.jsx
class UserProfile extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      user: null,
      loading: true,
      error: null
    };
  }

  async componentDidMount() {
    try {
      const user = await fetchUser(this.props.userId);
      this.setState({ user, loading: false });
    } catch (error) {
      this.setState({ error: error.message, loading: false });
    }
  }

  componentDidUpdate(prevProps) {
    if (prevProps.userId !== this.props.userId) {
      this.fetchUserData();
    }
  }

  // ... rest of component
}
```

**Terminal Output**:
```bash
$ npm test UserProfile
PASS src/components/UserProfile.test.js
✓ should render user data correctly
✓ should handle loading state
✗ should handle error state (1)

FAIL src/components/UserProfile.test.js
Expected: "Error: User not found"
Received: "Loading..."
```

---

## 2. Bug Fixing & Debugging

### Use Case 2.1: Memory Leak in React Component

**Developer Scenario**: Component causes memory leaks due to improper cleanup.

**Cline Prompt**:
```
I'm experiencing memory leaks in this component. The browser dev tools show increasing memory usage and React DevTools shows components not unmounting properly. Please analyze the issue and fix memory leaks with proper cleanup.
```

**Context Setup**:
```bash
# Problematic component
@file src/components/DataStreamer.tsx
@file src/hooks/useWebSocket.ts
@file src/services/dataService.ts

# Performance data
@file performance-profile.json         # Chrome DevTools export
@file src/utils/memoryUtils.ts        # Memory debugging utilities
```

**File Code Context**:
```typescript
// src/components/DataStreamer.tsx
const DataStreamer: React.FC = () => {
  const [data, setData] = useState([]);
  
  useEffect(() => {
    const interval = setInterval(() => {
      fetchData().then(setData);
    }, 1000);
    
    const ws = new WebSocket('ws://localhost:8080');
    ws.onmessage = (event) => {
      setData(prev => [...prev, JSON.parse(event.data)]);
    };
    
    // Missing cleanup!
  }, []);

  return <div>{/* render data */}</div>;
};
```

**Error Messages**:
```
Warning: Can't perform a React state update on an unmounted component. This is a no-op, but it indicates a memory leak in your application.

Chrome DevTools Memory Tab:
Heap Size: 156 MB (increasing)
Detached DOM Nodes: 1,247
Event Listeners: 89 (not cleaned up)
```

**Terminal Output**:
```bash
$ npm run dev
[webpack-dev-server] Memory usage: 487 MB

$ npm run test:memory
Memory Leak Test: FAILED
Expected cleanup after unmount
Actual: WebSocket connections remain open
```

---

### Use Case 2.2: CSS Layout Breaking on Mobile

**Developer Scenario**: Desktop layout works fine but breaks on mobile devices.

**Cline Prompt**:
```
The layout is breaking on mobile devices. The sidebar overlaps content and buttons are cut off. Please analyze the CSS and fix responsive issues while maintaining the desktop layout.
```

**Context Setup**:
```bash
# Layout components
@file src/components/Layout/Sidebar.tsx
@file src/components/Layout/MainContent.tsx
@file src/styles/layout.css

# Responsive utilities
@file src/hooks/useMediaQuery.ts
@file tailwind.config.js                # Breakpoint configuration

# Screenshots (describe in prompt)
# Mobile screenshot showing overlap issue
# Desktop screenshot showing correct layout
```

**File Code Context**:
```css
/* src/styles/layout.css */
.sidebar {
  position: fixed;
  left: 0;
  top: 0;
  width: 280px;
  height: 100vh;
  background: #fff;
  z-index: 1000;
}

.main-content {
  margin-left: 280px; /* Problem: no mobile consideration */
  padding: 20px;
}

.mobile-menu-button {
  display: none; /* Problem: no mobile menu toggle */
}
```

**Browser DevTools Output**:
```
Console Errors:
Viewport: 375x812 (iPhone X)
Element overflows: .sidebar (280px) > viewport (375px)
Computed styles: margin-left: 280px causes content shift off-screen

Responsive Design Mode:
✗ iPhone SE (375px): Content not visible
✗ iPad (768px): Sidebar too narrow
✓ Desktop (1024px): Layout correct
```

---

## 3. Feature Implementation

### Use Case 3.1: Implementing Dark Mode Toggle

**Developer Scenario**: Add dark mode support across the entire application.

**Cline Prompt**:
```
I need to implement a dark mode toggle that works across the entire application. It should persist user preference, update all components, and handle system preference detection. Please create a comprehensive dark mode solution.
```

**Context Setup**:
```bash
# Theme configuration
@file tailwind.config.js
@file src/styles/globals.css
@folder src/styles/themes

# Existing components to update
@file src/components/Header.tsx
@file src/components/Button.tsx
@folder src/components/ui

# State management
@file src/contexts/ThemeContext.tsx      # If exists
@file src/hooks/useLocalStorage.ts       # For persistence
```

**File Code Context**:
```css
/* src/styles/globals.css */
:root {
  --color-primary: #3b82f6;
  --color-background: #ffffff;
  --color-text: #1f2937;
  --color-border: #e5e7eb;
}

/* Dark mode variables needed */
```

**Terminal Output**:
```bash
$ npm run dev
Ready in 1.3s

$ npm run build
Building for production...
✓ 64 modules compiled successfully

# Need to test theme switching
$ npm run test:e2e
Running dark mode toggle tests...
```

---

### Use Case 3.2: Adding Form Validation with React Hook Form

**Developer Scenario**: Implement comprehensive form validation for a contact form.

**Cline Prompt**:
```
Please implement form validation for this contact form using React Hook Form and Zod schema validation. Include real-time validation, error messages, and submission handling with proper UX feedback.
```

**Context Setup**:
```bash
# Form component
@file src/components/forms/ContactForm.tsx
@file src/types/formTypes.ts

# Validation setup
@file src/schemas/contactSchema.ts       # If exists
@file src/hooks/useFormSubmission.ts     # Custom hook

# UI components
@file src/components/ui/Input.tsx
@file src/components/ui/Button.tsx
@file src/components/ui/ErrorMessage.tsx

# API service
@file src/services/contactApi.ts
```

**File Code Context**:
```typescript
// src/components/forms/ContactForm.tsx (current state)
const ContactForm: React.FC = () => {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });
  
  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    // Basic validation needed
    console.log(formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      {/* Basic form fields without validation */}
    </form>
  );
};
```

**Terminal Output**:
```bash
$ npm install react-hook-form @hookform/resolvers zod
added 3 packages in 2.1s

$ npm run dev
Form validation libraries loaded successfully
```

---

## 4. Code Refactoring

### Use Case 4.1: Breaking Down Large Component

**Developer Scenario**: Large monolithic component needs to be split into smaller, reusable components.

**Cline Prompt**:
```
This component has grown too large (500+ lines). Please refactor it into smaller, focused components while maintaining all functionality. Extract reusable logic into custom hooks and ensure proper component separation.
```

**Context Setup**:
```bash
# Large component to refactor
@file src/components/Dashboard.tsx       # 500+ lines

# Related files
@file src/types/dashboardTypes.ts
@file src/services/dashboardApi.ts
@file src/hooks/useDashboard.ts          # If exists

# Target component structure
@folder src/components/dashboard         # New folder structure
@file src/components/Dashboard.test.tsx  # Existing tests
```

**File Code Context**:
```typescript
// src/components/Dashboard.tsx (excerpt)
const Dashboard: React.FC = () => {
  // 50+ state variables
  const [users, setUsers] = useState([]);
  const [analytics, setAnalytics] = useState({});
  const [notifications, setNotifications] = useState([]);
  // ... many more

  // 20+ useEffect hooks
  useEffect(() => {
    // Fetch users
  }, []);

  useEffect(() => {
    // Fetch analytics
  }, []);
  
  // 10+ handler functions
  const handleUserAction = () => { /* complex logic */ };
  const handleAnalyticsUpdate = () => { /* complex logic */ };

  // 300+ lines of JSX
  return (
    <div className="dashboard">
      {/* Complex nested JSX */}
    </div>
  );
};
```

**Git Context**:
```bash
$ git log --oneline -5
a1b2c3d feat: add user management to dashboard
d4e5f6g fix: analytics data loading
g7h8i9j feat: notification system
j1k2l3m refactor: dashboard performance improvements
m4n5o6p initial dashboard implementation
```

---

## 5. Testing Implementation

### Use Case 5.1: Adding Unit Tests for Custom Hook

**Developer Scenario**: Custom hook needs comprehensive test coverage.

**Cline Prompt**:
```
Please create comprehensive unit tests for this custom hook. Include tests for all edge cases, error scenarios, loading states, and side effects. Use React Testing Library's renderHook utility.
```

**Context Setup**:
```bash
# Hook to test
@file src/hooks/useApiData.ts

# Test utilities
@file src/test-utils/renderHook.ts
@file src/__mocks__/apiMocks.ts

# Similar test patterns
@file src/hooks/useUser.test.ts          # Pattern reference
@file jest.config.js
@file src/setupTests.ts
```

**File Code Context**:
```typescript
// src/hooks/useApiData.ts
export const useApiData = <T>(url: string, options?: RequestOptions) => {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    let isCancelled = false;
    
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url, options);
        
        if (!response.ok) {
          throw new Error(`HTTP ${response.status}`);
        }
        
        const result = await response.json();
        
        if (!isCancelled) {
          setData(result);
          setError(null);
        }
      } catch (err) {
        if (!isCancelled) {
          setError(err instanceof Error ? err.message : 'Unknown error');
          setData(null);
        }
      } finally {
        if (!isCancelled) {
          setLoading(false);
        }
      }
    };

    fetchData();

    return () => {
      isCancelled = true;
    };
  }, [url, options]);

  return { data, loading, error };
};
```

**Terminal Output**:
```bash
$ npm test useApiData
PASS src/hooks/useApiData.test.ts
✓ should fetch data successfully
✓ should handle loading state
✗ should handle error state
✗ should cleanup on unmount

Test Suites: 1 failed, 0 passed, 1 total
Tests: 2 passed, 2 failed, 4 total
```

---

## 6. Performance Optimization

### Use Case 6.1: Optimizing Bundle Size

**Developer Scenario**: Application bundle is too large, affecting load times.

**Cline Prompt**:
```
Our bundle size is 2.8MB which is causing slow load times. Please analyze the bundle, identify heavy dependencies, implement code splitting, and optimize imports. Target bundle size under 1MB for initial load.
```

**Context Setup**:
```bash
# Build configuration
@file vite.config.ts                    # Build settings
@file package.json                      # Dependencies
@file tsconfig.json                     # TypeScript settings

# Large components/libraries
@file src/components/Charts.tsx         # Likely using heavy chart library
@file src/utils/lodash-utils.ts         # Full lodash imports?
@folder src/pages                       # Route components

# Bundle analysis
@file bundle-analysis.json              # Webpack bundle analyzer output
```

**Terminal Output**:
```bash
$ npm run build
Building for production...
✓ 127 modules compiled
Bundle size: 2.8MB (gzipped: 892KB)

$ npm run analyze
Analyzing bundle...
Top heavy imports:
- lodash: 847KB (30.2%)
- chart.js: 623KB (22.3%)  
- @fortawesome/fontawesome-free: 445KB (15.9%)
- moment.js: 234KB (8.4%)

$ npm run lighthouse
Performance Score: 47/100
First Contentful Paint: 3.2s
Largest Contentful Paint: 5.7s
```

**File Code Context**:
```typescript
// src/utils/lodash-utils.ts (problematic)
import _ from 'lodash'; // Imports entire library!

export const debounceSearch = _.debounce;
export const deepClone = _.cloneDeep;
export const groupData = _.groupBy;

// src/components/Charts.tsx
import Chart from 'chart.js/auto'; // Heavy import
import 'chart.js/auto'; // Unnecessary duplicate?
```

---

## 7. Git Workflow Integration

### Use Case 7.1: Pre-commit Hook Integration

**Developer Scenario**: Need to set up automated code quality checks before commits.

**Cline Prompt**:
```
Please set up pre-commit hooks that run linting, type checking, tests, and formatting before each commit. Also configure pre-push hooks for more comprehensive checks. Integrate with our existing CI/CD pipeline.
```

**Context Setup**:
```bash
# Git configuration
@file .git/config                       # Git settings
@file .githooks/pre-commit              # If exists
@file .husky/pre-commit                 # Husky hooks

# Quality tools configuration  
@file .eslintrc.js
@file .prettierrc
@file tsconfig.json
@file jest.config.js

# CI/CD files
@file .github/workflows/ci.yml
@file package.json                      # Scripts section
```

**Git Status Context**:
```bash
$ git status
On branch feature/user-authentication
Changes to be committed:
  modified: src/components/LoginForm.tsx
  modified: src/hooks/useAuth.ts
  new file: src/types/authTypes.ts

Changes not staged:
  modified: src/styles/components.css
  modified: package.json
```

**Terminal Output**:
```bash
$ git commit -m "feat: add user authentication"
Running pre-commit hooks...

✓ ESLint: No issues found
✗ TypeScript: Type errors found
  src/hooks/useAuth.ts:23:5 - error TS2322: Type 'string' is not assignable to type 'User'

✗ Tests: 2 failing tests
  LoginForm.test.tsx: should validate email format
  useAuth.test.ts: should handle login error

Pre-commit hook failed. Please fix issues before committing.
```

---

### Use Case 7.2: Merge Conflict Resolution

**Developer Scenario**: Complex merge conflicts in feature branch that need resolution.

**Cline Prompt**:
```
I have merge conflicts when trying to merge my feature branch. The conflicts are in component files, styles, and package.json. Please help resolve these conflicts while preserving both sets of changes appropriately.
```

**Context Setup**:
```bash
# Conflicted files
@file src/components/UserProfile.tsx     # Conflict markers present
@file src/styles/globals.css            # Conflict markers present
@file package.json                      # Dependency conflicts

# Git information
@file .git/MERGE_HEAD                   # Merge commit info
@file .git/MERGE_MSG                    # Merge message
```

**Git Context**:
```bash
$ git status
On branch feature/user-profile-enhancement
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  both modified:   src/components/UserProfile.tsx
  both modified:   src/styles/globals.css
  both modified:   package.json

$ git log --oneline --graph -5
* a1b2c3d (HEAD -> feature/user-profile-enhancement) feat: add profile editing
* d4e5f6g fix: profile image upload
| * g7h8i9j (main) feat: add user preferences
| * j1k2l3m fix: styling consistency
|/
* m4n5o6p common ancestor
```

**File Code Context with Conflicts**:
```typescript
// src/components/UserProfile.tsx
const UserProfile: React.FC<Props> = ({ userId }) => {
<<<<<<< HEAD
  const [profile, setProfile] = useState<UserProfile | null>(null);
  const [editing, setEditing] = useState(false);
  
  const handleEdit = () => {
    setEditing(true);
  };
=======
  const [userProfile, setUserProfile] = useState<Profile | null>(null);
  const [preferences, setPreferences] = useState<UserPreferences>({});
  
  const handlePreferenceUpdate = (prefs: UserPreferences) => {
    setPreferences(prefs);
  };
>>>>>>> main

  return (
    <div className="user-profile">
      {/* More conflict markers... */}
    </div>
  );
};
```

---

## 8. Build & Deployment Issues

### Use Case 8.1: Production Build Failures

**Developer Scenario**: Application builds successfully in development but fails in production.

**Cline Prompt**:
```
The production build is failing with TypeScript errors and webpack issues that don't appear in development. Please analyze the build configuration, fix the type errors, and ensure production build succeeds.
```

**Context Setup**:
```bash
# Build configuration
@file vite.config.ts
@file tsconfig.json
@file tsconfig.build.json              # Production TS config
@file package.json                     # Build scripts

# Environment files
@file .env
@file .env.production
@file .env.local

# Problematic files (from build output)
@file src/services/apiClient.ts
@file src/utils/environmentUtils.ts
```

**Terminal Output**:
```bash
$ npm run build
Building for production...

✗ Build failed in 15.3s

Error: Build failed with 4 errors:
src/services/apiClient.ts:15:23: ERROR: Could not resolve "process"
src/utils/environmentUtils.ts:8:15: ERROR: 'NODE_ENV' is not defined
src/components/DevTools.tsx:3:1: ERROR: Export 'DevTools' was not found

TypeScript errors:
src/services/apiClient.ts(23,5): error TS2322: Type 'undefined' is not assignable to type 'string'
Property 'VITE_API_URL' does not exist on type 'ImportMetaEnv'

Bundle analysis:
Entry point: src/main.tsx
Output: dist/
Total size: Build failed before completion
```

**File Code Context**:
```typescript
// src/services/apiClient.ts
const API_URL = process.env.VITE_API_URL; // Problem: process not available in browser

// src/utils/environmentUtils.ts
export const isDevelopment = process.env.NODE_ENV === 'development'; // Problem: not defined

// vite.config.ts
export default defineConfig({
  plugins: [react()],
  define: {
    // Missing environment variable definitions
  },
  build: {
    // Missing production-specific settings
  }
});
```

---

## 9. API Integration

### Use Case 9.1: GraphQL Integration with Apollo Client

**Developer Scenario**: Integrate GraphQL API with Apollo Client, including caching and error handling.

**Cline Prompt**:
```
Please integrate Apollo Client for GraphQL API communication. Set up proper caching, error handling, loading states, and type-safe queries. Include pagination and optimistic updates where appropriate.
```

**Context Setup**:
```bash
# GraphQL setup
@file src/graphql/client.ts            # Apollo Client config
@folder src/graphql/queries
@folder src/graphql/mutations
@file src/types/graphqlTypes.ts

# Components using GraphQL
@file src/components/UserList.tsx
@file src/hooks/useUsers.ts

# API schema
@file schema.graphql                   # GraphQL schema
@file codegen.yml                      # Code generation config
```

**File Code Context**:
```typescript
// src/components/UserList.tsx (current REST implementation)
const UserList: React.FC = () => {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    fetch('/api/users')
      .then(res => res.json())
      .then(setUsers)
      .catch(err => setError(err.message))
      .finally(() => setLoading(false));
  }, []);

  // Need to convert to GraphQL with Apollo Client
};
```

**GraphQL Schema Context**:
```graphql
# schema.graphql
type User {
  id: ID!
  name: String!
  email: String!
  avatar: String
  posts: [Post!]!
}

type Query {
  users(first: Int, after: String): UserConnection!
  user(id: ID!): User
}

type UserConnection {
  edges: [UserEdge!]!
  pageInfo: PageInfo!
}
```

**Terminal Output**:
```bash
$ npm install @apollo/client graphql
added 12 packages in 3.4s

$ npm run codegen
GraphQL Code Generator v2.13.12

Generated:
- src/generated/graphql.ts (types)
- src/generated/hooks.ts (hooks)

$ npm run dev
Apollo Client initialized
Cache configured with type policies
```

---

## 10. Responsive Design

### Use Case 10.1: Mobile-First Responsive Layout

**Developer Scenario**: Convert desktop-first design to mobile-first responsive layout.

**Cline Prompt**:
```
This layout was designed desktop-first and doesn't work well on mobile. Please convert it to mobile-first responsive design, optimize touch interactions, and improve mobile UX while maintaining desktop functionality.
```

**Context Setup**:
```bash
# Layout components
@file src/components/Layout/Header.tsx
@file src/components/Layout/Sidebar.tsx
@file src/components/Layout/MainContent.tsx

# Responsive utilities
@file src/hooks/useMediaQuery.ts
@file src/hooks/useBreakpoint.ts

# Styling
@file tailwind.config.js              # Breakpoint configuration
@file src/styles/responsive.css
@folder src/styles/mobile

# Mobile testing screenshots (describe in prompt)
```

**File Code Context**:
```css
/* src/styles/layout.css (desktop-first - problematic) */
.container {
  display: grid;
  grid-template-columns: 280px 1fr;
  grid-template-rows: 80px 1fr;
  height: 100vh;
}

.sidebar {
  grid-column: 1;
  grid-row: 1 / -1;
  background: #f8f9fa;
  padding: 20px;
}

.header {
  grid-column: 2;
  grid-row: 1;
  border-bottom: 1px solid #e9ecef;
}

.main {
  grid-column: 2;
  grid-row: 2;
  padding: 20px;
  overflow-y: auto;
}

/* Missing mobile styles! */
@media (max-width: 768px) {
  /* Empty - needs implementation */
}
```

**Mobile Issues Context**:
```
Mobile Testing Results:
❌ iPhone SE (375px): Sidebar covers content completely  
❌ iPhone 12 (390px): Header navigation not accessible
❌ iPad (768px): Grid layout breaks, content overlaps
❌ Touch targets too small (buttons < 44px)
❌ Horizontal scrolling required
❌ Menu toggle not functional

Performance Impact:
- Mobile LCP: 4.2s (poor)
- CLS: 0.15 (needs improvement)
- FID: 180ms (needs improvement)
```

**Terminal Output**:
```bash
$ npm run dev:mobile
Starting development server with mobile debugging...
Server: http://localhost:3000
QR Code: [QR code for mobile testing]

$ npm run test:responsive
Testing responsive breakpoints...
✗ xs (320px): Layout broken
✗ sm (640px): Navigation issues  
✗ md (768px): Sidebar overlap
✓ lg (1024px): Desktop layout working
✓ xl (1280px): Desktop layout working

$ npm run lighthouse:mobile
Mobile Performance Score: 23/100
Issues found:
- Large layout shifts
- Non-responsive images  
- Touch targets too small
```

---

## Additional Context Patterns

### Error Handling Context
```typescript
// Always include error boundaries and error states
@file src/components/ErrorBoundary.tsx
@file src/hooks/useErrorHandler.ts
@file src/utils/errorReporting.ts
```

### Performance Monitoring Context  
```bash
# Include performance monitoring setup
@file src/utils/performance.ts
@file .env                            # Performance tracking keys
@file src/hooks/usePerformanceMonitor.ts
```

### Accessibility Context
```bash
# A11y testing and utilities
@file src/utils/a11y.ts
@folder src/__tests__/a11y
@file .axerc.json                     # Axe accessibility config
```

### State Management Context
```bash
# Always include relevant state files
@folder src/store                     # Redux/Zustand store
@folder src/contexts                  # React Context
@file src/types/storeTypes.ts         # State type definitions
```

Each use case demonstrates the comprehensive context needed for Cline to provide accurate, actionable solutions. The key is providing enough context while being specific about the problem, expected outcome, and current state of the codebase.