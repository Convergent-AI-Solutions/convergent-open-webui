# Coding Best Practices

This steering guide enforces clean, readable code with minimal but effective comments.

## Core Principles

### Code Clarity
- Write self-documenting code through clear naming
- Use descriptive variable and function names
- Keep functions small and focused on single responsibility
- Prefer explicit over implicit behavior

### Minimal Comments
- Comment **why**, not **what**
- Only add comments when code intent isn't obvious
- Remove outdated or redundant comments
- Avoid commenting obvious operations

### Code Structure
- Use consistent indentation and formatting
- Group related functionality together
- Separate concerns into different modules/files
- Follow established conventions for the language

## Naming Conventions

### Variables and Functions
```javascript
// Good - clear intent
const userAge = 25;
const isAuthenticated = true;
function calculateTotalPrice(items) { }

// Bad - unclear purpose
const x = 25;
const flag = true;
function calc(arr) { }
```

### Constants and Classes
```javascript
// Good
const MAX_RETRY_ATTEMPTS = 3;
class UserRepository { }

// Bad
const max = 3;
class repo { }
```

## Function Design

### Keep Functions Small
- Single responsibility principle
- Maximum 20-30 lines when possible
- Extract complex logic into separate functions
- Use early returns to reduce nesting

### Clear Parameters
```javascript
// Good - explicit parameters
function createUser(email, password, role) { }

// Bad - unclear object parameter
function createUser(data) { }
```

## Comment Guidelines

### When to Comment
```javascript
// Good - explains business logic
// Apply 15% discount for premium users
const discountedPrice = price * 0.85;

// Bad - states the obvious
// Set price to price times 0.85
const discountedPrice = price * 0.85;
```

### Complex Logic Only
```javascript
// Good - explains non-obvious algorithm
// Binary search requires sorted array
function binarySearch(arr, target) {
    // Implementation here
}

// Bad - unnecessary comment
// Loop through array
for (let i = 0; i < arr.length; i++) { }
```

## Error Handling

### Be Explicit
```javascript
// Good - clear error handling
try {
    const result = await apiCall();
    return result;
} catch (error) {
    logger.error('API call failed', error);
    throw new Error('Unable to fetch data');
}

// Bad - silent failures
try {
    const result = await apiCall();
    return result;
} catch (error) {
    // Handle error somehow
}
```

## Code Organization

### File Structure
- One class/main function per file
- Group related utilities together
- Use clear file and folder names
- Keep imports at the top

### Consistent Formatting
- Use automated formatters (Prettier, Black, etc.)
- Consistent spacing and indentation
- Remove trailing whitespace
- End files with newline

## Language-Specific Guidelines

### JavaScript/TypeScript
- Use `const` by default, `let` when reassignment needed
- Prefer arrow functions for callbacks
- Use template literals for string interpolation
- Destructure objects and arrays when appropriate

### Python
- Follow PEP 8 style guide
- Use list comprehensions for simple transformations
- Prefer f-strings for formatting
- Use type hints for function parameters and returns

### General
- Avoid deep nesting (max 3-4 levels)
- Use guard clauses to reduce complexity
- Prefer composition over inheritance
- Write tests for complex logic

## What to Avoid

### Unnecessary Code
- Dead code and unused imports
- Commented-out code blocks
- Redundant variables
- Over-engineered solutions

### Poor Comments
```javascript
// Bad examples
// Increment i
i++;

// This function adds two numbers
function add(a, b) { return a + b; }

// TODO: Fix this later (without context)
```

### Complex Expressions
```javascript
// Bad - hard to read
const result = users.filter(u => u.active && u.role === 'admin' && u.lastLogin > Date.now() - 86400000).map(u => u.id);

// Good - broken down
const oneDayAgo = Date.now() - 86400000;
const activeAdmins = users.filter(user => 
    user.active && 
    user.role === 'admin' && 
    user.lastLogin > oneDayAgo
);
const adminIds = activeAdmins.map(user => user.id);
```

## Code Review Checklist

- [ ] Function and variable names are clear
- [ ] Comments explain why, not what
- [ ] No unnecessary code or comments
- [ ] Consistent formatting applied
- [ ] Error handling is explicit
- [ ] Functions have single responsibility
- [ ] Complex logic is broken down

---

*Write code as if the person maintaining it is a violent psychopath who knows where you live. Make it clear, make it simple, make it work.*