# Helios-Starling Protocol

<p align="center">
  <em>A streamlined WebSocket protocol focused on bidirectional request/response patterns</em>
</p>

<p align="center">
  <a href="#key-features">Key Features</a> •
  <a href="#quick-start">Quick Start</a> •
  <a href="#documentation">Documentation</a> •
  <a href="#implementations">Implementations</a> •
  <a href="#roadmap">Roadmap</a>
</p>

## Overview

Helios-Starling is a protocol specification that brings structure and reliability to WebSocket communications. It focuses on bidirectional request/response patterns while keeping implementation simple and developer experience smooth.

```javascript
// Server-side (Bun)
helios.method("users:getProfile", async (context) => {
  const profile = await db.getProfile(context.payload.userId);
  context.success({ profile });
});

// Client-side
const profile = await client.request("users:getProfile", { 
  userId: "123" 
});
```

## Key Features

- **Bidirectional Request/Response**: Both client and server can initiate requests
- **Built-in State Recovery**: Seamless reconnection with state preservation
- **Topic-Based Notifications**: Flexible pub/sub system built on top of requests
- **Developer-First**: Focus on debugging, testing, and maintaining

## Current Status

Helios-Starling is currently in active development. While the protocol specification aims to be language-agnostic, current implementations are JavaScript-focused:

- **Server**: Bun-optimized implementation
- **Client**: Browser and Node.js compatible
- **Protocol**: Core specification and documentation

> 💡 While currently JS-centric, the protocol is designed to be implemented in any language. Contributions for other language implementations are welcome!

## Quick Start

```bash
# Install server library
bun add @helios/server

# Install client library
npm install @helios/client
```

### Minimal Server Example

```javascript
import { Helios } from '@helios/server';

const helios = new Helios();

helios.method("echo", async (context) => {
  context.success(context.payload);
});

await helios.listen(8080);
```

### Minimal Client Example

```javascript
import { Starling } from '@helios/client';

const client = new Starling('ws://localhost:8080');
await client.connect();

// Make requests
const response = await client.request('echo', { message: 'Hello!' });

// Listen for notifications
client.on('user:presence', (data) => {
  console.log('User presence updated:', data);
});
```

## Documentation

- [Protocol Specification](./specification/)
- [Implementation Guide](./docs/guides/implementation.md)
- [API Reference](./docs/api/)
- [Examples](./docs/examples/)

## Implementations

### Official
- [`@helios/server`](https://github.com/helios/server) - Bun server implementation
- [`@helios/client`](https://github.com/helios/client) - JavaScript client
- [`@helios/protocol`](https://github.com/helios/protocol) - Shared protocol utilities

### Community
*Coming soon - We welcome community implementations!*

## Roadmap

See our detailed [Evolution Roadmap](./docs/design/roadmap.md) for our vision and planned features. Key highlights:

- Advanced streaming capabilities
- Enhanced state management
- Comprehensive developer tools
- Cross-language implementations

## Contributing

We're excited to welcome contributions! See our [Contributing Guide](CONTRIBUTING.md) for details.

Areas we particularly welcome help with:
- Protocol specification improvements
- Additional language implementations
- Documentation and examples
- Developer tools

## License

MIT © [Your Name]

---

<p align="center">
  <sub>Built with ❤️ for real-time communication</sub>
</p>