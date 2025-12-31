---
name: "zero-error-enforcement"
displayName: "Zero Error Enforcement"
description: "Comprehensive quality enforcement system for achieving and maintaining zero errors across lint, TypeScript, formatting, tests, and builds. Includes automated monitoring, CI/CD integration, team workflow guidance, and deep lint mastery techniques."
keywords: ["zero errors", "quality enforcement", "lint compliance", "eslint", "typescript", "prettier", "formatting", "testing", "ci/cd", "automation", "code quality"]
author: "Engineering Standards Team"
---

# Zero Error Enforcement

## Overview

The Zero Error Enforcement Power provides a comprehensive framework for achieving and maintaining zero errors across all dimensions of code quality. This power combines proven strategies from successful zero-error implementations with practical tools and workflows for enforcement.

**What this power covers:**
- **Multi-dimensional error tracking** across lint, TypeScript, formatting, tests, and builds
- **Comprehensive lint mastery** including ESLint configuration, advanced rules, and error resolution patterns
- **Automated enforcement mechanisms** including pre-commit hooks and CI/CD integration
- **Team workflow optimization** for sustainable zero-error maintenance
- **Error resolution strategies** with prioritization and systematic approaches
- **Monitoring and reporting** systems for continuous quality improvement

**Why zero errors matter:**
- Prevents production bugs and runtime failures
- Maintains consistent code quality across teams
- Enables safe, frequent deployments
- Reduces technical debt accumulation
- Improves developer productivity and confidence

This power is based on real-world implementations that have successfully achieved and maintained zero errors across large codebases with multiple contributors.

## Available Steering Files

This power includes specialized steering files for different aspects of zero error enforcement:

- **ci-cd-integration** - Complete CI/CD pipeline setup with quality gates and automated enforcement
- **error-resolution** - Systematic approaches for resolving existing errors and preventing new ones
- **team-workflows** - Team processes and workflows for sustainable zero-error maintenance
- **lint-mastery** - Deep dive into ESLint configuration, rules, and advanced lint compliance techniques

Call action "readSteering" to access specific guidance as needed.

## Core Principles

All zero error enforcement MUST follow these principles:

1. **Zero tolerance for errors**  
   No errors are permitted in any dimension - lint, TypeScript, formatting, tests, or builds.

2. **Prevention over correction**  
   Automated systems prevent error introduction rather than relying on manual fixes.

3. **Fail fast and fail clearly**  
   Quality gates provide immediate, actionable feedback when errors are introduced.

4. **Systematic error resolution**  
   Existing errors are resolved through prioritized, methodical approaches.

5. **Continuous monitoring and improvement**  
   Quality metrics are tracked and systems are continuously optimized.

## Quick Start Implementation

### 1. Install Required Tools

```bash
# Core linting and formatting tools
npm install --save-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
npm install --save-dev prettier eslint-config-prettier

# React support (if using React)
npm install --save-dev eslint-plugin-react eslint-plugin-react-hooks

# Import organization and accessibility
npm install --save-dev eslint-plugin-import eslint-plugin-jsx-a11y

# Pre-commit hooks
npm install --save-dev husky lint-staged
```

### 2. Basic Configuration Setup

Create `.eslintrc.js`:
```javascript
module.exports = {
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint', 'react', 'react-hooks', 'import'],
  extends: [
    'eslint:recommended',
    '@typescript-eslint/recommended',
    'plugin:react/recommended',
    'plugin:react-hooks/recommended',
  ],
  rules: {
    '@typescript-eslint/no-explicit-any': 'error',
    '@typescript-eslint/no-unused-vars': 'error',
    'react-hooks/rules-of-hooks': 'error',
    'react-hooks/exhaustive-deps': 'error',
    'no-console': 'error',
    'prefer-const': 'error',
  },
  env: {
    browser: true,
    node: true,
    es2022: true,
    jest: true,
  },
}
```

Create `.prettierrc`:
```json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2
}
```

### 3. Pre-commit Hooks Setup

```bash
# Initialize husky
npx husky install

# Add pre-commit hook
npx husky add .husky/pre-commit "npx lint-staged"
```

