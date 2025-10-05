# Aurelius Federal Platform - Resource Library

## Overview

This repository (`next-build`) is part of the Aurelius Federal platform, serving as the front-end templating, build, and deployment system for VA.gov CMS content. This document provides a comprehensive resource guide for understanding and utilizing this repository within the Aurelius Federal platform ecosystem.

## Repository Information

- **Repository Name**: next-build
- **Organization**: AureliustechandTalentSolutions
- **Primary Purpose**: Front-end templating, build, and deploy for VA.gov CMS content
- **Technology Stack**: Next.js, TypeScript, Node.js
- **Platform**: Aurelius Federal Platform

## Key Features

### 1. Federal Healthcare System Integration

This repository includes specialized support for the Lovell Federal health care system, which represents a unique case in federal healthcare information architecture:

- **Dual Entity Management**: Handles both VA and DOD (TRICARE) entities
- **Content Bifurcation**: Automatically generates separate pages for VA and TRICARE from single content sources
- **Federated Resource Management**: Merges federal-level content with organization-specific content

For detailed information, see [Lovell Federal Healthcare Documentation](./lovell.md).

### 2. Multi-Environment Support

The platform supports multiple deployment environments:

- **Production** (`vagovprod`): Production VA.gov assets
- **Staging** (`vagovstaging`): Staging environment for testing
- **Development** (`vagovdev`): Development environment
- **Local Development** (`localhost`): Local development with adjacent vets-website repository
- **Tugboat**: Pull request preview environments

### 3. Content Management System Integration

- **Drupal CMS Integration**: Connects to VA.gov CMS via JSON:API
- **OAuth Authentication**: Secure API access using public/private key pairs
- **Preview Mode**: Real-time content preview capabilities
- **Feature Flags**: Controlled rollout of new content types

## Resource Directory

### Getting Started Resources

1. **[README.md](../README.md)** - Main repository documentation with setup instructions
2. **[Quickstart Guide](./quickstart.md)** - Step-by-step guide for development
3. **[Contributing Guidelines](../CONTRIBUTING.md)** - How to contribute to the project
4. **[Code Guidelines](./code-guidelines.md)** - Coding standards and best practices

### Development Resources

1. **[TypeScript Guide](./typescript.md)** - TypeScript usage and conventions
2. **[Testing Documentation](./testing.md)** - Testing strategies and tools
3. **[Queries Documentation](./queries.md)** - GraphQL/API query patterns
4. **[Templates Guide](./templates.md)** - Template structure and creation
5. **[Generators](./generators.md)** - Code generation tools
6. **[Business Logic](./business-logic.md)** - Core business logic documentation

### Architecture & Infrastructure

1. **[Environment Loader](./env-loader.md)** - Environment configuration management
2. **[Caching Strategy](./caching.md)** - Redis caching implementation
3. **[Infrastructure Overview](./devops/infrastructure.md)** - AWS/EKS infrastructure
4. **[DevOps Resources](./devops/)** - Deployment and operations documentation
5. **[Docker Configuration](../docker/Dockerfile)** - Container setup

### Content & Integration

1. **[Paragraph Types](./paragraph.md)** - Content component documentation
2. **[Layout Rollout](./layout-rollout.md)** - Feature flag and content type deployment
3. **[Pagination](./pagination.md)** - List and pagination handling
4. **[Analytics](./analytics.md)** - Analytics integration
5. **[Next.js Slug Handling](./%5B%5B...slug%5D%5D.md)** - Dynamic routing

### Platform-Specific Resources

1. **[Lovell Federal Health Care](./lovell.md)** - Federal healthcare system specifics
2. **[Preview Mode](./preview.md)** - Content preview functionality
3. **[Tugboat Integration](./tugboat.md)** - PR preview environments
4. **[Broken Links](./broken-links.md)** - Link validation

### External Integration

