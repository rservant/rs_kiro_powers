# Lint Mastery for Zero Error Enforcement

This steering file provides comprehensive lint compliance guidance as part of the zero error enforcement framework. It covers ESLint configuration, advanced rules, and lint-specific error resolution patterns.

## ESLint Configuration Mastery

### Comprehensive ESLint Setup

**Complete ESLint Configuration**
```javascript
// .eslintrc.js - Production-ready configuration
module.exports = {
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaVersion: 2022,
    sourceType: 'module',
    project: './tsconfig.json',
    ecmaFeatures: {
      jsx: true,
    },
  },
  plugins: [
    '@typescript-eslint',
    'react',
    'react-hooks',
    'import',
    'jsx-a11y',
    'prefer-arrow',
  ],
  extends: [
    'eslint:recommended',
    '@typescript-eslint/recommended',
    '@typescript-eslint/recommended-requiring-type-checking',
    'plugin:react/recommended',
    'plugin:react-hooks/recommended',
    'plugin:import/recommended',
    'plugin:import/typescript',
    'plugin:jsx-a11y/recommended',
  ],
  rules: {
    // TypeScript-specific rules (ZERO TOLERANCE)
    '@typescript-eslint/no-explicit-any': 'error',
    '@typescript-eslint/no-unused-vars': 'error',
    '@typescript-eslint/explicit-function-return-type': 'warn',
    '@typescript-eslint/no-non-null-assertion': 'error',
    '@typescript-eslint/prefer-nullish-coalescing': 'error',
    '@typescript-eslint/prefer-optional-chain': 'error',
    '@typescript-eslint/no-floating-promises': 'error',
    '@typescript-eslint/await-thenable': 'error',
    '@typescript-eslint/no-misused-promises': 'error',

    // React-specific rules (ZERO TOLERANCE)
    'react-hooks/rules-of-hooks': 'error',
    'react-hooks/exhaustive-deps': 'error',
    'react/prop-types': 'off', // Using TypeScript instead
    'react/react-in-jsx-scope': 'off', // React 17+
    'react/jsx-uses-react': 'off', // React 17+
    'react/jsx-uses-vars': 'error',
    'react/no-unused-state': 'error',
    'react/no-direct-mutation-state': 'error',

    // Import/Export rules
    'import/order': [
      'error',
      {
        groups: [
          'builtin',
          'external',
          'internal',
          'parent',
          'sibling',
          'index',
        ],
        'newlines-between': 'always',
        alphabetize: { order: 'asc', caseInsensitive: true },
      },
    ],
    'import/no-unresolved': 'error',
    'import/no-unused-modules': 'warn',

    // Code quality rules (ZERO TOLERANCE)
    'no-console': 'error',
    'no-debugger': 'error',
    'no-alert': 'error',
    'no-unused-vars': 'off', // Using TypeScript version
    'prefer-const': 'error',
    'no-var': 'error',
    'eqeqeq': 'error',
    'curly': 'error',

    // Accessibility rules
    'jsx-a11y/alt-text': 'error',
    'jsx-a11y/anchor-has-content': 'error',
    'jsx-a11y/aria-props': 'error',
    'jsx-a11y/aria-proptypes': 'error',
    'jsx-a11y/aria-unsupported-elements': 'error',
    'jsx-a11y/role-has-required-aria-props': 'error',
    'jsx-a11y/role-supports-aria-props': 'error',
  },
  settings: {
    react: {
      version: 'detect',
    },
    'import/resolver': {
      typescript: {
        alwaysTryTypes: true,
        project: './tsconfig.json',
      },
    },
  },
  env: {
    browser: true,
    node: true,
    es2022: true,
    jest: true,
  },
  ignorePatterns: [
    'dist/',
    'build/',
    'node_modules/',
    '*.config.js',
    '*.config.ts',
  ],
}
```

### Rule Categories and Priorities

