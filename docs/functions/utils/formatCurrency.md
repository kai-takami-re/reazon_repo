# formatCurrency

Formats numeric values as currency strings with proper localization and symbol placement.

## Signature

```typescript
function formatCurrency(
  amount: number,
  currency: string,
  locale?: string,
  options?: CurrencyFormatOptions
): string
```

## Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| amount | number | Yes | - | The numeric amount to format |
| currency | string | Yes | - | ISO 4217 currency code (e.g., 'USD', 'EUR', 'GBP') |
| locale | string | No | 'en-US' | BCP 47 language tag for localization |
| options | CurrencyFormatOptions | No | {} | Additional formatting options |

### CurrencyFormatOptions

```typescript
interface CurrencyFormatOptions {
  minimumFractionDigits?: number; // Minimum decimal places (default: 2)
  maximumFractionDigits?: number; // Maximum decimal places (default: 2)
  useGrouping?: boolean;          // Use thousands separators (default: true)
  currencyDisplay?: 'symbol' | 'narrowSymbol' | 'code' | 'name'; // Display format (default: 'symbol')
  signDisplay?: 'auto' | 'never' | 'always' | 'exceptZero'; // Sign display (default: 'auto')
}
```

## Return Value

Returns a formatted currency string according to the specified locale and options.

**Type**: `string`

## Examples

### Basic Usage

```typescript
import { formatCurrency } from '@reazon/utils';

// Basic USD formatting
const price = formatCurrency(1234.56, 'USD');
console.log(price); // "$1,234.56"

// Euro formatting
const euroPrice = formatCurrency(1234.56, 'EUR');
console.log(euroPrice); // "€1,234.56"

// British Pound
const gbpPrice = formatCurrency(1234.56, 'GBP');
console.log(gbpPrice); // "£1,234.56"
```

### Localization Examples

```typescript
// German locale with Euro
const germanPrice = formatCurrency(1234.56, 'EUR', 'de-DE');
console.log(germanPrice); // "1.234,56 €"

// Japanese locale with Yen
const japanesePrice = formatCurrency(1234, 'JPY', 'ja-JP');
console.log(japanesePrice); // "¥1,234"

// French locale with Euro
const frenchPrice = formatCurrency(1234.56, 'EUR', 'fr-FR');
console.log(frenchPrice); // "1 234,56 €"
```

### Custom Options

```typescript
// No decimal places
const wholeNumber = formatCurrency(1234.56, 'USD', 'en-US', {
  minimumFractionDigits: 0,
  maximumFractionDigits: 0
});
console.log(wholeNumber); // "$1,235"

// Four decimal places
const precise = formatCurrency(1234.5678, 'USD', 'en-US', {
  minimumFractionDigits: 4,
  maximumFractionDigits: 4
});
console.log(precise); // "$1,234.5678"

// Currency code instead of symbol
const withCode = formatCurrency(1234.56, 'USD', 'en-US', {
  currencyDisplay: 'code'
});
console.log(withCode); // "USD 1,234.56"

// Currency name
const withName = formatCurrency(1234.56, 'USD', 'en-US', {
  currencyDisplay: 'name'
});
console.log(withName); // "1,234.56 US dollars"
```

### Sign Display Options

```typescript
// Always show sign
const alwaysSign = formatCurrency(-1234.56, 'USD', 'en-US', {
  signDisplay: 'always'
});
console.log(alwaysSign); // "-$1,234.56"

const positiveWithSign = formatCurrency(1234.56, 'USD', 'en-US', {
  signDisplay: 'always'
});
console.log(positiveWithSign); // "+$1,234.56"

// Never show sign
const neverSign = formatCurrency(-1234.56, 'USD', 'en-US', {
  signDisplay: 'never'
});
console.log(neverSign); // "$1,234.56"
```

### Grouping Options

```typescript
// Disable thousands separators
const noGrouping = formatCurrency(1234567.89, 'USD', 'en-US', {
  useGrouping: false
});
console.log(noGrouping); // "$1234567.89"
```

## Edge Cases

### Zero and Small Values

```typescript
// Zero value
const zero = formatCurrency(0, 'USD');
console.log(zero); // "$0.00"

// Very small values
const small = formatCurrency(0.01, 'USD');
console.log(small); // "$0.01"

// Fractional cents (rounded)
const fractional = formatCurrency(0.126, 'USD');
console.log(fractional); // "$0.13"
```

### Large Numbers

```typescript
// Large amounts
const million = formatCurrency(1000000, 'USD');
console.log(million); // "$1,000,000.00"

const billion = formatCurrency(1000000000, 'USD');
console.log(billion); // "$1,000,000,000.00"
```

### Negative Values

```typescript
// Negative amounts
const negative = formatCurrency(-1234.56, 'USD');
console.log(negative); // "-$1,234.56"

// Negative with parentheses (accounting style)
const accounting = formatCurrency(-1234.56, 'USD', 'en-US', {
  signDisplay: 'exceptZero'
});
console.log(accounting); // "-$1,234.56"
```

## Error Handling

The function throws specific errors for invalid inputs:

### ValidationError

