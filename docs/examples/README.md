# Examples

Real-world usage examples and implementation patterns for reazon_repo components, APIs, and utilities.

## Overview

This section provides practical examples that demonstrate how to use the various features of reazon_repo in real applications. Each example includes complete, runnable code with explanations.

## 📁 Example Categories

### 🔌 API Integration Examples
- [User Authentication Flow](./api/authentication-flow.md)
- [Data Fetching with Error Handling](./api/data-fetching.md)
- [File Upload with Progress](./api/file-upload.md)
- [Real-time Updates with WebSockets](./api/websockets.md)
- [Pagination and Filtering](./api/pagination.md)
- [Batch Operations](./api/batch-operations.md)

### 🎨 Component Examples
- [Form Validation](./components/form-validation.md)
- [Data Table with Sorting](./components/data-table.md)
- [Modal Dialog Patterns](./components/modal-patterns.md)
- [Navigation Components](./components/navigation.md)
- [Dashboard Layout](./components/dashboard.md)
- [Responsive Design](./components/responsive-design.md)

### ⚡ Utility Function Examples
- [Data Processing Pipeline](./utils/data-processing.md)
- [Currency and Number Formatting](./utils/formatting.md)
- [Date and Time Utilities](./utils/date-time.md)
- [Validation Helpers](./utils/validation.md)
- [Performance Optimization](./utils/performance.md)
- [Error Handling Patterns](./utils/error-handling.md)

### 🏗️ Architecture Patterns
- [Clean Architecture Setup](./architecture/clean-architecture.md)
- [State Management](./architecture/state-management.md)
- [Testing Strategies](./architecture/testing.md)
- [Performance Monitoring](./architecture/monitoring.md)
- [Deployment Patterns](./architecture/deployment.md)

### 🔧 Integration Examples
- [Third-party API Integration](./integrations/third-party-apis.md)
- [Database Operations](./integrations/database.md)
- [Authentication Providers](./integrations/auth-providers.md)
- [Payment Processing](./integrations/payments.md)
- [Email and Notifications](./integrations/notifications.md)

## 🚀 Quick Start Examples

### Simple React App

```tsx
// App.tsx
import React, { useState } from 'react';
import { Button, Card, Input } from '@reazon/components';
import { validateEmail, formatCurrency } from '@reazon/utils';
import { useApi } from '@reazon/hooks';

function App() {
  const [email, setEmail] = useState('');
  const [amount, setAmount] = useState(0);
  const { data, loading, error, execute } = useApi();

  const handleSubmit = async () => {
    if (!validateEmail(email)) {
      alert('Please enter a valid email');
      return;
    }

    await execute('/api/transactions', {
      method: 'POST',
      body: { email, amount }
    });
  };

  return (
    <Card>
      <h1>Payment Form</h1>
      
      <Input
        type="email"
        placeholder="Email address"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      
      <Input
        type="number"
        placeholder="Amount"
        value={amount}
        onChange={(e) => setAmount(Number(e.target.value))}
      />
      
      <p>Total: {formatCurrency(amount, 'USD')}</p>
      
      <Button 
        onClick={handleSubmit}
        loading={loading}
        disabled={!validateEmail(email) || amount <= 0}
      >
        Process Payment
      </Button>
      
      {error && <p style={{ color: 'red' }}>{error.message}</p>}
      {data && <p style={{ color: 'green' }}>Payment successful!</p>}
    </Card>
  );
}

export default App;
```

### API Client Usage

```typescript
// services/userService.ts
import { ReazonClient } from '@reazon/api-client';
import { User, CreateUserData } from '../types';

const client = new ReazonClient({
  baseUrl: process.env.REACT_APP_API_URL,
  apiKey: process.env.REACT_APP_API_KEY
});

export class UserService {
  static async getAllUsers(page = 1, limit = 20) {
    return client.get<{ users: User[], total: number }>('/users', {
      params: { page, limit }
    });
  }

  static async getUserById(id: string) {
    return client.get<User>(`/users/${id}`);
  }

  static async createUser(userData: CreateUserData) {
    return client.post<User>('/users', userData);
  }

  static async updateUser(id: string, updates: Partial<User>) {
    return client.patch<User>(`/users/${id}`, updates);
  }

  static async deleteUser(id: string) {
    return client.delete(`/users/${id}`);
  }

  static async uploadAvatar(id: string, file: File) {
    const formData = new FormData();
    formData.append('avatar', file);
    
    return client.post<{ url: string }>(`/users/${id}/avatar`, formData, {
      headers: { 'Content-Type': 'multipart/form-data' }
    });
  }
}
```

