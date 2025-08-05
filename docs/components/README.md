# Component Documentation

Comprehensive documentation for all reusable UI components in the reazon_repo project.

## Overview

This section contains detailed documentation for all frontend components, including React components, Vue components, and web components. Each component includes:

- **API Reference**: Props, events, and methods
- **Usage Examples**: Code samples and live demos
- **Design Guidelines**: Visual specifications and best practices
- **Accessibility**: WCAG compliance and keyboard navigation
- **Testing**: Unit tests and integration examples

## Component Categories

### Layout Components
- [Container](./layout/Container.md) - Responsive container with max-width constraints
- [Grid](./layout/Grid.md) - Flexible grid system for layouts
- [Flex](./layout/Flex.md) - Flexbox utilities and components
- [Sidebar](./layout/Sidebar.md) - Collapsible sidebar navigation
- [Header](./layout/Header.md) - Application header with navigation
- [Footer](./layout/Footer.md) - Application footer

### Navigation Components
- [Navbar](./navigation/Navbar.md) - Main navigation bar
- [Breadcrumb](./navigation/Breadcrumb.md) - Breadcrumb navigation
- [Tabs](./navigation/Tabs.md) - Tab navigation component
- [Pagination](./navigation/Pagination.md) - Page navigation controls
- [Menu](./navigation/Menu.md) - Dropdown and context menus

### Form Components
- [Input](./forms/Input.md) - Text input field
- [TextArea](./forms/TextArea.md) - Multi-line text input
- [Select](./forms/Select.md) - Dropdown selection
- [Checkbox](./forms/Checkbox.md) - Checkbox input
- [Radio](./forms/Radio.md) - Radio button input
- [Switch](./forms/Switch.md) - Toggle switch
- [DatePicker](./forms/DatePicker.md) - Date selection component
- [FileUpload](./forms/FileUpload.md) - File upload component
- [Form](./forms/Form.md) - Form wrapper with validation

### Display Components
- [Button](./display/Button.md) - Clickable button component
- [Badge](./display/Badge.md) - Status and notification badges
- [Card](./display/Card.md) - Content card container
- [Table](./display/Table.md) - Data table with sorting and filtering
- [List](./display/List.md) - Ordered and unordered lists
- [Avatar](./display/Avatar.md) - User profile pictures
- [Image](./display/Image.md) - Responsive image component
- [Icon](./display/Icon.md) - Icon component with SVG support

### Feedback Components
- [Alert](./feedback/Alert.md) - Alert messages and notifications
- [Toast](./feedback/Toast.md) - Temporary notification messages
- [Modal](./feedback/Modal.md) - Modal dialogs and overlays
- [Tooltip](./feedback/Tooltip.md) - Contextual tooltips
- [Spinner](./feedback/Spinner.md) - Loading spinners
- [ProgressBar](./feedback/ProgressBar.md) - Progress indicators
- [Skeleton](./feedback/Skeleton.md) - Loading skeleton screens

### Data Visualization
- [Chart](./charts/Chart.md) - Chart component wrapper
- [LineChart](./charts/LineChart.md) - Line chart visualization
- [BarChart](./charts/BarChart.md) - Bar chart visualization
- [PieChart](./charts/PieChart.md) - Pie chart visualization
- [Dashboard](./charts/Dashboard.md) - Dashboard layout component

## Design System

### Theme Configuration
```typescript
interface Theme {
  colors: {
    primary: string;
    secondary: string;
    success: string;
    warning: string;
    error: string;
    info: string;
    background: string;
    surface: string;
    text: string;
  };
  typography: {
    fontFamily: string;
    fontSize: {
      xs: string;
      sm: string;
      md: string;
      lg: string;
      xl: string;
    };
    fontWeight: {
      light: number;
      normal: number;
      medium: number;
      bold: number;
    };
  };
  spacing: {
    xs: string;
    sm: string;
    md: string;
    lg: string;
    xl: string;
  };
  breakpoints: {
    sm: string;
    md: string;
    lg: string;
    xl: string;
  };
}
```

