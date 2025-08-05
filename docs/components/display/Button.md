# Button Component

A versatile, accessible button component with multiple variants, sizes, and states.

## Overview

The Button component is a fundamental UI element that triggers actions when clicked. It supports various visual styles, sizes, and states while maintaining full accessibility compliance.

## Import

```tsx
import { Button } from '@reazon/components';
```

## Basic Usage

```tsx
function BasicExample() {
  return (
    <Button onClick={() => console.log('Clicked!')}>
      Click me
    </Button>
  );
}
```

## API Reference

### Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | `'primary' \| 'secondary' \| 'outline' \| 'ghost' \| 'danger'` | `'primary'` | Visual style variant |
| size | `'xs' \| 'sm' \| 'md' \| 'lg' \| 'xl'` | `'md'` | Button size |
| disabled | `boolean` | `false` | Whether the button is disabled |
| loading | `boolean` | `false` | Shows loading spinner |
| fullWidth | `boolean` | `false` | Makes button full width |
| icon | `ReactNode` | - | Icon to display |
| iconPosition | `'left' \| 'right'` | `'left'` | Position of the icon |
| type | `'button' \| 'submit' \| 'reset'` | `'button'` | HTML button type |
| children | `ReactNode` | - | Button content |
| onClick | `(event: MouseEvent) => void` | - | Click handler |
| onFocus | `(event: FocusEvent) => void` | - | Focus handler |
| onBlur | `(event: FocusEvent) => void` | - | Blur handler |

### Ref

The Button component forwards refs to the underlying HTML button element.

```tsx
const buttonRef = useRef<HTMLButtonElement>(null);

<Button ref={buttonRef}>
  Button with ref
</Button>
```

## Variants

### Primary (Default)

```tsx
<Button variant="primary">
  Primary Button
</Button>
```

### Secondary

```tsx
<Button variant="secondary">
  Secondary Button
</Button>
```

### Outline

```tsx
<Button variant="outline">
  Outline Button
</Button>
```

### Ghost

```tsx
<Button variant="ghost">
  Ghost Button
</Button>
```

### Danger

```tsx
<Button variant="danger">
  Danger Button
</Button>
```

## Sizes

```tsx
<div className="flex gap-4 items-center">
  <Button size="xs">Extra Small</Button>
  <Button size="sm">Small</Button>
  <Button size="md">Medium</Button>
  <Button size="lg">Large</Button>
  <Button size="xl">Extra Large</Button>
</div>
```

## States

### Disabled

```tsx
<Button disabled>
  Disabled Button
</Button>
```

### Loading

```tsx
<Button loading>
  Loading Button
</Button>
```

### Full Width

```tsx
<Button fullWidth>
  Full Width Button
</Button>
```

## With Icons

### Icon on Left (Default)

```tsx
import { PlusIcon } from '@heroicons/react/24/outline';

<Button icon={<PlusIcon />}>
  Add Item
</Button>
```

### Icon on Right

```tsx
import { ArrowRightIcon } from '@heroicons/react/24/outline';

<Button 
  icon={<ArrowRightIcon />}
  iconPosition="right"
>
  Continue
</Button>
```

### Icon Only

```tsx
import { HeartIcon } from '@heroicons/react/24/outline';

<Button 
  icon={<HeartIcon />}
  aria-label="Like"
/>
```

## Form Integration

```tsx
function LoginForm() {
  const handleSubmit = (event: FormEvent) => {
    event.preventDefault();
    // Handle form submission
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="email" name="email" />
      <input type="password" name="password" />
      
      <div className="flex gap-2">
        <Button type="submit" variant="primary">
          Sign In
        </Button>
        <Button type="button" variant="outline">
          Cancel
        </Button>
      </div>
    </form>
  );
}
```

## Styling

### CSS Variables

The Button component uses CSS custom properties for theming:

```css
.button {
  /* Primary variant */
  --button-primary-bg: var(--color-primary-500);
  --button-primary-color: white;
  --button-primary-border: var(--color-primary-500);
  
  /* Secondary variant */
  --button-secondary-bg: var(--color-secondary-100);
  --button-secondary-color: var(--color-secondary-900);
  --button-secondary-border: var(--color-secondary-200);
  
  /* Sizes */
  --button-xs-height: 24px;
  --button-xs-padding: 0 8px;
  --button-xs-font-size: 12px;
  
  --button-sm-height: 32px;
  --button-sm-padding: 0 12px;
  --button-sm-font-size: 14px;
  
  --button-md-height: 40px;
  --button-md-padding: 0 16px;
  --button-md-font-size: 16px;
  
  --button-lg-height: 48px;
  --button-lg-padding: 0 20px;
  --button-lg-font-size: 18px;
  
  --button-xl-height: 56px;
  --button-xl-padding: 0 24px;
  --button-xl-font-size: 20px;
}
```