**Critical Rules (Build-Breaking)**
```javascript
// These rules MUST be errors - zero tolerance
const criticalRules = {
  '@typescript-eslint/no-explicit-any': 'error',
  '@typescript-eslint/no-unused-vars': 'error',
  'react-hooks/rules-of-hooks': 'error',
  'react-hooks/exhaustive-deps': 'error',
  'no-console': 'error',
  'import/no-unresolved': 'error',
}
```

**High Priority Rules (Should be Errors)**
```javascript
const highPriorityRules = {
  '@typescript-eslint/no-floating-promises': 'error',
  '@typescript-eslint/await-thenable': 'error',
  '@typescript-eslint/no-misused-promises': 'error',
  'prefer-const': 'error',
  'no-var': 'error',
  'eqeqeq': 'error',
}
```

**Medium Priority Rules (Warnings OK)**
```javascript
const mediumPriorityRules = {
  '@typescript-eslint/explicit-function-return-type': 'warn',
  'import/no-unused-modules': 'warn',
  '@typescript-eslint/prefer-nullish-coalescing': 'warn',
}
```

## Advanced Lint Error Resolution

### Systematic `any` Type Elimination

**The Unknown-First Pattern**
```typescript
// BAD: Direct any casting
const result = data as any as SpecificType

// GOOD: Safe type assertion via unknown
const result = data as unknown as SpecificType

// BETTER: Type guard function
function isSpecificType(obj: unknown): obj is SpecificType {
  return typeof obj === 'object' && 
         obj !== null && 
         'requiredProperty' in obj
}

if (isSpecificType(data)) {
  // TypeScript knows this is SpecificType
  console.log(data.requiredProperty)
}
```

**Helper Function Pattern for Common Conversions**
```typescript
// Create reusable type-safe helpers
function parseIntSafe(value: unknown): number {
  if (typeof value === 'string') {
    const parsed = parseInt(value, 10)
    return isNaN(parsed) ? 0 : parsed
  }
  if (typeof value === 'number') {
    return Math.floor(value)
  }
  return 0
}

function ensureString(value: unknown): string {
  return typeof value === 'string' ? value : String(value ?? '')
}

function parseJsonSafe<T>(json: string, fallback: T): T {
  try {
    const parsed = JSON.parse(json)
    return parsed as T
  } catch {
    return fallback
  }
}

// Usage examples
const userId = parseIntSafe(request.params.id)
const name = ensureString(userData.name)
const config = parseJsonSafe(configString, defaultConfig)
```

**Interface Creation for Test Mocks**
```typescript
// BAD: Using Partial for mocks
const mockScraper: Partial<ToastmastersScraper> = {
  scrapeDistrictData: vi.fn(),
}

// GOOD: Dedicated mock interface
interface MockToastmastersScraper {
  scrapeDistrictData: Mock<any[], Promise<DistrictData>>
  scrapeClubData: Mock<any[], Promise<ClubData>>
  scrapeEventData: Mock<any[], Promise<EventData>>
}

const mockScraper: MockToastmastersScraper = {
  scrapeDistrictData: vi.fn().mockResolvedValue(mockDistrictData),
  scrapeClubData: vi.fn().mockResolvedValue(mockClubData),
  scrapeEventData: vi.fn().mockResolvedValue(mockEventData),
}
```

### React Hooks Dependency Resolution

**Exhaustive Dependencies Pattern**
```typescript
// BAD: Missing dependencies
useEffect(() => {
  fetchUserData(userId, apiKey)
}, []) // Missing userId and apiKey

// GOOD: All dependencies included
useEffect(() => {
  fetchUserData(userId, apiKey)
}, [userId, apiKey])

// BETTER: Memoized callback to reduce dependencies
const fetchUser = useCallback(async () => {
  await fetchUserData(userId, apiKey)
}, [userId, apiKey])

useEffect(() => {
  fetchUser()
}, [fetchUser])

// BEST: Custom hook for complex logic
function useUserData(userId: string, apiKey: string) {
  const [userData, setUserData] = useState<UserData | null>(null)
  const [loading, setLoading] = useState(false)
  const [error, setError] = useState<Error | null>(null)

  useEffect(() => {
    let cancelled = false
    
    async function fetchData() {
      setLoading(true)
      setError(null)
      
      try {
        const data = await fetchUserData(userId, apiKey)
        if (!cancelled) {
          setUserData(data)
        }
      } catch (err) {
        if (!cancelled) {
          setError(err instanceof Error ? err : new Error(String(err)))
        }
      } finally {
        if (!cancelled) {
          setLoading(false)
        }
      }
    }

    fetchData()
    
    return () => {
      cancelled = true
    }
  }, [userId, apiKey])

  return { userData, loading, error }
}
```