1. **[Working with Other Repos](./next-build-and-other-repos.md)** - Integration with vets-website and CMS
2. **[Content Release Process](./devops/content-release.md)** - Deployment workflow
3. **[DataDog Integration](./devops/datadog.md)** - Monitoring and observability
4. **[Data Synchronization](./devops/datasync.md)** - Data sync processes

## Quick Setup for Aurelius Federal Platform

### Prerequisites

- VA SOCKS access for secure connectivity
- Node.js (via NVM) - Version 22
- Yarn package manager
- Docker for Redis container
- Git access to repository

### Basic Installation

```bash
# Clone the repository
git clone git@github.com:AureliustechandTalentSolutions/next-build.git

# Clone adjacent vets-website repository (for local assets)
git clone git@github.com:department-of-veterans-affairs/vets-website.git

# Navigate to next-build
cd next-build

# Use correct Node version
nvm use

# Install dependencies
yarn install

# Copy environment configuration
cp envs/.env.example envs/.env.local

# Setup initial assets
yarn setup

# Start development server
yarn dev
```

The application will be available at http://localhost:3999

## Federal Platform Considerations

### Security

- **SOCKS Proxy Required**: All production/staging connections require VA SOCKS access
- **OAuth Keys**: Stored in AWS SSM for secure API authentication
- **Certificate Management**: Custom SSL certificates for local CMS development
- **Secret Management**: Environment-specific secrets in `.env` files (gitignored)

### Compliance

- **Accessibility**: Built-in accessibility testing with Playwright and axe-core
- **Section 508**: Federal accessibility standards compliance
- **Performance**: Lighthouse testing for performance metrics
- **Code Quality**: ESLint, Prettier, and TypeScript for code standards

### Federal Healthcare Integration

This repository includes specialized handling for federal healthcare systems:

- **Lovell Federal System**: Unique bifurcation logic for VA/TRICARE content
- **VAMC Systems**: Veterans Affairs Medical Center integrations
- **Multi-Administration Support**: Content across different organizational structures

## Build and Deployment

### Development Builds

```bash
# Start local development server
yarn dev

# Build for preview (without static generation)
yarn build:preview

# Start production server locally
yarn start
```

### Static Site Generation

```bash
# Generate full static site with Redis cache
yarn redis          # Start Redis container
yarn export         # Generate static pages

# Serve static build locally
yarn export:serve   # Available at http://localhost:8001
```

### Deployment Pipeline

1. **Code Changes**: Developer commits to feature branch
2. **Tugboat Preview**: Automatic preview environment created
3. **CI/CD**: GitHub Actions run tests and builds
4. **Code Review**: PR review and approval
5. **Merge**: Merge to main branch
6. **Deployment**: Automatic deployment to target environment
7. **Asset Sync**: Static assets synced to S3 buckets

## Testing Strategy

### Unit Tests

```bash
# Run all tests
yarn test

# Run specific test
yarn test -- path/to/file

# CI test mode
yarn test:ci
```

### End-to-End Tests

```bash
# Playwright tests
yarn test:playwright

# Accessibility tests
yarn test:playwright:a11y
```

### Performance Testing

```bash
# Lighthouse performance testing
yarn lighthouse

# Load testing
cd load-testing && ./run-load-test.sh
```

## Asset Management

### Build Types

The platform uses different asset sources based on build type:

- **`vagovprod`**: Production S3 bucket (default)
- **`vagovstaging`**: Staging S3 bucket
- **`vagovdev`**: Development S3 bucket
- **`localhost`**: Symlink to local vets-website build
- **`tugboat`**: Development S3 bucket (for PR previews)

### Asset Script

```bash
# Default: Download from production bucket
yarn setup

# Use local vets-website assets
BUILD_TYPE=localhost yarn setup

# Use staging assets
BUILD_TYPE=vagovstaging yarn setup
```

## Environment Configuration

### Key Environment Variables