Configure `lint-staged` in `package.json`:
```json
{
  "lint-staged": {
    "*.{ts,tsx,js,jsx}": [
      "eslint --fix",
      "prettier --write"
    ],
    "*.{json,md,yml,yaml}": [
      "prettier --write"
    ]
  }
}
```

### 4. Essential Package.json Scripts

Add these scripts to your `package.json`:
```json
{
  "scripts": {
    "lint": "eslint . --ext .ts,.tsx,.js,.jsx --cache",
    "lint:fix": "eslint . --ext .ts,.tsx,.js,.jsx --cache --fix",
    "format": "prettier --write .",
    "format:check": "prettier --check .",
    "type-check": "tsc --noEmit --skipLibCheck",
    "test": "jest --passWithNoTests",
    "build": "tsc && vite build",
    "quality:check": "npm run lint && npm run type-check && npm run format:check && npm test",
    "quality:fix": "npm run lint:fix && npm run format"
  }
}
```

### 5. Initial Quality Assessment

Run comprehensive assessment to establish baseline:
```bash
# Check current error state
npm run quality:check

# Generate detailed reports for tracking
npm run lint -- --format json --output-file lint-report.json
npm run type-check > typescript-errors.log 2>&1 || true
npm test -- --coverage > test-report.log 2>&1 || true
```

## Error Dimensions

### 1. Lint Compliance
- **ESLint errors**: Code quality and best practice violations
- **ESLint warnings**: Potential issues and style inconsistencies
- **React-specific rules**: Hooks usage, component patterns, performance
- **TypeScript-specific rules**: Type safety, proper typing patterns

### 2. TypeScript Errors
- **Compilation errors**: Code that fails to compile
- **Type safety violations**: Implicit any, unsafe assertions
- **Strict mode compliance**: Null checks, function types, unused variables
- **Type coverage**: Percentage of properly typed code

### 3. Formatting Violations
- **Prettier compliance**: Consistent code formatting
- **JSON syntax**: Valid JSON structure and formatting
- **Markdown formatting**: Consistent documentation style
- **Import organization**: Proper import sorting and grouping

### 4. Test Failures
- **Unit test failures**: Business logic and component testing
- **Integration test failures**: Service and API contract testing
- **End-to-end test failures**: Critical user journey validation
- **Test coverage**: Minimum coverage thresholds

### 5. Build Failures
- **Compilation failures**: TypeScript, bundling, and asset processing
- **Dependency issues**: Package resolution and compatibility
- **Environment configuration**: Missing variables and setup issues
- **Deployment validation**: Production readiness checks

## Quality Gates and Enforcement

### Pre-commit Enforcement
```bash
# Install pre-commit hooks
npm install --save-dev husky lint-staged

# Configure package.json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.{ts,tsx,js,jsx}": [
      "eslint --fix",
      "prettier --write",
      "git add"
    ],
    "*.{json,md}": [
      "prettier --write",
      "git add"
    ]
  }
}
```

### CI/CD Pipeline Requirements
Every pipeline MUST include these quality gates:

1. **Lint Check**
   ```bash
   npm run lint
   # Must return exit code 0
   ```

2. **TypeScript Compilation**
   ```bash
   npx tsc --noEmit --skipLibCheck
   # Must return exit code 0
   ```

3. **Formatting Check**
   ```bash
   npm run format:check
   # Must return exit code 0
   ```

4. **Test Execution**
   ```bash
   npm test -- --run --coverage
   # Must return exit code 0
   ```

5. **Build Validation**
   ```bash
   npm run build
   # Must return exit code 0
   ```

### Branch Protection Rules
Configure repository settings to require:
- All quality checks pass before merge
- Up-to-date branches with main/master
- At least one approving review
- No force pushes to protected branches

## Error Resolution Strategy

### Phase 1: Assessment and Baseline
1. **Run comprehensive audit** across all error dimensions
2. **Categorize errors** by type, severity, and impact
3. **Establish baseline metrics** for tracking progress
4. **Create resolution timeline** with milestones