**Component Definition Outside Render**
```typescript
// BAD: Component defined inside render
function ParentComponent() {
  const ChildComponent = ({ data }: { data: string }) => (
    <div>{data}</div>
  )
  
  return <ChildComponent data="test" />
}

// GOOD: Component defined outside
const ChildComponent = ({ data }: { data: string }) => (
  <div>{data}</div>
)

function ParentComponent() {
  return <ChildComponent data="test" />
}

// BETTER: Separate file for reusable components
// ChildComponent.tsx
export const ChildComponent = ({ data }: { data: string }) => (
  <div>{data}</div>
)

// ParentComponent.tsx
import { ChildComponent } from './ChildComponent'

export function ParentComponent() {
  return <ChildComponent data="test" />
}
```

### Import Organization and Unused Variables

**Automated Import Fixes**
```bash
# Fix import order automatically
npx eslint --fix --rule "import/order: error" "src/**/*.{ts,tsx}"

# Remove unused imports
npx eslint --fix --rule "@typescript-eslint/no-unused-vars: error" "src/**/*.{ts,tsx}"

# Organize imports in VS Code
# Add to settings.json:
{
  "editor.codeActionsOnSave": {
    "source.organizeImports": true
  }
}
```

**Manual Import Organization Pattern**
```typescript
// GOOD: Proper import organization
// 1. Node modules (external libraries)
import React, { useState, useEffect, useCallback } from 'react'
import { Router } from 'express'
import axios from 'axios'

// 2. Internal modules (absolute imports)
import { UserService } from '@/services/UserService'
import { ApiClient } from '@/utils/ApiClient'
import { CONFIG } from '@/config'

// 3. Relative imports (same directory and subdirectories)
import { UserCard } from './UserCard'
import { UserModal } from './UserModal'
import { validateUser } from '../utils/validation'

// 4. Type-only imports (separate section)
import type { User, UserPreferences } from '@/types/User'
import type { ApiResponse } from '@/types/Api'
```

## Lint Performance Optimization

### Caching and Incremental Linting

**ESLint Cache Configuration**
```javascript
// .eslintrc.js
module.exports = {
  // ... other config
  cache: true,
  cacheLocation: 'node_modules/.cache/eslint/',
  cacheStrategy: 'content', // or 'metadata'
}
```

**Package.json Scripts for Performance**
```json
{
  "scripts": {
    "lint": "eslint . --ext .ts,.tsx,.js,.jsx --cache",
    "lint:fix": "eslint . --ext .ts,.tsx,.js,.jsx --cache --fix",
    "lint:changed": "eslint $(git diff --name-only --diff-filter=ACMR | grep -E '\\.(ts|tsx|js|jsx)$' | xargs) --cache",
    "lint:staged": "lint-staged"
  }
}
```

**Lint-Staged Configuration for Speed**
```json
{
  "lint-staged": {
    "*.{ts,tsx,js,jsx}": [
      "eslint --cache --fix",
      "prettier --write"
    ],
    "*.{json,md,yml,yaml}": [
      "prettier --write"
    ]
  }
}
```

### IDE Integration Optimization

**VS Code Settings for Performance**
```json
{
  "eslint.validate": [
    "javascript",
    "javascriptreact", 
    "typescript",
    "typescriptreact"
  ],
  "eslint.run": "onType",
  "eslint.codeAction.showDocumentation": {
    "enable": true
  },
  "eslint.format.enable": false,
  "eslint.lintTask.enable": true,
  "eslint.workingDirectories": [
    { "mode": "auto" }
  ]
}
```

