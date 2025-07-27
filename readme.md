# Changi Airport Chatbot Frontend

A modern, responsive web interface for the Changi Airport intelligent chatbot system. Built with vanilla HTML, CSS, and JavaScript for optimal performance and compatibility.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Screenshots](#screenshots)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Development](#development)
- [Customization](#customization)
- [Deployment](#deployment)
- [Browser Support](#browser-support)
- [Contributing](#contributing)
- [License](#license)

## Overview

This frontend provides an intuitive, chat-based interface for travelers to interact with the Changi Airport information system. Users can ask questions about dining, shopping, transportation, services, and attractions at Singapore's Changi Airport and Jewel Changi Airport.

## Features

### User Experience
- **Intuitive Chat Interface**: Clean, modern chat UI with message bubbles
- **Real-time Conversations**: Instant responses with typing indicators
- **Smart Suggestions**: Pre-built query suggestions for common questions
- **Source Attribution**: Clear citation of information sources
- **Mobile Responsive**: Optimized for all screen sizes and devices
- **Accessibility**: Screen reader friendly with proper ARIA labels

### Visual Design
- **Modern Aesthetics**: Gradient backgrounds and smooth animations
- **Glassmorphism Effects**: Contemporary design with backdrop filters
- **Smooth Animations**: Engaging micro-interactions and transitions
- **Dark/Light Themes**: Automatic theme adaptation
- **Progressive Loading**: Elegant loading states and error handling

### Technical Features
- **Zero Dependencies**: Pure HTML, CSS, and JavaScript
- **Fast Loading**: Optimized assets and minimal payload
- **Cross-browser Compatible**: Works on all modern browsers
- **Offline Indicators**: Connection status monitoring
- **Error Recovery**: Graceful error handling with user-friendly messages

## Screenshots

### Desktop Interface
![Desktop Chat Interface](screenshots/desktop-chat.png)

### Mobile Interface
![Mobile Chat Interface](screenshots/mobile-chat.png)

### Welcome Screen
![Welcome Screen with Suggestions](screenshots/welcome-screen.png)

## Installation

### Quick Start

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/changi-chatbot-frontend.git
cd changi-chatbot-frontend
```

2. **Configure backend URL**
```javascript
// Edit the API_URL in index.html
const API_URL = 'https://your-backend-url.com';
```

3. **Serve the files**
```bash
# Using Python's built-in server
python -m http.server 8080

# Using Node.js http-server
npx http-server -p 8080

# Or simply open index.html in your browser
open index.html
```

### Development Setup

1. **Install development tools** (optional)
```bash
# For live reloading during development
npm install -g live-server

# Start development server
live-server --port=8080 --entry-file=index.html
```

2. **Enable CORS for local development**
   - Backend must allow `http://localhost:8080` origin
   - Or use a proxy/tunnel service for development

## Configuration

### Backend Connection

Update the `API_URL` constant in `index.html`:

```javascript
// Configuration - Backend URL
const API_URL = 'https://your-backend-api.com';
```

### Customization Options

The interface can be customized by modifying CSS variables:

```css
:root {
  /* Primary Colors */
  --primary-blue: #4facfe;
  --secondary-blue: #00f2fe;
  --accent-purple: #667eea;
  
  /* UI Colors */
  --background-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  --chat-container-bg: rgba(255, 255, 255, 0.95);
  --user-message-bg: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  --bot-message-bg: #f8f9fa;
  
  /* Typography */
  --font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  --font-size-base: 1rem;
  --line-height-base: 1.5;
  
  /* Spacing */
  --border-radius: 20px;
  --padding-base: 20px;
  --gap-base: 15px;
}
```

### Feature Toggles

Enable/disable features by modifying JavaScript constants:

```javascript
// Feature configuration
const FEATURES = {
  welcomeMessage: true,
  suggestionChips: true,
  typingIndicator: true,
  connectionStatus: true,
  sourceAttribution: true,
  messageTimestamps: true,
  debugMode: false
};
```

## Usage

### Basic Interaction

1. **Open the application** in your web browser
2. **View welcome message** with suggested queries
3. **Click suggestions** or type your own questions
4. **Receive responses** with source citations
5. **Continue conversations** with follow-up questions

### Example Queries

The interface provides smart suggestions for common questions:

- "Tell me about Rain Vortex show times" (Attractions)
- "Best halal dining options" (Dining)
- "Duty-free shopping deals" (Shopping)
- "How to get to the city from airport" (Transportation)
- "Family activities at Jewel" (Family Services)

### Advanced Features

**Message History**: Conversations are maintained during the browser session

**Source Verification**: Click on source links to verify information

**Mobile Gestures**: Swipe and touch-friendly interface on mobile devices

**Keyboard Shortcuts**:
- `Enter`: Send message
- `Shift + Enter`: New line in message
- `Escape`: Clear current input

## Development

### Project Structure

```
changi-chatbot-frontend/
├── index.html              # Main application file
├── README.md              # This file
├── screenshots/           # UI screenshots
│   ├── desktop-chat.png
│   ├── mobile-chat.png
│   └── welcome-screen.png
├── docs/                  # Documentation
│   ├── api-integration.md
│   ├── customization.md
│   └── deployment.md
└── assets/               # Optional assets
    ├── icons/
    └── fonts/
```

### Code Organization

The `index.html` file is organized into clear sections:

1. **HTML Structure**
   - Chat container and message areas
   - Input controls and buttons
   - Status indicators

2. **CSS Styles**
   - Responsive design with mobile-first approach
   - Modern animations and transitions
   - Accessibility considerations

3. **JavaScript Functionality**
   - API communication logic
   - UI state management
   - Event handling and user interactions

### Key Functions

#### `sendMessage()`
Main function for sending user messages to the backend.

```javascript
async function sendMessage() {
    const message = messageInput.value.trim();
    if (!message || sendButton.disabled) return;
    
    // Add user message to chat
    addMessage(message, 'user');
    
    // Send to backend and handle response
    try {
        const response = await fetch(`${API_URL}/chat`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ message, session_id: generateSessionId() })
        });
        // Handle response...
    } catch (error) {
        // Error handling...
    }
}
```

#### `addMessage()`
Dynamically adds messages to the chat interface.

```javascript
function addMessage(content, sender, metadata = {}) {
    const messageDiv = document.createElement('div');
    messageDiv.className = `message ${sender}-message`;
    
    // Build message content with sources and metadata
    let messageHTML = `<div class="message-content">${content}</div>`;
    
    if (metadata.sources && metadata.sources.length > 0) {
        messageHTML += buildSourcesSection(metadata.sources);
    }
    
    messageDiv.innerHTML = messageHTML;
    messagesContainer.appendChild(messageDiv);
    scrollToBottom();
}
```

#### `testConnection()`
Tests backend connectivity and shows status to users.

```javascript
async function testConnection() {
    try {
        const response = await fetch(`${API_URL}/health`);
        if (response.ok) {
            showConnectionStatus('Connected successfully!', 'success');
        }
    } catch (error) {
        showConnectionStatus('Backend starting up...', 'warning');
    }
}
```

### Development Guidelines

**Code Style**:
- Use ES6+ features (async/await, arrow functions, template literals)
- Follow consistent naming conventions
- Add comments for complex logic
- Keep functions focused and modular

**CSS Best Practices**:
- Use CSS custom properties for theming
- Mobile-first responsive design
- Semantic class names
- Efficient selectors

**JavaScript Patterns**:
- Event delegation for dynamic content
- Async/await for API calls
- Error boundaries for graceful failures
- Progressive enhancement

## Customization

### Theming

Create custom themes by modifying CSS variables:

```css
/* Custom Theme Example */
:root[data-theme="dark"] {
  --background-gradient: linear-gradient(135deg, #2c3e50 0%, #34495e 100%);
  --chat-container-bg: rgba(44, 62, 80, 0.95);
  --bot-message-bg: #34495e;
  --text-color: #ecf0f1;
}
```

### Branding

Customize branding elements:

```css
.chat-header h1::before {
  content: '🏢'; /* Replace with your logo */
}

.chat-header h1 {
  font-family: 'Your Custom Font', sans-serif;
}
```

### Suggestion Chips

Modify welcome suggestions:

```javascript
const suggestions = [
    { text: "Your custom suggestion", icon: "🔍" },
    { text: "Another suggestion", icon: "💡" },
    // Add more suggestions...
];
```

### Message Templates

Customize message templates:

```javascript
const messageTemplates = {
    error: "I'm experiencing technical difficulties. Please try again or contact support.",
    loading: "Let me search for that information...",
    noResults: "I couldn't find specific information about that. Would you like to try a different question?"
};
```

## Deployment

### Static Hosting

Deploy to any static hosting service:

**Netlify**:
1. Connect your GitHub repository
2. Set build command: (none needed)
3. Set publish directory: `/` (root)
4. Deploy

**Vercel**:
1. Import your GitHub repository
2. Configure build settings (default works)
3. Deploy

**GitHub Pages**:
1. Enable Pages in repository settings
2. Select source branch (main/master)
3. Access at `https://username.github.io/repository-name`

### CDN Deployment

For better performance, deploy assets to a CDN:

```html
<!-- Example CDN links -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="stylesheet" href="https://your-cdn.com/styles.css">
<script src="https://your-cdn.com/app.js"></script>
```

### Production Optimization

**Minification**:
```bash
# Minify CSS and JavaScript for production
npm install -g uglify-js clean-css-cli
uglifyjs app.js -o app.min.js
cleancss styles.css -o styles.min.css
```

**Compression**:
- Enable Gzip compression on your server
- Use WebP images for better performance
- Implement proper caching headers

### Environment Configuration

Handle different environments:

```javascript
const config = {
    development: {
        API_URL: 'http://localhost:8000',
        DEBUG: true
    },
    production: {
        API_URL: 'https://your-production-api.com',
        DEBUG: false
    }
};

const env = window.location.hostname === 'localhost' ? 'development' : 'production';
const API_URL = config[env].API_URL;
```

## Browser Support

### Supported Browsers

- **Chrome**: 60+
- **Firefox**: 55+
- **Safari**: 12+
- **Edge**: 79+
- **Mobile Safari**: iOS 12+
- **Chrome for Android**: 60+

### Polyfills

For older browser support, include polyfills:

```html
<!-- Fetch API polyfill for IE -->
<script src="https://polyfill.io/v3/polyfill.min.js?features=fetch"></script>

<!-- CSS Grid polyfill -->
<script src="https://cdn.jsdelivr.net/npm/css-grid-polyfill@1.0.0/dist/css-grid-polyfill.min.js"></script>
```

### Progressive Enhancement

The interface degrades gracefully:
- Core functionality works without JavaScript
- Basic styling applies without CSS Grid/Flexbox
- Form submission works without fetch API

## Performance

### Optimization Techniques

**Lazy Loading**:
- Images loaded on demand
- Progressive message rendering
- Background resource loading

**Caching**:
- Browser caching for static assets
- LocalStorage for user preferences
- Session storage for temporary data

**Bundle Size**:
- Zero external dependencies
- Inline critical CSS
- Optimized images and fonts

### Performance Metrics

Target performance benchmarks:
- **First Contentful Paint**: < 1.5s
- **Largest Contentful Paint**: < 2.5s
- **Cumulative Layout Shift**: < 0.1
- **First Input Delay**: < 100ms

## Accessibility

### WCAG Compliance

The interface follows WCAG 2.1 AA guidelines:

- **Keyboard Navigation**: All interactive elements accessible via keyboard
- **Screen Reader Support**: Proper ARIA labels and semantic HTML
- **Color Contrast**: 4.5:1 ratio for normal text, 3:1 for large text
- **Focus Management**: Clear focus indicators and logical tab order

### Accessibility Features

```html
<!-- Screen reader announcements -->
<div aria-live="polite" aria-atomic="true" class="sr-only" id="status"></div>

<!-- Semantic structure -->
<main role="main">
  <section aria-label="Chat conversation">
    <div role="log" aria-live="polite">
      <!-- Messages appear here -->
    </div>
  </section>
</main>

<!-- Accessible form controls -->
<label for="messageInput" class="sr-only">Type your message</label>
<textarea id="messageInput" aria-describedby="inputHelp"></textarea>
```

## Contributing

We welcome contributions to improve the frontend experience!

### Getting Started

1. **Fork the repository**
2. **Create a feature branch**: `git checkout -b feature/ui-improvement`
3. **Make your changes**
4. **Test across browsers**
5. **Submit a pull request**

### Contribution Guidelines

**UI/UX Improvements**:
- Maintain accessibility standards
- Test on multiple devices and browsers
- Follow existing design patterns
- Provide before/after screenshots

**Code Contributions**:
- Follow existing code style
- Add comments for complex logic
- Test new features thoroughly
- Update documentation as needed

**Bug Reports**:
- Include browser version and OS
- Provide steps to reproduce
- Include screenshots if applicable
- Test in multiple browsers

## Acknowledgments

- Design inspiration from modern chat interfaces
- Singapore's Changi Airport for comprehensive information access
- Open source community for web development best practices
- Contributors who have helped improve the user experience