### Phase 2: Critical Error Resolution
**Priority Order:**
1. **Build-breaking errors** (prevents compilation/deployment)
2. **TypeScript compilation errors** (type safety violations)
3. **Test failures** (broken functionality)
4. **High-severity lint errors** (explicit any, unused variables)
5. **Formatting violations** (consistency issues)

### Phase 3: Systematic Cleanup
**Approach:**
- Fix errors in small, focused batches
- Test thoroughly after each batch
- Commit frequently with clear messages
- Track progress against baseline metrics

### Phase 4: Prevention Implementation
- Install and configure pre-commit hooks
- Set up CI/CD quality gates
- Configure branch protection rules
- Train team on new workflows

## Monitoring and Metrics

### Key Performance Indicators
Track these metrics to measure success:

- **Total error count** across all dimensions
- **Error introduction rate** (new errors per commit/PR)
- **Error resolution rate** (errors fixed per sprint)
- **Time to resolution** (how quickly errors are fixed)
- **Quality gate pass rate** (percentage of successful builds)

### Reporting Dashboard
Create automated reports showing:
- Current error counts by dimension
- Trend analysis over time
- Team performance metrics
- Quality gate effectiveness
- Cost of quality (time spent on error resolution)

### Alert Thresholds
Configure alerts for:
- Any new errors introduced
- Quality gate failures
- Unusual error patterns
- Performance degradation in quality metrics

## Team Workflows

### Daily Workflow
1. **Start with clean slate** - verify all quality checks pass
2. **Develop with quality in mind** - fix errors as they appear
3. **Pre-commit validation** - ensure hooks catch issues
4. **Monitor CI/CD results** - address failures immediately

### Pull Request Process
1. **Author responsibilities:**
   - Ensure all quality checks pass locally
   - Include error count impact in PR description
   - Fix any CI/CD failures before requesting review

2. **Reviewer responsibilities:**
   - Verify quality checks pass in CI/CD
   - Review error resolution approaches
   - Ensure no new technical debt introduced

### Sprint Planning
- **Include error resolution** in sprint capacity planning
- **Assign error cleanup tasks** to team members
- **Track progress** against quality improvement goals
- **Review and adjust** quality standards as needed

## Configuration Examples

### ESLint Configuration
```json
{
  "extends": [
    "@typescript-eslint/recommended",
    "@typescript-eslint/recommended-requiring-type-checking",
    "plugin:react/recommended",
    "plugin:react-hooks/recommended"
  ],
  "rules": {
    "@typescript-eslint/no-explicit-any": "error",
    "@typescript-eslint/no-unused-vars": "error",
    "@typescript-eslint/explicit-function-return-type": "warn",
    "@typescript-eslint/no-floating-promises": "error",
    "@typescript-eslint/await-thenable": "error",
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "error",
    "no-console": "error",
    "prefer-const": "error",
    "import/order": ["error", {
      "groups": ["builtin", "external", "internal", "parent", "sibling", "index"],
      "newlines-between": "always",
      "alphabetize": { "order": "asc", "caseInsensitive": true }
    }]
  }
}
```

### TypeScript Configuration
```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "noImplicitReturns": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "exactOptionalPropertyTypes": true
  }
}
```

### Prettier Configuration
```json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false
}
```

### Package.json Scripts
```json
{
  "scripts": {
    "lint": "eslint . --ext .ts,.tsx,.js,.jsx --cache",
    "lint:fix": "eslint . --ext .ts,.tsx,.js,.jsx --cache --fix",
    "format": "prettier --write .",
    "format:check": "prettier --check .",
    "type-check": "tsc --noEmit --skipLibCheck",
    "test": "jest --passWithNoTests",
    "test:coverage": "jest --coverage --passWithNoTests",
    "build": "tsc && vite build",
    "quality:check": "npm run lint && npm run type-check && npm run format:check && npm test",
    "quality:fix": "npm run lint:fix && npm run format"
  }
}
```

## Success Stories and Case Studies

### Case Study: Toast Stats Project
**Achievement:** Complete zero error compliance across 1,090+ tests