## Advanced Lint Patterns

### Custom ESLint Rules

**Creating Project-Specific Rules**
```javascript
// eslint-rules/no-direct-api-calls.js
module.exports = {
  meta: {
    type: 'problem',
    docs: {
      description: 'Disallow direct API calls outside service layer',
      category: 'Best Practices',
    },
    schema: [],
  },
  create(context) {
    return {
      CallExpression(node) {
        if (
          node.callee.type === 'MemberExpression' &&
          node.callee.object.name === 'fetch'
        ) {
          const filename = context.getFilename()
          if (!filename.includes('/services/')) {
            context.report({
              node,
              message: 'Direct API calls should be in service layer',
            })
          }
        }
      },
    }
  },
}

// .eslintrc.js
module.exports = {
  // ... other config
  rules: {
    'local/no-direct-api-calls': 'error',
  },
  plugins: ['local'],
}
```

### Lint Rule Exceptions and Documentation

**Proper Exception Handling**
```typescript
// GOOD: Specific disable with justification and timeline
// eslint-disable-next-line @typescript-eslint/no-explicit-any
const legacyData: any = thirdPartyLibrary.getData() 
// TODO: Add proper types when library updates to v2.0 (Q2 2024)

// GOOD: File-level disable with detailed explanation
/* eslint-disable @typescript-eslint/no-explicit-any */
/**
 * Legacy integration file for ThirdPartyLib v1.x
 * 
 * This file contains necessary `any` types due to:
 * 1. ThirdPartyLib lacks TypeScript definitions
 * 2. Dynamic object structures that change at runtime
 * 3. Complex nested data that would require 50+ interface definitions
 * 
 * Migration plan:
 * - Q1 2024: Evaluate ThirdPartyLib v2.x with TypeScript support
 * - Q2 2024: Migrate to typed version or create comprehensive interfaces
 * - Q3 2024: Remove all `any` types from this file
 * 
 * Last reviewed: 2024-01-15
 * Next review: 2024-04-15
 */

// BAD: Generic disable without explanation
// eslint-disable-next-line @typescript-eslint/no-explicit-any
const data: any = getData()
```

### Lint Metrics and Monitoring

**Automated Lint Reporting**
```bash
# Create lint metrics script
cat > scripts/lint-metrics.sh << 'EOF'
#!/bin/bash

echo "=== LINT METRICS REPORT ==="
echo "Generated: $(date)"
echo ""

# Run lint and capture output
npm run lint --silent > lint-output.txt 2>&1 || true

# Count errors and warnings
ERRORS=$(grep -c "error" lint-output.txt || echo "0")
WARNINGS=$(grep -c "warning" lint-output.txt || echo "0")

echo "Total Errors: $ERRORS"
echo "Total Warnings: $WARNINGS"
echo ""

# Count by rule type
echo "=== ERROR BREAKDOWN ==="
grep "error" lint-output.txt | \
  sed -E 's/.*\s+([a-z-]+\/[a-z-]+)\s+.*/\1/' | \
  sort | uniq -c | sort -nr | head -10

echo ""
echo "=== WARNING BREAKDOWN ==="
grep "warning" lint-output.txt | \
  sed -E 's/.*\s+([a-z-]+\/[a-z-]+)\s+.*/\1/' | \
  sort | uniq -c | sort -nr | head -10

# Generate JSON report for dashboards
cat > lint-metrics.json << EOL
{
  "timestamp": "$(date -u +%Y-%m-%dT%H:%M:%SZ)",
  "errors": $ERRORS,
  "warnings": $WARNINGS,
  "total_issues": $((ERRORS + WARNINGS)),
  "zero_error_compliant": $([ $ERRORS -eq 0 ] && echo "true" || echo "false")
}
EOL

echo ""
echo "Zero Error Compliant: $([ $ERRORS -eq 0 ] && echo "✅ YES" || echo "❌ NO ($ERRORS errors)")"
EOF

chmod +x scripts/lint-metrics.sh
```

