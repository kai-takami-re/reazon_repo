# Getting Started

Welcome to reazon_repo! This guide will help you get up and running quickly with the project.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Project Structure](#project-structure)
- [Development Setup](#development-setup)
- [Your First Implementation](#your-first-implementation)
- [Common Patterns](#common-patterns)
- [Next Steps](#next-steps)

## Prerequisites

Before you begin, ensure you have the following installed:

### Required

- **Node.js**: Version 18.0 or higher
- **npm**: Version 8.0 or higher (comes with Node.js)
- **Git**: For version control

### Recommended

- **VS Code**: With recommended extensions
- **Docker**: For containerized development (optional)

### System Requirements

| Platform | Minimum Version |
|----------|----------------|
| macOS | 10.15 (Catalina) |
| Windows | 10 (version 1903) |
| Linux | Ubuntu 18.04, CentOS 8, or equivalent |

## Installation

### Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-org/reazon_repo.git
   cd reazon_repo
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Set up environment**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

4. **Start development server**
   ```bash
   npm run dev
   ```

5. **Open in browser**
   Navigate to `http://localhost:3000`

### Package Manager Options

#### npm (Recommended)
```bash
npm install
npm run dev
```

#### Yarn
```bash
yarn install
yarn dev
```

#### pnpm
```bash
pnpm install
pnpm dev
```

## Project Structure

```
reazon_repo/
├── docs/                   # Documentation
│   ├── api/               # API documentation
│   ├── components/        # Component docs
│   ├── functions/         # Function docs
│   └── guides/            # Tutorials and guides
├── src/                   # Source code
│   ├── components/        # React components
│   ├── hooks/             # Custom React hooks
│   ├── utils/             # Utility functions
│   ├── services/          # API services
│   ├── types/             # TypeScript definitions
│   └── tests/             # Test files
├── public/                # Static assets
├── scripts/               # Build and deployment scripts
├── .env.example           # Environment variables template
├── package.json           # Project dependencies
├── tsconfig.json          # TypeScript configuration
└── README.md              # Project overview
```

### Key Directories

- **`src/components/`**: Reusable UI components
- **`src/hooks/`**: Custom React hooks for shared logic
- **`src/utils/`**: Pure utility functions
- **`src/services/`**: API calls and external integrations
- **`src/types/`**: TypeScript type definitions
- **`docs/`**: All project documentation

## Development Setup

### Environment Configuration

Create a `.env` file from the template:

```bash
cp .env.example .env
```

Edit the `.env` file with your settings:

```env
# API Configuration
API_BASE_URL=http://localhost:3001/api/v1
API_KEY=your-development-api-key

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/reazon_dev

# Authentication
JWT_SECRET=your-jwt-secret-key
AUTH_PROVIDER=local

# Feature Flags
ENABLE_ANALYTICS=false
ENABLE_DEBUG_MODE=true
```

### IDE Configuration

#### VS Code Extensions

Install recommended extensions:

```json
{
  "recommendations": [
    "ms-vscode.vscode-typescript-next",
    "bradlc.vscode-tailwindcss",
    "esbenp.prettier-vscode",
    "ms-vscode.vscode-eslint",
    "ms-vscode.vscode-jest"
  ]
}
```

#### Settings

Create `.vscode/settings.json`:

```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "typescript.preferences.importModuleSpecifier": "relative"
}
```

### Available Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start development server |
| `npm run build` | Build for production |
| `npm run test` | Run test suite |
| `npm run test:watch` | Run tests in watch mode |
| `npm run lint` | Lint code |
| `npm run lint:fix` | Fix linting issues |
| `npm run type-check` | Check TypeScript types |
| `npm run format` | Format code with Prettier |

## Your First Implementation

### 1. Create a Simple Component

Create `src/components/Welcome/Welcome.tsx`:

```tsx
import React from 'react';

interface WelcomeProps {
  name: string;
  onGetStarted?: () => void;
}

export const Welcome: React.FC<WelcomeProps> = ({ name, onGetStarted }) => {
  return (
    <div className="welcome-container">
      <h1>Welcome, {name}!</h1>
      <p>Thanks for using reazon_repo.</p>
      {onGetStarted && (
        <button onClick={onGetStarted} className="get-started-btn">
          Get Started
        </button>
      )}
    </div>
  );
};
```

### 2. Add Styling

Create `src/components/Welcome/Welcome.module.css`:

```css
.welcome-container {
  text-align: center;
  padding: 2rem;
  border-radius: 8px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  margin: 1rem;
}

.welcome-container h1 {
  margin-bottom: 1rem;
  font-size: 2rem;
}

.get-started-btn {
  background: white;
  color: #667eea;
  border: none;
  padding: 0.75rem 1.5rem;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 600;
  transition: transform 0.2s;
}

.get-started-btn:hover {
  transform: translateY(-2px);
}
```

### 3. Create a Custom Hook

Create `src/hooks/useLocalStorage.ts`:

```typescript
import { useState, useEffect } from 'react';

export function useLocalStorage<T>(
  key: string,
  initialValue: T
): [T, (value: T) => void] {
  // Get from local storage then parse stored json or return initialValue
  const [storedValue, setStoredValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.warn(`Error reading localStorage key "${key}":`, error);
      return initialValue;
    }
  });

  // Return a wrapped version of useState's setter function that persists the new value to localStorage
  const setValue = (value: T) => {
    try {
      setStoredValue(value);
      window.localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.error(`Error setting localStorage key "${key}":`, error);
    }
  };

  return [storedValue, setValue];
}
```

### 4. Add a Utility Function

Create `src/utils/formatters.ts`:

```typescript
/**
 * Formats a date for display
 */
export function formatDate(date: Date | string, locale = 'en-US'): string {
  const dateObj = typeof date === 'string' ? new Date(date) : date;
  
  return new Intl.DateTimeFormat(locale, {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  }).format(dateObj);
}

/**
 * Truncates text to specified length
 */
export function truncateText(text: string, maxLength: number): string {
  if (text.length <= maxLength) return text;
  return text.slice(0, maxLength) + '...';
}
```

### 5. Put It All Together

Update `src/App.tsx`:

```tsx
import React from 'react';
import { Welcome } from './components/Welcome/Welcome';
import { useLocalStorage } from './hooks/useLocalStorage';
import { formatDate } from './utils/formatters';

function App() {
  const [userName, setUserName] = useLocalStorage('userName', 'Developer');
  const [visitCount, setVisitCount] = useLocalStorage('visitCount', 0);

  React.useEffect(() => {
    setVisitCount(visitCount + 1);
  }, []);

  const handleGetStarted = () => {
    const newName = prompt('What\'s your name?', userName);
    if (newName) {
      setUserName(newName);
    }
  };

  return (
    <div className="app">
      <Welcome name={userName} onGetStarted={handleGetStarted} />
      <p>
        Visit #{visitCount} on {formatDate(new Date())}
      </p>
    </div>
  );
}

export default App;
```

### 6. Add Tests

Create `src/components/Welcome/Welcome.test.tsx`:

```tsx
import React from 'react';
import { render, screen, fireEvent } from '@testing-library/react';
import { Welcome } from './Welcome';

describe('Welcome Component', () => {
  it('displays the welcome message with name', () => {
    render(<Welcome name="John" />);
    expect(screen.getByText('Welcome, John!')).toBeInTheDocument();
  });

  it('calls onGetStarted when button is clicked', () => {
    const mockGetStarted = jest.fn();
    render(<Welcome name="John" onGetStarted={mockGetStarted} />);
    
    fireEvent.click(screen.getByText('Get Started'));
    expect(mockGetStarted).toHaveBeenCalledTimes(1);
  });

  it('does not render button when onGetStarted is not provided', () => {
    render(<Welcome name="John" />);
    expect(screen.queryByText('Get Started')).not.toBeInTheDocument();
  });
});
```

## Common Patterns

### Error Handling

```tsx
import React, { useState } from 'react';

interface AsyncComponentProps {
  userId: string;
}

export const AsyncComponent: React.FC<AsyncComponentProps> = ({ userId }) => {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);
  const [data, setData] = useState(null);

  const fetchData = async () => {
    try {
      setLoading(true);
      setError(null);
      const response = await fetch(`/api/users/${userId}`);
      
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      
      const result = await response.json();
      setData(result);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'An error occurred');
    } finally {
      setLoading(false);
    }
  };

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!data) return <div>No data</div>;

  return <div>{/* Render your data */}</div>;
};
```

### API Service Pattern

```typescript
// src/services/api.ts
class ApiService {
  private baseURL: string;

  constructor(baseURL: string) {
    this.baseURL = baseURL;
  }

  private async request<T>(
    endpoint: string,
    options: RequestInit = {}
  ): Promise<T> {
    const url = `${this.baseURL}${endpoint}`;
    
    const config: RequestInit = {
      headers: {
        'Content-Type': 'application/json',
        ...options.headers,
      },
      ...options,
    };

    const response = await fetch(url, config);
    
    if (!response.ok) {
      throw new Error(`API Error: ${response.status}`);
    }

    return response.json();
  }

  async getUser(id: string) {
    return this.request<User>(`/users/${id}`);
  }

  async createUser(userData: CreateUserData) {
    return this.request<User>('/users', {
      method: 'POST',
      body: JSON.stringify(userData),
    });
  }
}

export const api = new ApiService(process.env.API_BASE_URL || '');
```

### Form Handling

```tsx
import React, { useState } from 'react';

interface FormData {
  name: string;
  email: string;
}

export const ContactForm: React.FC = () => {
  const [formData, setFormData] = useState<FormData>({
    name: '',
    email: ''
  });
  const [errors, setErrors] = useState<Partial<FormData>>({});

  const validate = (data: FormData): Partial<FormData> => {
    const errors: Partial<FormData> = {};
    
    if (!data.name.trim()) {
      errors.name = 'Name is required';
    }
    
    if (!data.email.trim()) {
      errors.email = 'Email is required';
    } else if (!/\S+@\S+\.\S+/.test(data.email)) {
      errors.email = 'Email is invalid';
    }
    
    return errors;
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    
    const validationErrors = validate(formData);
    if (Object.keys(validationErrors).length > 0) {
      setErrors(validationErrors);
      return;
    }
    
    // Submit form
    console.log('Submitting:', formData);
  };

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    
    // Clear error when user starts typing
    if (errors[name as keyof FormData]) {
      setErrors(prev => ({ ...prev, [name]: undefined }));
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input
          type="text"
          name="name"
          placeholder="Name"
          value={formData.name}
          onChange={handleChange}
        />
        {errors.name && <span className="error">{errors.name}</span>}
      </div>
      
      <div>
        <input
          type="email"
          name="email"
          placeholder="Email"
          value={formData.email}
          onChange={handleChange}
        />
        {errors.email && <span className="error">{errors.email}</span>}
      </div>
      
      <button type="submit">Submit</button>
    </form>
  );
};
```

## Next Steps

### Explore the Documentation

1. **[API Reference](../api/README.md)** - Learn about available endpoints
2. **[Component Library](../components/README.md)** - Discover reusable components
3. **[Utility Functions](../functions/README.md)** - Explore helper functions
4. **[Examples](../examples/README.md)** - See real-world implementations

### Advanced Topics

- **State Management**: Learn about Redux, Zustand, or Context API patterns
- **Testing**: Dive deeper into unit and integration testing
- **Performance**: Optimize your application with code splitting and memoization
- **Deployment**: Set up CI/CD pipelines and production deployment

### Community and Support

- **GitHub Issues**: Report bugs or request features
- **Discussions**: Ask questions and share ideas
- **Documentation**: Contribute to improving the docs
- **Code Reviews**: Participate in the development process

### Development Workflow

1. **Create Feature Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make Changes**
   - Write code following the established patterns
   - Add tests for new functionality
   - Update documentation if needed

3. **Test Your Changes**
   ```bash
   npm run test
   npm run lint
   npm run type-check
   ```

4. **Commit and Push**
   ```bash
   git add .
   git commit -m "feat: add your feature description"
   git push origin feature/your-feature-name
   ```

5. **Create Pull Request**
   - Use the provided PR template
   - Include tests and documentation updates
   - Wait for code review

## Troubleshooting

### Common Issues

#### Port Already in Use
```bash
Error: Port 3000 is already in use
```
**Solution**: Use a different port
```bash
PORT=3001 npm run dev
```

#### Module Not Found
```bash
Module not found: Can't resolve 'module-name'
```
**Solution**: Install missing dependency
```bash
npm install module-name
```

#### TypeScript Errors
```bash
Type 'X' is not assignable to type 'Y'
```
**Solution**: Check type definitions and ensure correct imports

### Getting Help

- Check the [FAQ](./FAQ.md) for common questions
- Search [existing issues](https://github.com/your-org/reazon_repo/issues)
- Create a [new issue](https://github.com/your-org/reazon_repo/issues/new) with detailed information
- Join our [community discussions](https://github.com/your-org/reazon_repo/discussions)

## Resources

- [React Documentation](https://react.dev/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Testing Library](https://testing-library.com/)
- [MDN Web Docs](https://developer.mozilla.org/)

---

**Welcome to the reazon_repo community!** We're excited to see what you'll build with these tools.