# Network Requests Documentation for "meine erste cloud variable.html"

This document provides a comprehensive analysis of all outgoing network requests found in the HTML file `meine erste cloud variable.html`.

## Summary

The HTML file is a packaged Scratch/TurboWarp project that makes **one type of outgoing request**: WebSocket connections for cloud variables.

## 1. Cloud Variable WebSocket Connection

### Description
The project establishes a WebSocket connection to cloud variable servers to enable synchronization of cloud variables across multiple instances of the project.

### Connection Details

**Protocol:** WebSocket (WSS - Secure WebSocket)

**Servers (Fallback List):**
1. `wss://clouddata.turbowarp.org`
2. `wss://clouddata.turbowarp.xyz`

**Project ID:** `1251551334`

**Location in Code:** Line 252

```javascript
scaffolding.addCloudProvider(new Scaffolding.Cloud.WebSocketProvider(
    ["wss://clouddata.turbowarp.org","wss://clouddata.turbowarp.xyz"], 
    "1251551334"
));
```

### Purpose
- Synchronize cloud variables between different users/sessions
- Store and retrieve shared data across project instances
- Enable real-time collaborative features in Scratch projects

### Request Flow

#### 1. Initial Handshake
When the WebSocket connection is established, the client sends a handshake message:

**Message Format (JSON):**
```json
{
  "method": "handshake",
  "project_id": "1251551334",
  "user": "[username]"
}
```

#### 2. Setting Cloud Variables
When a cloud variable is updated, the client sends:

**Message Format (JSON):**
```json
{
  "method": "set",
  "project_id": "1251551334",
  "user": "[username]",
  "name": "☁ [variable_name]",
  "value": "[variable_value]"
}
```

#### 3. Receiving Updates
The server sends updates to clients when cloud variables change:

**Received Message Format (JSON):**
```json
{
  "method": "set",
  "name": "☁ [variable_name]",
  "value": "[variable_value]"
}
```

### Connection Behavior

1. **Fallback Mechanism:** The client attempts to connect to the first server, and if that fails, it tries the next server in the list
2. **Reconnection:** If the connection is lost, the client automatically attempts to reconnect with exponential backoff (up to 32 seconds between attempts)
3. **Username:** The username is sent with each message (can be set via `scaffolding.setUsername()`)
4. **Buffering:** Messages are buffered if the connection is not ready and sent once the connection is established

### Error Codes

The implementation handles specific WebSocket close codes:

| Code | Meaning | Action |
|------|---------|--------|
| 4002 | Invalid username | Does not reconnect |
| 4003 | Server is full | Does not reconnect |
| 4004 | Project is blocked | Does not reconnect |
| Other | Connection lost | Reconnects with exponential backoff |

## Sample cURL Requests

WebSocket connections cannot be directly replicated with cURL as they require bidirectional communication. However, here are equivalent demonstrations:

### Testing WebSocket Connection (using websocat)

If you have `websocat` installed, you can test the connection:

```bash
# Connect to the WebSocket server
websocat wss://clouddata.turbowarp.org

# Then send a handshake (paste this after connecting):
{"method":"handshake","project_id":"1251551334","user":"testuser"}

# Send a variable update:
{"method":"set","project_id":"1251551334","user":"testuser","name":"☁ myVar","value":"123"}
```

### Testing WebSocket Connection (using wscat)

If you have `wscat` installed:

```bash
# Install wscat if needed
npm install -g wscat

# Connect to the WebSocket server
wscat -c wss://clouddata.turbowarp.org

# Then send messages (one per line):
{"method":"handshake","project_id":"1251551334","user":"testuser"}
{"method":"set","project_id":"1251551334","user":"testuser","name":"☁ myVar","value":"123"}
```

### Browser Console Example

You can also test the WebSocket connection directly in the browser console:

```javascript
// Open a connection
const ws = new WebSocket('wss://clouddata.turbowarp.org');

// Handle connection open
ws.onopen = () => {
    console.log('Connected!');
    // Send handshake
    ws.send(JSON.stringify({
        method: 'handshake',
        project_id: '1251551334',
        user: 'testuser'
    }));
};

// Handle incoming messages
ws.onmessage = (event) => {
    console.log('Received:', event.data);
};

// Handle errors
ws.onerror = (error) => {
    console.error('WebSocket error:', error);
};

// Handle connection close
ws.onclose = (event) => {
    console.log('Connection closed:', event.code, event.reason);
};

// To send a variable update after connected:
ws.send(JSON.stringify({
    method: 'set',
    project_id: '1251551334',
    user: 'testuser',
    name: '☁ myVariable',
    value: '100'
}));
```

## Data Fetching Requests

### Cloud Variable Data Retrieval

**Type:** WebSocket subscription/listener

**Direction:** Bidirectional (both send and receive)

**Data Format:** JSON

**Data Retrieved:**
- Cloud variable names (prefixed with ☁)
- Cloud variable values (strings or numbers)

**Privacy Note:** Cloud variables are **public** and can be read by anyone with access to the project ID. Do not store sensitive information in cloud variables.

## No Other External Requests

**Important:** This HTML file does NOT make any of the following types of requests:
- HTTP/HTTPS GET requests
- HTTP/HTTPS POST requests  
- Fetch API calls
- XMLHttpRequest calls
- Image loading from external sources
- External script loading
- External stylesheet loading
- External font loading

All project assets (images, sounds, code) are embedded directly in the HTML file as base64-encoded data.

## Technical Implementation Details

### Cloud Variable Features in Code

The implementation includes:

1. **CloudManager** class - Manages cloud variable providers
2. **WebSocketProvider** class - Handles WebSocket connections and message buffering
3. **LocalStorageProvider** class - Optional local storage fallback (not used in this file by default)

### Key Code Components

```javascript
// Cloud variable connection setup (line 252)
scaffolding.addCloudProvider(
    new Scaffolding.Cloud.WebSocketProvider(
        ["wss://clouddata.turbowarp.org","wss://clouddata.turbowarp.xyz"], 
        "1251551334"
    )
);
```

## Security Considerations

1. **Public Data:** All cloud variables are publicly accessible and visible to anyone
2. **Rate Limiting:** The server may implement rate limiting on variable updates
3. **Validation:** The server validates usernames and may reject invalid ones
4. **Connection Limits:** Servers may limit the number of concurrent connections
5. **No Authentication:** Cloud variables do not require authentication beyond a username

## Debugging and Monitoring

To monitor cloud variable traffic in the browser:

1. Open the browser's Developer Tools (F12)
2. Go to the Network tab
3. Filter by "WS" (WebSocket)
4. Click on the WebSocket connection to see frames
5. Watch the "Messages" tab to see all sent/received data

## Summary Table

| Request Type | Protocol | Endpoints | Purpose | Frequency |
|-------------|----------|-----------|---------|-----------|
| Cloud Variables | WebSocket (WSS) | `wss://clouddata.turbowarp.org`<br>`wss://clouddata.turbowarp.xyz` | Sync cloud variables | On project start, then as needed for updates |

---

**Document Version:** 1.0  
**Last Updated:** 2026-01-09  
**HTML File:** `meine erste cloud variable.html`  
**Project ID:** 1251551334
