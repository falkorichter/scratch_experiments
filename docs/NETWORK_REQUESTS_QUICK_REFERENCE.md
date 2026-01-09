# Quick Reference: Network Requests in "meine erste cloud variable.html"

## TL;DR

The HTML file makes **only ONE type** of outgoing request:
- **WebSocket connection** to cloud variable servers for syncing shared data

## Connection Details

```
Protocol: WebSocket (WSS)
Servers:  wss://clouddata.turbowarp.org (primary)
          wss://clouddata.turbowarp.xyz (fallback)
Project:  1251551334
```

## Test the Connection

### Quick Browser Test
```javascript
const ws = new WebSocket('wss://clouddata.turbowarp.org');
ws.onopen = () => ws.send('{"method":"handshake","project_id":"1251551334","user":"test"}');
ws.onmessage = (e) => console.log(e.data);
```

### Using websocat
```bash
websocat wss://clouddata.turbowarp.org
# Then paste: {"method":"handshake","project_id":"1251551334","user":"test"}
```

### Using wscat
```bash
wscat -c wss://clouddata.turbowarp.org
# Then paste: {"method":"handshake","project_id":"1251551334","user":"test"}
```

## Message Examples

### Handshake
```json
{"method":"handshake","project_id":"1251551334","user":"myusername"}
```

### Set Variable
```json
{"method":"set","project_id":"1251551334","user":"myusername","name":"☁ score","value":"100"}
```

### Receive Update (from server)
```json
{"method":"set","name":"☁ score","value":"150"}
```

## What's NOT in this file

✗ HTTP/HTTPS requests  
✗ External images or scripts  
✗ Fetch API calls  
✗ XMLHttpRequest  
✗ Analytics or tracking  

Everything is self-contained and embedded in the HTML file.

## See Full Documentation

For complete details, see [NETWORK_REQUESTS_DOCUMENTATION.md](./NETWORK_REQUESTS_DOCUMENTATION.md)
