# 🤝 Contributing to Boutique-To-Box

Thank you for your interest in contributing to Boutique-To-Box! This document provides guidelines and information for contributors.

## 📋 Table of Contents
- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [Contributing Process](#contributing-process)
- [Coding Standards](#coding-standards)
- [Testing Guidelines](#testing-guidelines)
- [Pull Request Process](#pull-request-process)
- [Issue Guidelines](#issue-guidelines)
- [Community](#community)

## 📜 Code of Conduct

This project adheres to the [Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code.

## 🚀 Getting Started

### Prerequisites
- Node.js (v18 or higher)
- npm/yarn/pnpm
- Git
- Basic knowledge of React, Node.js, and TypeScript

### First-time Contributors
1. Look for issues labeled `good first issue` or `help wanted`
2. Read through the codebase to understand the project structure
3. Join our [Discord community](#community) for questions

## 🛠️ Development Setup

### 1. Fork and Clone
```bash
# Fork the repository on GitHub, then clone your fork
git clone https://github.com/YOUR_USERNAME/Boutique-To-Box.git
cd Boutique-To-Box
```

### 2. Install Dependencies
```bash
# Install frontend dependencies
npm install

# Install backend dependencies
cd backend
npm install
cd ..
```

### 3. Environment Setup
```bash
# Copy environment files
cp .env.example .env
cp backend/.env.example backend/.env

# Fill in your environment variables
# See README.md for required API keys
```

### 4. Start Development Servers
```bash
# Terminal 1: Start frontend
npm run dev

# Terminal 2: Start backend
cd backend
npm run dev
```

## 🔄 Contributing Process

### 1. Create a Branch
```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/issue-number-description
```

### 2. Make Changes
- Write clean, readable code
- Follow our [coding standards](#coding-standards)
- Add tests for new features
- Update documentation as needed

### 3. Commit Changes
```bash
# Use conventional commits
git commit -m "feat: add new design generation feature"
git commit -m "fix: resolve authentication issue"
git commit -m "docs: update API documentation"
```

### 4. Push and Create PR
```bash
git push origin your-branch-name
# Create a Pull Request on GitHub
```

## 📝 Coding Standards

### TypeScript/JavaScript
- Use TypeScript for all new code
- Follow ESLint configuration
- Use meaningful variable and function names
- Add JSDoc comments for complex functions

### React Components
```tsx
// ✅ Good
interface ButtonProps {
  variant: 'primary' | 'secondary';
  onClick: () => void;
  children: React.ReactNode;
}

export const Button: React.FC<ButtonProps> = ({ variant, onClick, children }) => {
  return (
    <button 
      className={`btn btn-${variant}`}
      onClick={onClick}
    >
      {children}
    </button>
  );
};
```

### CSS/Styling
- Use Tailwind CSS classes
- Follow mobile-first responsive design
- Use semantic class names
- Maintain consistent spacing and typography

### Backend Code
- Use async/await instead of callbacks
- Implement proper error handling
- Add input validation
- Use meaningful HTTP status codes

## 🧪 Testing Guidelines

### Frontend Testing
```bash
# Run tests
npm test

# Run tests with coverage
npm run test:coverage
```

### Backend Testing
```bash
cd backend
npm test
```

### Test Requirements
- Write unit tests for new functions
- Add integration tests for API endpoints
- Ensure 80%+ code coverage
- Test edge cases and error scenarios

## 📥 Pull Request Process

### Before Submitting
- [ ] Code follows project standards
- [ ] Tests pass locally
- [ ] Documentation updated
- [ ] No console.log statements
- [ ] Branch is up to date with main

### PR Template
```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests pass
- [ ] Manual testing completed

## Screenshots (if applicable)
Add screenshots for UI changes

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] Tests added/updated
```

### Review Process
1. Automated checks must pass
2. At least one maintainer review required
3. Address feedback promptly
4. Squash commits before merge

## 🐛 Issue Guidelines

### Bug Reports
Use the bug report template and include:
- Steps to reproduce
- Expected vs actual behavior
- Environment details
- Screenshots/logs if applicable

### Feature Requests
Use the feature request template and include:
- Problem description
- Proposed solution
- Alternative solutions considered
- Additional context

### Issue Labels
- `bug`: Something isn't working
- `enhancement`: New feature or request
- `good first issue`: Good for newcomers
- `help wanted`: Extra attention needed
- `documentation`: Documentation improvements
- `question`: Further information requested

## 🏗️ Project Structure

```
Boutique-To-Box/
├── src/                    # Frontend source code
│   ├── components/         # Reusable React components
│   ├── pages/             # Page components
│   ├── hooks/             # Custom React hooks
│   ├── services/          # API services
│   ├── utils/             # Utility functions
│   └── types/             # TypeScript type definitions
├── backend/               # Backend source code
│   ├── routes/            # API routes
│   ├── middleware/        # Express middleware
│   ├── models/            # Data models
│   └── utils/             # Backend utilities
├── docs/                  # Documentation
├── tests/                 # Test files
└── .github/               # GitHub workflows and templates
```

## 🌟 Recognition

Contributors will be recognized in:
- README.md contributors section
- Release notes for significant contributions
- Annual contributor appreciation posts

## 💬 Community

- **Discord**: [Join our community](https://discord.gg/boutique-to-box)
- **GitHub Discussions**: For questions and ideas
- **Twitter**: [@BoutiqueToBox](https://twitter.com/boutiquetoobox)

## 📚 Resources

- [React Documentation](https://reactjs.org/docs)
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)
- [TypeScript Handbook](https://www.typescriptlang.org/docs)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)

## ❓ Questions?

Don't hesitate to ask questions! You can:
- Open a GitHub Discussion
- Join our Discord community
- Comment on relevant issues
- Email the maintainers

Thank you for contributing to Boutique-To-Box! 🎉