### Custom Hook Example

```typescript
// hooks/useUserManagement.ts
import { useState, useEffect } from 'react';
import { UserService } from '../services/userService';
import { User, CreateUserData } from '../types';

export function useUserManagement() {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<Error | null>(null);

  const loadUsers = async (page = 1, limit = 20) => {
    try {
      setLoading(true);
      setError(null);
      const response = await UserService.getAllUsers(page, limit);
      setUsers(response.users);
    } catch (err) {
      setError(err instanceof Error ? err : new Error('Unknown error'));
    } finally {
      setLoading(false);
    }
  };

  const createUser = async (userData: CreateUserData) => {
    try {
      setLoading(true);
      const newUser = await UserService.createUser(userData);
      setUsers(prev => [...prev, newUser]);
      return newUser;
    } catch (err) {
      setError(err instanceof Error ? err : new Error('Unknown error'));
      throw err;
    } finally {
      setLoading(false);
    }
  };

  const updateUser = async (id: string, updates: Partial<User>) => {
    try {
      setLoading(true);
      const updatedUser = await UserService.updateUser(id, updates);
      setUsers(prev => prev.map(user => 
        user.id === id ? updatedUser : user
      ));
      return updatedUser;
    } catch (err) {
      setError(err instanceof Error ? err : new Error('Unknown error'));
      throw err;
    } finally {
      setLoading(false);
    }
  };

  const deleteUser = async (id: string) => {
    try {
      setLoading(true);
      await UserService.deleteUser(id);
      setUsers(prev => prev.filter(user => user.id !== id));
    } catch (err) {
      setError(err instanceof Error ? err : new Error('Unknown error'));
      throw err;
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    loadUsers();
  }, []);

  return {
    users,
    loading,
    error,
    loadUsers,
    createUser,
    updateUser,
    deleteUser
  };
}
```

## 📚 Featured Examples

### 1. E-commerce Product Catalog

A complete product listing with search, filtering, and cart functionality.

**Features:**
- Product grid with pagination
- Search and filter capabilities
- Shopping cart management
- Responsive design

**Technologies:**
- React components
- Custom hooks
- API integration
- Local storage

[View Complete Example →](./featured/ecommerce-catalog.md)

### 2. User Dashboard

A comprehensive user dashboard with authentication, profile management, and data visualization.

**Features:**
- User authentication flow
- Profile editing
- Data charts and analytics
- Settings management

**Technologies:**
- Authentication API
- Form components
- Chart components
- Real-time updates

[View Complete Example →](./featured/user-dashboard.md)

### 3. File Management System

A file upload and management interface with drag-and-drop support.

**Features:**
- Drag-and-drop file upload
- Progress tracking
- File preview
- Folder organization

**Technologies:**
- File upload API
- Progress components
- Modal dialogs
- Error handling

[View Complete Example →](./featured/file-management.md)

### 4. Real-time Chat Application

A chat interface with real-time messaging and user presence.

**Features:**
- Real-time messaging
- User presence indicators
- Message history
- Typing indicators

**Technologies:**
- WebSocket API
- Chat components
- State management
- Performance optimization

[View Complete Example →](./featured/chat-application.md)

## 🛠️ Setup and Installation

### Prerequisites

Before running any examples, ensure you have:

1. **Node.js 18.0+** installed
2. **reazon_repo** package installed
3. **API keys** configured (if required)

### Installation

```bash
# Clone the repository
git clone https://github.com/your-org/reazon_repo.git
cd reazon_repo

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env
# Edit .env with your configuration

# Run example applications
npm run examples
```

### Environment Configuration

Create a `.env` file with the following variables:

```env
# API Configuration
REACT_APP_API_URL=http://localhost:3001/api/v1
REACT_APP_API_KEY=your-api-key

# Authentication
REACT_APP_AUTH_DOMAIN=your-auth-domain
REACT_APP_AUTH_CLIENT_ID=your-client-id

# Feature Flags
REACT_APP_ENABLE_ANALYTICS=true
REACT_APP_ENABLE_CHAT=true
```