**Results:**
- **Backend**: 596 tests passed, 2 skipped (99.7% pass rate)
- **Frontend**: 494 tests passed (100% pass rate)
- **Lint Errors**: 0 across entire codebase
- **TypeScript Errors**: 0 across entire codebase
- **Formatting Violations**: 0 across entire codebase

**Key Success Factors:**
- Systematic error resolution approach
- Comprehensive pre-commit hook implementation
- CI/CD pipeline enforcement
- Team commitment to zero-tolerance policy
- Regular monitoring and maintenance

**Maintenance Mode:**
- Automated prevention of new errors
- Continuous compliance monitoring
- Performance optimization ongoing
- Team adherence to established workflows

## Troubleshooting Common Issues

### Pre-commit Hooks Not Working
**Symptoms:**
- Commits succeed despite having errors
- Hooks don't run automatically

**Solutions:**
1. Verify husky installation: `npx husky install`
2. Check hook permissions: `chmod +x .husky/pre-commit`
3. Reinstall hooks: `npm run prepare`
4. Test manually: `npx lint-staged`

### CI/CD Quality Gates Failing
**Symptoms:**
- Pipeline fails on quality checks
- Inconsistent results between local and CI

**Solutions:**
1. Ensure same tool versions in CI and local
2. Check environment variables and configuration
3. Verify file paths and working directories
4. Review CI/CD logs for specific error messages

### Performance Issues with Quality Checks
**Symptoms:**
- Slow pre-commit hooks
- Long CI/CD pipeline execution times

**Solutions:**
1. Optimize lint-staged configuration for changed files only
2. Use incremental TypeScript compilation
3. Implement parallel test execution
4. Cache dependencies and build artifacts

### Team Resistance to Zero Error Policy
**Symptoms:**
- Developers bypassing quality checks
- Complaints about development velocity

**Solutions:**
1. Provide training on tools and workflows
2. Demonstrate long-term benefits and ROI
3. Optimize tooling for better performance
4. Celebrate quality achievements and milestones

## Best Practices

### Development Practices
- **Fix errors immediately** when they appear during development
- **Use IDE integrations** for real-time error detection
- **Write tests first** to prevent regression errors
- **Refactor incrementally** to maintain quality standards
- **Document exceptions** when temporary workarounds are needed

### Tool Configuration
- **Use strict configurations** for maximum error detection
- **Enable all recommended rules** unless explicitly justified
- **Configure IDE extensions** for consistent development experience
- **Maintain tool versions** across team and CI/CD environments
- **Regular tool updates** with testing and validation

### Team Processes
- **Regular quality reviews** to assess effectiveness
- **Continuous improvement** of tools and processes
- **Knowledge sharing** of quality techniques and solutions
- **Mentoring and training** for new team members
- **Recognition and rewards** for quality achievements

## Migration Planning

### For Existing Projects
1. **Assessment Phase** (1-2 weeks)
   - Audit current error state across all dimensions
   - Categorize and prioritize errors
   - Estimate resolution effort and timeline

2. **Foundation Phase** (2-4 weeks)
   - Set up tooling and configuration
   - Implement basic quality gates
   - Begin systematic error resolution

3. **Resolution Phase** (4-12 weeks, depending on codebase size)
   - Execute systematic error cleanup
   - Implement prevention mechanisms
   - Train team on new workflows

4. **Maintenance Phase** (ongoing)
   - Monitor quality metrics
   - Continuous improvement
   - Regular tool and process updates

### For New Projects
1. **Start with strict configuration** from day one
2. **Implement all quality gates** before first commit
3. **Train team** on zero error workflows
4. **Monitor and adjust** as project grows

## Final Rule

> **Zero errors are the foundation of reliable, maintainable software.**  
> **Quality is not negotiable - it's a prerequisite for sustainable development.**  
> **Every error prevented is a production bug avoided.**

**Enforcement**: Zero error policies will be validated through automated systems and regular audits.

**Accountability**: All team members are responsible for maintaining quality standards and preventing error introduction.

**Continuous Improvement**: Regular reviews of quality systems to ensure they support development velocity while maintaining excellence.