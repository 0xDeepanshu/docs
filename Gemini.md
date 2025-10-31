# Gx402 SDK Documentation Project - Context File

## Project Overview

This is a comprehensive documentation website for the Gx402 SDK - a cross-platform SDK that provides all the power of x402 functionality specifically designed for games. The documentation is built using Mintlify, a modern platform for creating beautiful developer documentation using MDX files.

The project structure follows Mintlify's standard organization and now includes:
- Platform-specific guides for Unity, Unreal Engine, WebGL, Mobile, Telegram, and Farcaster
- Complete API reference documentation
- Tutorials for different skill levels
- Feature-specific documentation
- Community resources and contribution guidelines
- AI tools integration guides

## Project Type
Documentation website for the Gx402 SDK using Mintlify framework with MDX content format.

## Key Technologies & Tools
- **Mintlify CLI**: Used for local development and deployment
- **MDX**: Markdown with React components for rich content
- **Gx402 SDK**: Cross-platform gaming SDK with x402 functionality
- **React**: Component-based UI elements in MDX files

## Complete Directory Structure
```
docs/
├── ai-tools/               # AI tools integration guides
│   ├── claude-code.mdx     # Claude Code integration
│   ├── cursor.mdx          # Cursor AI editor integration  
│   └── windsurf.mdx        # Windsurf Studio integration
├── api-reference/          # Complete API documentation
│   ├── endpoint/           # Individual endpoint docs
│   │   ├── create.mdx      # POST endpoints
│   │   ├── delete.mdx      # DELETE endpoints
│   │   ├── get.mdx         # GET endpoints
│   │   └── webhook.mdx     # Webhook events
│   ├── authentication.mdx  # Authentication API
│   ├── core-functions.mdx  # Core SDK functions
│   └── introduction.mdx    # API overview
├── community/              # Community resources
│   ├── contributing.mdx    # Contribution guidelines
│   ├── showcase.mdx        # Project showcase
│   └── support.mdx         # Support resources
├── essentials/             # Core documentation features
│   ├── code.mdx            # Code examples guide
│   ├── images.mdx          # Images and media guide
│   ├── markdown.mdx        # Markdown/MDX guide
│   ├── navigation.mdx      # Navigation structure
│   ├── reusable-snippets.mdx # Reusable content snippets
│   └── settings.mdx        # Global settings
├── features/               # Feature documentation
│   ├── data-management.mdx # Data management features
│   ├── real-time-sync.mdx  # Real-time synchronization
│   ├── security.mdx        # Security features
│   └── x402-integration.mdx # Core x402 functionality
├── images/                 # Image assets and media
├── logo/                   # Light/dark mode logos
├── platforms/              # Platform-specific guides
│   ├── mobile.mdx          # Mobile (iOS/Android) guide
│   ├── telegram.mdx        # Telegram integration
│   ├── unity.mdx           # Unity engine guide
│   ├── unreal-engine.mdx   # Unreal Engine guide
│   └── webgl.mdx           # WebGL guide
├── snippets/               # Reusable content snippets
├── tutorials/              # Step-by-step tutorials
│   ├── basic-integration.mdx # Basic integration guide
│   └── hello-world.mdx     # Getting started tutorial
├── api-reference.mdx       # API reference introduction
├── community.mdx           # Community overview
├── development.mdx         # Development guidelines
├── docs.json              # Main configuration file
├── favicon.svg            # Favicon
├── faq.mdx                # Frequently asked questions
├── getting-started.mdx    # Getting started guide
├── index.mdx              # Homepage content
├── quickstart.mdx         # Quick start guide
├── README.md              # Project overview and quickstart
├── what-is-Gx402.mdx      # What is Gx402 guide
└── QWEN.md                # This context file
```

## Setup and Running

### Prerequisites
- Node.js version 19 or higher
- Mintlify CLI installed globally

### Installation
1. Install the Mintlify CLI globally:
   ```bash
   npm i -g mint
   ```

2. Navigate to the documentation directory (where `docs.json` is located)

3. Start the local development server:
   ```bash
   mint dev
   ```

4. View your documentation at `http://localhost:3000`

### Customization
- Site configuration is in `docs.json`
- Navigation and content organization defined there
- Colors and theme settings configured in `docs.json`
- Custom components and styling through MDX files

### Deployment
- Deployments happen automatically when pushing to the default branch (if GitHub app is installed)
- Install the Mintlify GitHub app from the dashboard for automatic deployments

## Content Creation

### Writing Content
- Use MDX format (.mdx files) which supports both Markdown and React components
- Frontmatter includes `title` and `description` fields
- Component library includes Cards, Steps, Accordion, Tabs, and more

### Navigation Structure
- Organized in multiple tabs (Getting Started, SDK Reference, Tutorials, Community)
- Each tab has groups with related content
- Supports multiple navigation tabs with grouped content
- Includes global navigation elements (GitHub, Discord)

### API Documentation
- Complete API reference with all endpoints (GET, POST, DELETE, Webhooks)
- Authentication, core functions, and platform-specific API references
- Real-world examples and error handling documentation

## Gx402 SDK Specific Features

### Multi-Platform Support
- Unity integration guide with C# examples
- Unreal Engine integration with Blueprint and C++ examples
- WebGL integration with JavaScript/TypeScript examples
- Mobile (iOS/Android) integration with native code examples
- Telegram mini-app integration
- Farcaster integration for decentralized gaming

### Core SDK Features
- x402 integration and processing
- Real-time synchronization across platforms
- Data management and storage
- Security and encryption
- Performance optimization

### Documentation Sections
- Getting Started guides for each platform
- API Reference with complete method documentation
- Tutorials from beginner to advanced levels
- Community resources for support and contribution
- AI tools integration guides (Cursor, Claude, Windsurf)

## Development Conventions

### File Naming
- Use kebab-case for file names (e.g., `getting-started.mdx`)
- Group related content in appropriate folders
- Navigation paths reference file paths without extensions

### Content Organization
- Platform-specific documentation in dedicated directories
- Feature-specific documentation organized by functionality
- API endpoints documented individually
- Tutorials organized by skill level

### Component Usage
- Leverage built-in Mintlify components for consistent UI
- Use Cards for linking to related content
- Apply proper accessibility attributes to images and interactive elements

## Key Configuration (`docs.json`)

The `docs.json` file contains:
- Site name and branding configuration (now set to "Gx402 SDK")
- Navigation structure with tabs and groups for all platforms
- Color theme settings
- Logo and favicon paths
- API documentation setup
- Footer and navbar configurations
- Social media links and support channels

## Useful Commands
- `mint dev` - Start local development server
- `mint dev --port <number>` - Start on custom port
- `mint broken-links` - Validate internal links
- `mint update` - Update CLI to latest version

## Special Features of the Gx402 Documentation
- Multi-platform coverage (Unity, Unreal, WebGL, Mobile, Telegram, Farcaster)
- Complete API reference with endpoint examples
- Interactive tutorials for different skill levels
- AI tools integration guidance
- Community resources and contribution guides
- Security and performance best practices
- Cross-platform synchronization examples

## Documentation Standards Implemented
- Consistent formatting across all platform guides
- Complete code examples for all major languages (JavaScript, C#, C++, Swift, Kotlin)
- Error handling and security best practices
- Performance optimization guides
- Accessibility and responsive design
- SEO-friendly structure with proper meta information