## 🎯 Usage Patterns

### Error Boundary Pattern

```tsx
// components/ErrorBoundary.tsx
import React, { Component, ReactNode } from 'react';
import { Alert } from '@reazon/components';

interface Props {
  children: ReactNode;
  fallback?: ReactNode;
}

interface State {
  hasError: boolean;
  error?: Error;
}

export class ErrorBoundary extends Component<Props, State> {
  constructor(props: Props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    console.error('Error caught by boundary:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback || (
        <Alert variant="error">
          <h2>Something went wrong</h2>
          <p>{this.state.error?.message}</p>
          <button onClick={() => this.setState({ hasError: false })}>
            Try again
          </button>
        </Alert>
      );
    }

    return this.props.children;
  }
}
```

### Loading State Pattern

```tsx
// components/LoadingWrapper.tsx
import React from 'react';
import { Spinner, Skeleton } from '@reazon/components';

interface LoadingWrapperProps {
  loading: boolean;
  error?: Error | null;
  children: React.ReactNode;
  loadingComponent?: React.ReactNode;
  errorComponent?: React.ReactNode;
  skeletonLines?: number;
}

export const LoadingWrapper: React.FC<LoadingWrapperProps> = ({
  loading,
  error,
  children,
  loadingComponent,
  errorComponent,
  skeletonLines = 3
}) => {
  if (error) {
    return errorComponent || (
      <div className="error-state">
        <p>Error: {error.message}</p>
      </div>
    );
  }

  if (loading) {
    return loadingComponent || (
      <div className="loading-state">
        <Skeleton lines={skeletonLines} />
      </div>
    );
  }

  return <>{children}</>;
};
```

### Data Fetching Pattern

```typescript
// hooks/useAsyncData.ts
import { useState, useEffect, useCallback } from 'react';

interface AsyncState<T> {
  data: T | null;
  loading: boolean;
  error: Error | null;
}

export function useAsyncData<T>(
  asyncFunction: () => Promise<T>,
  dependencies: React.DependencyList = []
) {
  const [state, setState] = useState<AsyncState<T>>({
    data: null,
    loading: true,
    error: null
  });

  const execute = useCallback(async () => {
    setState(prev => ({ ...prev, loading: true, error: null }));
    
    try {
      const result = await asyncFunction();
      setState({ data: result, loading: false, error: null });
    } catch (error) {
      setState({
        data: null,
        loading: false,
        error: error instanceof Error ? error : new Error('Unknown error')
      });
    }
  }, dependencies);

  useEffect(() => {
    execute();
  }, [execute]);

  const refetch = useCallback(() => {
    execute();
  }, [execute]);

  return { ...state, refetch };
}
```

## 🔗 External Resources

### Documentation Links
- [React Documentation](https://react.dev/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Testing Library](https://testing-library.com/)

### Community Examples
- [CodeSandbox Examples](https://codesandbox.io/s/reazon-examples)
- [GitHub Discussions](https://github.com/your-org/reazon_repo/discussions)
- [Stack Overflow Tags](https://stackoverflow.com/questions/tagged/reazon-repo)

### Video Tutorials
- [Getting Started with reazon_repo](https://youtube.com/watch?v=example)
- [Building a Complete App](https://youtube.com/watch?v=example)
- [Advanced Patterns](https://youtube.com/watch?v=example)

## 🤝 Contributing Examples

Help us improve the examples! Here's how to contribute:

1. **Identify a gap**: Look for missing use cases or patterns
2. **Create the example**: Write clear, complete code examples
3. **Document thoroughly**: Include explanations and comments
4. **Test your code**: Ensure examples work as expected
5. **Submit PR**: Follow our contribution guidelines

### Example Contribution Template

```markdown
# Example Title

Brief description of what this example demonstrates.

## What You'll Learn

- Feature 1
- Feature 2
- Feature 3

## Prerequisites

- Requirement 1
- Requirement 2

## Code

\`\`\`typescript
// Complete example code here
\`\`\`

## Explanation

Step-by-step explanation of the code.

## Next Steps

Suggestions for extending the example.
```

---

**Ready to explore?** Start with the [Getting Started Guide](../guides/getting-started.md) or dive into a specific example that matches your needs!