## Troubleshooting Common Lint Issues

### ESLint Configuration Problems

**Issue: Rules not being applied**
```bash
# Debug ESLint configuration
npx eslint --print-config src/index.ts

# Check which config files are being used
npx eslint --debug src/index.ts 2>&1 | grep "Config"

# Verify parser and plugins are loaded
npx eslint --debug src/index.ts 2>&1 | grep -E "(Parser|Plugin)"
```

**Issue: Performance problems**
```bash
# Profile ESLint performance
TIMING=1 npx eslint src/

# Check which rules are slowest
npx eslint src/ --debug 2>&1 | grep "Rule.*took"

# Use faster parser options
# In .eslintrc.js:
parserOptions: {
  project: './tsconfig.json',
  createDefaultProgram: false, // Faster but less accurate
}
```

**Issue: IDE integration not working**
```json
// VS Code troubleshooting settings
{
  "eslint.trace.server": "verbose",
  "eslint.debug": true,
  "eslint.workingDirectories": [
    { "mode": "auto" }
  ]
}
```

### Rule-Specific Troubleshooting

**TypeScript ESLint Issues**
```bash
# Check TypeScript project configuration
npx tsc --showConfig

# Verify ESLint can find TypeScript config
npx eslint --print-config src/index.ts | grep -A 5 "parserOptions"

# Test specific TypeScript rules
npx eslint --no-eslintrc --parser @typescript-eslint/parser --rule '@typescript-eslint/no-explicit-any: error' src/index.ts
```

**React Hooks Issues**
```typescript
// Common exhaustive-deps fixes
useEffect(() => {
  // If you need to exclude a dependency, document why
  fetchData(userId)
  // eslint-disable-next-line react-hooks/exhaustive-deps
}, [userId]) // Excluding apiKey because it never changes

// Better: Use useCallback to stabilize dependencies
const stableApiKey = useCallback(() => apiKey, [])
useEffect(() => {
  fetchData(userId, stableApiKey())
}, [userId, stableApiKey])

// Best: Move stable values outside component or use useRef
const API_KEY = 'stable-key' // Outside component

function Component() {
  const apiKeyRef = useRef(apiKey)
  
  useEffect(() => {
    fetchData(userId, apiKeyRef.current)
  }, [userId])
}
```

## Integration with Zero Error Framework

### Lint as Part of Quality Gates

**Pre-commit Integration**
```bash
# .husky/pre-commit
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

echo "🔍 Running quality checks..."

# Lint check (zero tolerance)
npm run lint
if [ $? -ne 0 ]; then
  echo "❌ Lint errors found. Zero error policy violated."
  echo "Run 'npm run lint:fix' to auto-fix issues."
  exit 1
fi

echo "✅ Lint check passed"
```

**CI/CD Integration**
```yaml
# GitHub Actions
- name: Lint Check (Zero Error Policy)
  run: |
    npm run lint
    if [ $? -ne 0 ]; then
      echo "::error::Lint errors detected. Zero error policy violated."
      exit 1
    fi
```

### Monitoring and Metrics

**Quality Dashboard Integration**
```bash
# Send lint metrics to monitoring system
LINT_ERRORS=$(npm run lint 2>&1 | grep -c "error" || echo "0")
LINT_WARNINGS=$(npm run lint 2>&1 | grep -c "warning" || echo "0")

curl -X POST "https://metrics.company.com/api/quality" \
  -H "Content-Type: application/json" \
  -d "{
    \"repository\": \"$GITHUB_REPOSITORY\",
    \"timestamp\": \"$(date -u +%Y-%m-%dT%H:%M:%SZ)\",
    \"lint_errors\": $LINT_ERRORS,
    \"lint_warnings\": $LINT_WARNINGS,
    \"zero_error_compliant\": $([ $LINT_ERRORS -eq 0 ] && echo "true" || echo "false")
  }"
```

This comprehensive lint mastery guide provides the deep technical knowledge needed to achieve and maintain zero lint errors as part of the broader zero error enforcement framework.