# Contributing to Documentation

Thank you for contributing to the reazon_repo documentation! This guide will help you create high-quality, consistent documentation that benefits all users.

## Table of Contents

- [Documentation Standards](#documentation-standards)
- [Writing Guidelines](#writing-guidelines)
- [API Documentation](#api-documentation)
- [Component Documentation](#component-documentation)
- [Function Documentation](#function-documentation)
- [Examples and Code Samples](#examples-and-code-samples)
- [Review Process](#review-process)
- [Tools and Templates](#tools-and-templates)

## Documentation Standards

### Core Principles

1. **Clarity**: Write for your audience. Assume readers are intelligent but not familiar with the specific implementation.
2. **Completeness**: Include all necessary information for successful implementation.
3. **Accuracy**: Keep documentation synchronized with code changes.
4. **Consistency**: Follow established patterns and formats.
5. **Accessibility**: Write for developers of all experience levels.

### Content Structure

Every documentation page should include:

- **Overview**: Brief description of purpose and functionality
- **Quick Start**: Minimal example to get started
- **Detailed API**: Complete reference with all options
- **Examples**: Real-world usage scenarios
- **Error Handling**: Common issues and solutions
- **Related Items**: Links to related documentation

## Writing Guidelines

### Language and Tone

- **Use active voice**: "This function returns..." not "The result is returned by..."
- **Be concise**: Remove unnecessary words while maintaining clarity
- **Use present tense**: "The API returns..." not "The API will return..."
- **Avoid jargon**: Explain technical terms when first introduced
- **Be specific**: Use exact parameter names, types, and values

### Formatting Standards

#### Headers

Use descriptive, hierarchical headers:

```markdown
# Main Topic
## Major Section
### Subsection
#### Detail
```

#### Code Blocks

Always specify the language for syntax highlighting:

```typescript
// ✅ Good - with language
function example() {
  return 'hello';
}
```

```
// ❌ Bad - no language specified
function example() {
  return 'hello';
}
```

#### Links

Use descriptive link text:

```markdown
// ✅ Good
See the [Button component documentation](./components/Button.md) for details.

// ❌ Bad
See [here](./components/Button.md) for details.
```

#### Tables

Use tables for structured information:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| name | string | Yes | User's full name |
| age | number | No | User's age in years |

### Code Examples

#### Complete Examples

Provide runnable examples whenever possible:

```typescript
// ✅ Complete example with imports
import { Button } from '@reazon/components';
import { useState } from 'react';

function LoginForm() {
  const [loading, setLoading] = useState(false);

  const handleSubmit = async () => {
    setLoading(true);
    // ... implementation
    setLoading(false);
  };

  return (
    <Button loading={loading} onClick={handleSubmit}>
      Sign In
    </Button>
  );
}
```

#### Error Scenarios

Show what happens when things go wrong:

```typescript
try {
  const result = await apiCall();
  console.log(result);
} catch (error) {
  if (error instanceof ValidationError) {
    console.error('Validation failed:', error.message);
  } else {
    console.error('Unexpected error:', error);
  }
}
```

## API Documentation

### Endpoint Documentation

Each API endpoint must include:

1. **HTTP Method and URL**
2. **Authentication requirements**
3. **Request parameters** (path, query, body)
4. **Response format** (success and error)
5. **Status codes**
6. **Rate limiting**
7. **Code examples** in multiple languages

#### Template

```markdown
# Endpoint Name

Brief description of what this endpoint does.

## Request

`POST /api/v1/endpoint`

### Authentication
Requires Bearer token.

### Parameters

#### Path Parameters
| Name | Type | Description |
|------|------|-------------|
| id | string | Resource identifier |

#### Query Parameters
| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| limit | number | No | 20 | Items per page |

#### Request Body
\`\`\`json
{
  "name": "string",
  "email": "string"
}
\`\`\`

## Response

### Success (200 OK)
\`\`\`json
{
  "success": true,
  "data": {
    "id": "user_123",
    "name": "John Doe"
  }
}
\`\`\`

### Error (400 Bad Request)
\`\`\`json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid email format"
  }
}
\`\`\`

## Examples

### cURL
\`\`\`bash
curl -X POST "https://api.example.com/v1/endpoint" \\
  -H "Authorization: Bearer TOKEN" \\
  -H "Content-Type: application/json" \\
  -d '{"name": "John", "email": "john@example.com"}'
\`\`\`

### JavaScript
\`\`\`javascript
const response = await fetch('/api/v1/endpoint', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer ' + token,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    name: 'John',
    email: 'john@example.com'
  })
});
\`\`\`
```

## Component Documentation

### Required Sections

1. **Import statement**
2. **Basic usage example**
3. **Props table** with types and descriptions
4. **Variants and states**
5. **Accessibility information**
6. **Styling customization**
7. **Testing examples**

### Props Documentation

Use TypeScript-style type definitions:

```markdown
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | 'primary' \| 'secondary' \| 'outline' | 'primary' | Visual style variant |
| size | 'sm' \| 'md' \| 'lg' | 'md' | Component size |
| disabled | boolean | false | Disables user interaction |
| children | ReactNode | - | Content to display |
| onClick | (event: MouseEvent) => void | - | Click event handler |
```

### Accessibility Documentation

Include keyboard navigation and screen reader information:

```markdown
## Accessibility

### Keyboard Navigation
- **Tab**: Moves focus to the component
- **Enter/Space**: Activates the component
- **Escape**: Closes modal/dropdown (if applicable)

### Screen Reader Support
- Uses semantic HTML elements
- Includes proper ARIA labels
- Announces state changes
```

## Function Documentation

### Required Elements

1. **Function signature** with TypeScript types
2. **Parameter table** with types and descriptions
3. **Return value** description
4. **Usage examples** with different scenarios
5. **Error handling** documentation
6. **Performance notes** (time/space complexity)

### Type Documentation

Always include complete TypeScript definitions:

```typescript
function processData<T extends DataType>(
  data: T[],
  options: ProcessingOptions<T>
): Promise<ProcessedResult<T>>

interface ProcessingOptions<T> {
  filter?: (item: T) => boolean;
  transform?: (item: T) => T;
  batch?: {
    size: number;
    delay: number;
  };
}
```

### Error Documentation

Document all possible error scenarios:

```markdown
## Error Handling

### ValidationError
Thrown when input validation fails.

\`\`\`typescript
try {
  const result = validateData(input);
} catch (error) {
  if (error instanceof ValidationError) {
    console.log(error.field); // Field that failed validation
    console.log(error.code);  // Error code
  }
}
\`\`\`

### Common Errors
- `INVALID_EMAIL`: Email format is incorrect
- `REQUIRED_FIELD`: Required field is missing
- `VALUE_TOO_LARGE`: Value exceeds maximum limit
```

## Examples and Code Samples

### Guidelines for Examples

1. **Start simple**: Begin with basic usage
2. **Build complexity**: Show advanced features progressively
3. **Include imports**: Always show required imports
4. **Handle errors**: Demonstrate proper error handling
5. **Real-world scenarios**: Use realistic examples

### Example Structure

```markdown
## Examples

### Basic Usage
\`\`\`typescript
// Simple case
const result = functionName('input');
\`\`\`

### With Options
\`\`\`typescript
// With configuration
const result = functionName('input', {
  option1: true,
  option2: 'value'
});
\`\`\`

### Error Handling
\`\`\`typescript
// Proper error handling
try {
  const result = functionName('input');
  console.log(result);
} catch (error) {
  console.error('Operation failed:', error.message);
}
\`\`\`

### Real-World Example
\`\`\`typescript
// Complete implementation
import { functionName, ErrorType } from '@reazon/utils';

async function processUserData(userData: UserData) {
  try {
    const validated = functionName(userData);
    await saveToDatabase(validated);
    return { success: true, data: validated };
  } catch (error) {
    if (error instanceof ErrorType) {
      return { success: false, error: error.message };
    }
    throw error; // Re-throw unexpected errors
  }
}
\`\`\`
```

## Review Process

### Before Submitting

1. **Spell check**: Use a spell checker
2. **Link validation**: Ensure all links work
3. **Code testing**: Verify all code examples run
4. **Format check**: Follow markdown guidelines
5. **Completeness**: Include all required sections

### Review Checklist

- [ ] Clear and concise writing
- [ ] Complete code examples with imports
- [ ] Proper TypeScript type definitions
- [ ] Error scenarios documented
- [ ] Accessibility information included
- [ ] Performance notes where relevant
- [ ] Links to related documentation
- [ ] Consistent formatting and style

### Common Issues

#### Incomplete Examples

```typescript
// ❌ Bad - missing imports and context
const button = <Button>Click me</Button>;

// ✅ Good - complete example
import React from 'react';
import { Button } from '@reazon/components';

function MyComponent() {
  return (
    <Button onClick={() => console.log('clicked')}>
      Click me
    </Button>
  );
}
```

#### Vague Descriptions

```markdown
<!-- ❌ Bad -->
This function processes data.

<!-- ✅ Good -->
Validates user input data according to the specified schema and returns 
a sanitized object with type-safe properties.
```

#### Missing Error Information

```markdown
<!-- ❌ Bad -->
## Error Handling
The function may throw errors.

<!-- ✅ Good -->
## Error Handling

### ValidationError
Thrown when input fails schema validation.
- **field**: The property that failed validation
- **code**: Specific error code (e.g., 'INVALID_EMAIL')
- **message**: Human-readable error description
```

## Tools and Templates

### Markdown Linting

Use markdownlint for consistent formatting:

```bash
npm install -g markdownlint-cli
markdownlint docs/**/*.md
```

### Link Checking

Validate all links before committing:

```bash
npm install -g markdown-link-check
find docs -name "*.md" -exec markdown-link-check {} \;
```

### Documentation Templates

Use the provided templates:

- [API Endpoint Template](./templates/api-endpoint.md)
- [Component Template](./templates/component.md)
- [Function Template](./templates/function.md)

### Auto-generation Tools

For TypeScript projects, consider:

- **TypeDoc**: Generate API docs from code comments
- **Storybook**: Component documentation and testing
- **JSDoc**: Function documentation from comments

## Maintenance

### Keeping Documentation Current

1. **Code reviews**: Require documentation updates with code changes
2. **Regular audits**: Review documentation quarterly
3. **User feedback**: Monitor documentation issues and requests
4. **Automated checks**: Use CI/CD to validate documentation

### Deprecation Process

When documenting deprecated features:

```markdown
## ⚠️ Deprecated

> **Warning**: This function is deprecated as of v2.0.0 and will be removed in v3.0.0.
> Use [newFunction](./newFunction.md) instead.

### Migration Example

\`\`\`typescript
// Old way (deprecated)
const result = oldFunction(data);

// New way
const result = newFunction(data);
\`\`\`
```

### Version Management

Tag documentation with version information:

```markdown
## Changelog

### v2.1.0
- Added new `loading` prop
- Improved accessibility with ARIA labels

### v2.0.0 (Breaking Changes)
- **BREAKING**: Renamed `color` prop to `variant`
- Removed deprecated `size` values
```

## Getting Help

If you need assistance with documentation:

1. **Check existing docs**: Look for similar patterns
2. **Ask in discussions**: Use GitHub Discussions for questions
3. **Review guidelines**: Re-read this contributing guide
4. **Request review**: Ask for feedback on draft documentation

## Resources

- [Markdown Guide](https://www.markdownguide.org/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [MDN Web Docs Writing Style](https://developer.mozilla.org/en-US/docs/MDN/Writing_guidelines/Writing_style_guide)
- [Google Developer Documentation Style Guide](https://developers.google.com/style)