```typescript
import { formatCurrency, ValidationError } from '@reazon/utils';

try {
  // Invalid currency code
  formatCurrency(100, 'INVALID');
} catch (error) {
  if (error instanceof ValidationError) {
    console.log(error.message); // "Invalid currency code: INVALID"
    console.log(error.field);   // "currency"
    console.log(error.code);    // "INVALID_CURRENCY"
  }
}

try {
  // Invalid locale
  formatCurrency(100, 'USD', 'invalid-locale');
} catch (error) {
  if (error instanceof ValidationError) {
    console.log(error.message); // "Invalid locale: invalid-locale"
    console.log(error.field);   // "locale"
    console.log(error.code);    // "INVALID_LOCALE"
  }
}
```

### Common Error Scenarios

```typescript
// Non-numeric amount
try {
  formatCurrency(NaN, 'USD');
} catch (error) {
  // ValidationError: "Amount must be a valid number"
}

// Infinite values
try {
  formatCurrency(Infinity, 'USD');
} catch (error) {
  // ValidationError: "Amount must be finite"
}

// Null or undefined
try {
  formatCurrency(null as any, 'USD');
} catch (error) {
  // ValidationError: "Amount is required"
}
```

## Performance

- **Time Complexity**: O(1) - Constant time operation
- **Space Complexity**: O(1) - No additional space allocation
- **Caching**: The function caches `Intl.NumberFormat` instances for repeated locale/currency combinations

### Performance Tips

```typescript
// For repeated formatting with same locale/currency, the formatter is cached
const prices = [10.99, 25.50, 150.00];
const formattedPrices = prices.map(price => formatCurrency(price, 'USD'));
// Second and third calls reuse cached formatter

// For high-performance scenarios, consider pre-creating formatters
import { createCurrencyFormatter } from '@reazon/utils';

const usdFormatter = createCurrencyFormatter('USD', 'en-US');
const formatted = prices.map(price => usdFormatter.format(price));
```

## Browser Support

| Feature | Chrome | Firefox | Safari | Edge | Node.js |
|---------|--------|---------|--------|------|---------|
| Basic formatting | 24+ | 29+ | 10+ | 12+ | 13.0.0+ |
| All options | 77+ | 78+ | 14.1+ | 79+ | 13.0.0+ |

## Related Functions

- [parseCurrency](./parseCurrency.md) - Parse currency strings back to numbers
- [formatNumber](./formatNumber.md) - General number formatting
- [convertCurrency](./convertCurrency.md) - Currency conversion utilities
- [getCurrencySymbol](./getCurrencySymbol.md) - Extract currency symbols

## Migration Guide

### v1.x to v2.x

```typescript
// v1.x (deprecated)
formatCurrency(100, { currency: 'USD', locale: 'en-US' });

// v2.x (current)
formatCurrency(100, 'USD', 'en-US');
```

### v2.x to v3.x (upcoming)

No breaking changes planned. New options may be added for cryptocurrency support.

## Testing

### Unit Tests

```typescript
import { describe, it, expect } from '@jest/globals';
import { formatCurrency, ValidationError } from '../formatCurrency';

describe('formatCurrency', () => {
  describe('basic formatting', () => {
    it('formats USD correctly', () => {
      expect(formatCurrency(1234.56, 'USD')).toBe('$1,234.56');
    });

    it('formats EUR correctly', () => {
      expect(formatCurrency(1234.56, 'EUR')).toBe('€1,234.56');
    });

    it('handles zero values', () => {
      expect(formatCurrency(0, 'USD')).toBe('$0.00');
    });

    it('handles negative values', () => {
      expect(formatCurrency(-1234.56, 'USD')).toBe('-$1,234.56');
    });
  });

  describe('localization', () => {
    it('formats with German locale', () => {
      expect(formatCurrency(1234.56, 'EUR', 'de-DE')).toBe('1.234,56 €');
    });

    it('formats with Japanese locale', () => {
      expect(formatCurrency(1234, 'JPY', 'ja-JP')).toBe('¥1,234');
    });
  });

  describe('custom options', () => {
    it('respects minimum fraction digits', () => {
      expect(formatCurrency(100, 'USD', 'en-US', {
        minimumFractionDigits: 0
      })).toBe('$100');
    });

    it('uses currency code display', () => {
      expect(formatCurrency(100, 'USD', 'en-US', {
        currencyDisplay: 'code'
      })).toBe('USD 100.00');
    });
  });

  describe('error handling', () => {
    it('throws for invalid currency', () => {
      expect(() => formatCurrency(100, 'INVALID')).toThrow(ValidationError);
    });

    it('throws for invalid locale', () => {
      expect(() => formatCurrency(100, 'USD', 'invalid')).toThrow(ValidationError);
    });

    it('throws for non-numeric amount', () => {
      expect(() => formatCurrency(NaN, 'USD')).toThrow(ValidationError);
    });
  });
});
```

## Changelog

### v2.1.0
- Added `signDisplay` option for controlling sign visibility
- Improved error messages with specific error codes
- Added caching for better performance

### v2.0.0
- **BREAKING**: Changed parameter order (amount, currency, locale, options)
- Added comprehensive TypeScript definitions
- Added support for all `Intl.NumberFormat` options
- Improved error handling with custom error types

### v1.2.0
- Added support for cryptocurrency symbols
- Improved browser compatibility
- Added performance optimizations