### Custom Styling with styled-components

```tsx
import styled from 'styled-components';
import { Button } from '@reazon/components';

const GradientButton = styled(Button)`
  background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
  border: none;
  color: white;
  
  &:hover {
    background: linear-gradient(45deg, #ff5252, #26c6da);
  }
`;
```

### Custom Styling with CSS Classes

```tsx
<Button className="custom-button">
  Custom Styled Button
</Button>
```

```css
.custom-button {
  background: #ff6b6b;
  border-radius: 20px;
  box-shadow: 0 4px 12px rgba(255, 107, 107, 0.3);
}
```

## Accessibility

The Button component is fully accessible and follows WCAG 2.1 AA guidelines:

### Keyboard Navigation
- **Enter/Space**: Activates the button
- **Tab**: Moves focus to the next focusable element
- **Shift+Tab**: Moves focus to the previous focusable element

### Screen Reader Support
- Proper semantic markup using `<button>` element
- Supports `aria-label` for icon-only buttons
- Loading state announced to screen readers
- Disabled state properly communicated

### Focus Management
- Visible focus indicator
- Focus is maintained during state changes
- Focus is properly managed in loading state

### Example with Accessibility Features

```tsx
<Button
  aria-label="Add new item to cart"
  aria-describedby="cart-help-text"
  onClick={addToCart}
  loading={isLoading}
>
  Add to Cart
</Button>

<div id="cart-help-text" className="sr-only">
  This will add the item to your shopping cart
</div>
```

## Testing

### Unit Testing with Jest and React Testing Library

```tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from './Button';

describe('Button', () => {
  it('renders children correctly', () => {
    render(<Button>Test Button</Button>);
    expect(screen.getByRole('button', { name: 'Test Button' })).toBeInTheDocument();
  });

  it('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    
    fireEvent.click(screen.getByRole('button'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  it('does not call onClick when disabled', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick} disabled>Disabled</Button>);
    
    fireEvent.click(screen.getByRole('button'));
    expect(handleClick).not.toHaveBeenCalled();
  });

  it('shows loading spinner when loading', () => {
    render(<Button loading>Loading</Button>);
    expect(screen.getByRole('status')).toBeInTheDocument();
  });

  it('applies correct variant classes', () => {
    render(<Button variant="secondary">Secondary</Button>);
    expect(screen.getByRole('button')).toHaveClass('button--secondary');
  });
});
```

### Integration Testing

```tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { ThemeProvider } from '@reazon/components';
import { Button } from './Button';

describe('Button Integration', () => {
  it('works with theme provider', () => {
    const theme = {
      colors: { primary: '#ff0000' }
    };

    render(
      <ThemeProvider theme={theme}>
        <Button variant="primary">Themed Button</Button>
      </ThemeProvider>
    );

    const button = screen.getByRole('button');
    expect(button).toHaveStyle({ backgroundColor: '#ff0000' });
  });
});
```

## Performance Considerations

### Memoization

For buttons that receive complex props or event handlers, consider using `React.memo`:

```tsx
import { memo } from 'react';

const OptimizedButton = memo(Button);

// Use when parent re-renders frequently
<OptimizedButton onClick={stableClickHandler}>
  Optimized Button
</OptimizedButton>
```

### Event Handler Optimization

```tsx
// ❌ Avoid creating new functions on each render
<Button onClick={() => handleClick(id)}>
  Click me
</Button>

// ✅ Use useCallback for stable references
const handleButtonClick = useCallback(() => {
  handleClick(id);
}, [id]);

<Button onClick={handleButtonClick}>
  Click me
</Button>
```

## Related Components

- [IconButton](./IconButton.md) - Button specifically for icons
- [LinkButton](./LinkButton.md) - Button styled as a link
- [ButtonGroup](./ButtonGroup.md) - Group of related buttons
- [LoadingButton](./LoadingButton.md) - Button with built-in loading states

## Changelog

### v2.1.0
- Added `loading` prop with spinner
- Added `fullWidth` prop
- Improved accessibility with better ARIA support

### v2.0.0
- **BREAKING**: Removed deprecated `color` prop (use `variant` instead)
- Added `ghost` variant
- Improved TypeScript definitions
- Enhanced keyboard navigation

### v1.5.0
- Added icon support with `icon` and `iconPosition` props
- Added `xs` and `xl` sizes
- Improved focus management