### Color Palette
```css
:root {
  /* Primary Colors */
  --color-primary-50: #eff6ff;
  --color-primary-500: #3b82f6;
  --color-primary-900: #1e3a8a;
  
  /* Secondary Colors */
  --color-secondary-50: #f8fafc;
  --color-secondary-500: #64748b;
  --color-secondary-900: #0f172a;
  
  /* Status Colors */
  --color-success: #10b981;
  --color-warning: #f59e0b;
  --color-error: #ef4444;
  --color-info: #3b82f6;
}
```

### Typography Scale
```css
:root {
  --font-size-xs: 0.75rem;    /* 12px */
  --font-size-sm: 0.875rem;   /* 14px */
  --font-size-md: 1rem;       /* 16px */
  --font-size-lg: 1.125rem;   /* 18px */
  --font-size-xl: 1.25rem;    /* 20px */
  --font-size-2xl: 1.5rem;    /* 24px */
  --font-size-3xl: 1.875rem;  /* 30px */
  --font-size-4xl: 2.25rem;   /* 36px */
}
```

## Component Usage Patterns

### Basic Component Usage
```tsx
import { Button } from '@reazon/components';

function App() {
  return (
    <Button 
      variant="primary" 
      size="md"
      onClick={() => console.log('clicked')}
    >
      Click me
    </Button>
  );
}
```

### Styled Components
```tsx
import styled from 'styled-components';
import { Button } from '@reazon/components';

const StyledButton = styled(Button)`
  background: linear-gradient(45deg, #fe6b8b 30%, #ff8e53 90%);
  border-radius: 3px;
  border: 0;
  color: white;
  height: 48px;
  padding: 0 30px;
  box-shadow: 0 3px 5px 2px rgba(255, 105, 135, .3);
`;
```

### Theme Provider
```tsx
import { ThemeProvider } from '@reazon/components';
import { theme } from './theme';

function App() {
  return (
    <ThemeProvider theme={theme}>
      <YourComponents />
    </ThemeProvider>
  );
}
```

## Accessibility Guidelines

All components follow WCAG 2.1 AA standards:

- **Keyboard Navigation**: All interactive components support keyboard navigation
- **Screen Reader Support**: Proper ARIA labels and semantic markup
- **Color Contrast**: Minimum 4.5:1 contrast ratio for text
- **Focus Management**: Visible focus indicators and logical tab order
- **Alternative Text**: Images include descriptive alt text

## Browser Support

| Browser | Version |
|---------|---------|
| Chrome | 90+ |
| Firefox | 88+ |
| Safari | 14+ |
| Edge | 90+ |
| iOS Safari | 14+ |
| Android Chrome | 90+ |

## Component Development

### Creating New Components

1. **File Structure**
```
src/components/
├── Button/
│   ├── Button.tsx
│   ├── Button.test.tsx
│   ├── Button.stories.tsx
│   ├── Button.module.css
│   └── index.ts
```

2. **Component Template**
```tsx
import React from 'react';
import { styled } from 'styled-components';

interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'outline';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  children: React.ReactNode;
  onClick?: () => void;
}

export const Button: React.FC<ButtonProps> = ({
  variant = 'primary',
  size = 'md',
  disabled = false,
  children,
  onClick,
  ...props
}) => {
  return (
    <StyledButton
      variant={variant}
      size={size}
      disabled={disabled}
      onClick={onClick}
      {...props}
    >
      {children}
    </StyledButton>
  );
};
```

3. **Testing Requirements**
```tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from './Button';

describe('Button', () => {
  it('renders with text', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });

  it('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    fireEvent.click(screen.getByText('Click me'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
});
```

## Contributing

See [Component Contributing Guide](../CONTRIBUTING.md#components) for detailed guidelines on:
- Code style and conventions
- Testing requirements
- Documentation standards
- Review process