- **`NEXT_PUBLIC_DRUPAL_BASE_URL`**: CMS endpoint URL
- **`NEXT_PUBLIC_BUILD_TYPE`**: Build environment type
- **`DRUPAL_CLIENT_ID`**: OAuth client ID
- **`DRUPAL_CLIENT_SECRET`**: OAuth client secret
- **`USE_REDIS`**: Enable Redis caching
- **`REDIS_URL`**: Redis connection URL
- **`SITE_URL`**: Base URL for absolute paths
- **`FEATURE_NEXT_BUILD_CONTENT_*`**: Feature flags for content types

### Environment Files

- `.env.local` - Local development (gitignored)
- `.env.test` - Test environment
- `.env.tugboat` - Tugboat preview environments
- `.env.example` - Template with all available options

## Monitoring and Observability

### DataDog Integration

The platform includes comprehensive monitoring through DataDog:

- Application performance monitoring (APM)
- Error tracking and alerting
- Custom metrics and dashboards
- Log aggregation

See [DataDog Documentation](./devops/datadog.md) for details.

### Health Checks

- Build status via GitHub Actions badges
- CodeQL security scanning
- Playwright test results
- Accessibility audit results

## Common Tasks

### Adding a New Content Type

1. Create TypeScript interfaces in `src/types/drupal/`
2. Add resource type to `src/lib/constants/resourceTypes.ts`
3. Create query files in `src/data/queries/`
4. Implement formatter in `src/data/queries/`
5. Add feature flag variable
6. Create page templates
7. Add tests

See [Quickstart Guide](./quickstart.md) for detailed steps.

### Debugging

```bash
# Enable debug logging
DEBUG=* yarn dev

# Specific debug namespace
DEBUG=vets-website-assets yarn setup

# Interactive debugging
node --inspect ./scripts/yarn/dev.js
```

### Working with Redis

```bash
# Start Redis container
yarn redis

# Access Redis CLI
yarn redis:cli

# Stop and remove Redis container
yarn redis:stop
```

## Support and Resources

### Documentation Index

- All documentation is located in the `READMEs/` directory
- Template migration guides in `docs/template-migration/`
- DevOps documentation in `READMEs/devops/`
- Type definitions in `src/types/drupal/`

### Key Directories

- **`src/`**: Application source code
- **`public/`**: Static assets
- **`scripts/`**: Build and utility scripts
- **`packages/`**: Monorepo workspace packages
- **`READMEs/`**: Documentation
- **`docker/`**: Docker configuration
- **`playwright/`**: E2E tests

### Code Organization

- **Data Layer**: `src/data/queries/` - CMS data fetching
- **Formatters**: `src/data/queries/` - Data transformation
- **Components**: `src/components/` - React components
- **Templates**: `src/templates/` - Page templates
- **Library Code**: `src/lib/` - Shared utilities
- **Types**: `src/types/` - TypeScript definitions

## Version Control

### Branch Strategy

- **`main`**: Production-ready code
- **Feature branches**: `feature/description`
- **Bug fixes**: `fix/description`
- **Copilot branches**: `copilot/*` (automated)

### Commit Guidelines

- Follow conventional commits format
- Keep commits atomic and focused
- Reference issues/tickets in commit messages
- Use meaningful commit messages

## Additional Resources

### External Documentation

- [Next.js Documentation](https://nextjs.org/docs)
- [Drupal JSON:API](https://www.drupal.org/docs/core-modules-and-themes/core-modules/jsonapi-module)
- [VA Platform Documentation](https://depo-platform-documentation.scrollhelp.site/)

### Platform Status

See [Next Build Status](./next-build-status.md) for current content type coverage and migration status.

## Contact and Support

For questions, issues, or contributions related to the Aurelius Federal platform:

1. Review existing documentation in `READMEs/`
2. Check [GitHub Issues](https://github.com/AureliustechandTalentSolutions/next-build/issues)
3. Follow [Contributing Guidelines](../CONTRIBUTING.md)
4. Reference [Code of Conduct](../CODE_OF_CONDUCT.md)

---

**Last Updated**: 2024
**Platform**: Aurelius Federal Platform
**Repository**: next-build
**License**: See [LICENSE](../LICENSE)
