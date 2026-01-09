# Network Requests Analysis - README

## Overview

This directory contains the analysis and documentation for network requests made by the Scratch/TurboWarp project file `meine erste cloud variable.html`.

## Documentation Files

### 📘 [NETWORK_REQUESTS_DOCUMENTATION.md](./NETWORK_REQUESTS_DOCUMENTATION.md)
**Complete technical documentation** covering:
- Detailed analysis of all outgoing requests
- WebSocket protocol specifications
- Message formats and examples
- Connection behavior and error handling
- Security considerations
- Debugging guides
- Browser console testing examples

### 📋 [NETWORK_REQUESTS_QUICK_REFERENCE.md](./NETWORK_REQUESTS_QUICK_REFERENCE.md)
**Quick reference guide** with:
- Summary of request types
- Connection details at a glance
- Quick test commands
- Message format examples
- What's NOT in the file

## Key Findings

### Outgoing Requests Found

The HTML file makes **exactly ONE type** of network request:

| Type | Protocol | Purpose |
|------|----------|---------|
| **Cloud Variables** | WebSocket (WSS) | Synchronize shared variables across users |

**Endpoints:**
- Primary: `wss://clouddata.turbowarp.org`
- Fallback: `wss://clouddata.turbowarp.xyz`

**Project ID:** `1251551334`

### What's NOT Requested

The file does **NOT** make these requests:
- ❌ HTTP/HTTPS GET or POST requests
- ❌ Fetch API calls
- ❌ XMLHttpRequest
- ❌ External images, scripts, or stylesheets
- ❌ Analytics or tracking pixels
- ❌ Third-party APIs

All assets are embedded in the HTML file.

## Quick Start

### Test the WebSocket Connection

**Using Browser Console:**
```javascript
const ws = new WebSocket('wss://clouddata.turbowarp.org');
ws.onopen = () => ws.send('{"method":"handshake","project_id":"1251551334","user":"test"}');
ws.onmessage = (e) => console.log('Received:', e.data);
```

**Using websocat:**
```bash
websocat wss://clouddata.turbowarp.org
# Then type: {"method":"handshake","project_id":"1251551334","user":"test"}
```

**Using wscat:**
```bash
npm install -g wscat
wscat -c wss://clouddata.turbowarp.org
# Then type: {"method":"handshake","project_id":"1251551334","user":"test"}
```

## Example Messages

### Client → Server (Handshake)
```json
{
  "method": "handshake",
  "project_id": "1251551334",
  "user": "myusername"
}
```

### Client → Server (Update Variable)
```json
{
  "method": "set",
  "project_id": "1251551334",
  "user": "myusername",
  "name": "☁ myVariable",
  "value": "100"
}
```

### Server → Client (Variable Update)
```json
{
  "method": "set",
  "name": "☁ myVariable",
  "value": "150"
}
```

## File Analysis

### Source File
- **Name:** `meine erste cloud variable.html`
- **Location:** `/docs/meine erste cloud variable.html`
- **Type:** Self-contained Scratch/TurboWarp project
- **Technology:** TurboWarp Packager output

### Code Location
The cloud variable connection is established on **line 252**:
```javascript
scaffolding.addCloudProvider(new Scaffolding.Cloud.WebSocketProvider(
    ["wss://clouddata.turbowarp.org","wss://clouddata.turbowarp.xyz"], 
    "1251551334"
));
```

## Security Notes

⚠️ **Important Security Information:**

1. **Public Data:** Cloud variables are publicly accessible to anyone
2. **No Authentication:** Only requires a username (can be any string)
3. **Do Not Store Sensitive Data:** Anyone with the project ID can read/write
4. **Rate Limiting:** Servers may limit update frequency
5. **Server Validation:** Invalid usernames or blocked projects may be rejected

## Monitoring in Browser

To monitor WebSocket traffic:

1. Open Developer Tools (F12)
2. Go to **Network** tab
3. Filter by **WS** (WebSocket)
4. Click the connection to see frames
5. View **Messages** tab for sent/received data

## Purpose of Cloud Variables

Cloud variables enable:
- 🌍 Cross-device data synchronization
- 👥 Multi-user collaboration
- 📊 Shared scoreboards/leaderboards
- 💬 Simple messaging systems
- 🎮 Multiplayer game features

## Related Resources

- [TurboWarp Documentation](https://docs.turbowarp.org/)
- [Scratch Cloud Variables](https://en.scratch-wiki.info/wiki/Cloud_Data)
- [WebSocket Protocol (RFC 6455)](https://datatracker.ietf.org/doc/html/rfc6455)

## Questions or Issues?

For questions about this analysis, please refer to:
- The detailed documentation in `NETWORK_REQUESTS_DOCUMENTATION.md`
- The quick reference in `NETWORK_REQUESTS_QUICK_REFERENCE.md`

---

**Analysis Date:** 2026-01-09  
**Analyzer:** GitHub Copilot  
**Project ID:** 1251551334
