
This guide helps you diagnose and resolve common issues with device authorization flow. Learn how to handle errors, debug problems, and implement proper error recovery.

## Common error codes during device authorization

### authorization_pending

**Error**: `authorization_pending`
**Description**: The user hasn't completed the authorization process yet.

**Solution**: Continue polling the token endpoint. This is normal behavior.

```javascript
// Example: Handle authorization_pending
if (error === "authorization_pending") {
  console.log("User has not completed authorization yet. Continue polling...");
  // Wait for the specified interval before next poll
  setTimeout(pollForToken, interval * 1000);
}
```

### slow_down

**Error**: `slow_down`
**Description**: You're polling too frequently.

**Solution**: Increase the polling interval by 5 seconds.

```javascript
// Example: Handle slow_down
if (error === "slow_down") {
  console.log("Polling too fast. Increasing interval...");
  interval += 5; // Increase interval by 5 seconds
  setTimeout(pollForToken, interval * 1000);
}
```

### access_denied

**Error**: `access_denied`
**Description**: The user denied the authorization request.

**Solution**: Stop polling and inform the user they need to try again.

```javascript
// Example: Handle access_denied
if (error === "access_denied") {
  console.log("User denied authorization");
  showErrorMessage("Authorization was denied. Please try again.");
  stopPolling();
}
```

### expired_token

**Error**: `expired_token`
**Description**: The device code has expired (typically after 30 minutes).

**Solution**: Request a new device code.

```javascript
// Example: Handle expired_token
if (error === "expired_token") {
  console.log("Device code expired");
  showErrorMessage("This code has expired. Please request a new one.");
  requestNewDeviceCode();
}
```

## Polling mistakes during device authorization

### Too frequent polling

**Problem**: Polling more frequently than the recommended interval.

**Solution**: Always respect the `interval` value from the device authorization response.

```javascript
// Good: Respect the interval
function pollForToken(deviceCode, interval = 5) {
  setTimeout(() => {
    // Make token request
    checkTokenStatus(deviceCode);
  }, interval * 1000);
}

// Bad: Polling too frequently
function pollForToken(deviceCode) {
  setInterval(() => {
    // This polls every 1 second - too frequent!
    checkTokenStatus(deviceCode);
  }, 1000);
}
```

### Not handling `slow_down` properly

**Problem**: Not increasing the interval when receiving `slow_down` errors.

**Solution**: Implement exponential backoff.

```javascript
let currentInterval = 5; // Start with 5 seconds

function pollForToken(deviceCode) {
  checkTokenStatus(deviceCode).then((response) => {
    if (response.error === "slow_down") {
      currentInterval += 5; // Increase by 5 seconds
      console.log(`Increasing interval to ${currentInterval} seconds`);
    }

    // Continue polling with updated interval
    setTimeout(() => pollForToken(deviceCode), currentInterval * 1000);
  });
}
```

### Not stopping on errors

**Problem**: Continuing to poll after receiving fatal errors.

**Solution**: Stop polling for non-recoverable errors.

```javascript
function pollForToken(deviceCode) {
  checkTokenStatus(deviceCode).then((response) => {
    if (response.error) {
      switch (response.error) {
        case "authorization_pending":
          // Continue polling
          setTimeout(() => pollForToken(deviceCode), interval * 1000);
          break;
        case "slow_down":
          // Increase interval and continue
          interval += 5;
          setTimeout(() => pollForToken(deviceCode), interval * 1000);
          break;
        case "access_denied":
        case "expired_token":
          // Stop polling - these are fatal errors
          stopPolling();
          handleError(response.error);
          break;
      }
    } else {
      // Success - stop polling
      handleSuccess(response);
    }
  });
}
```

## Network issues during device authorization

### Connection timeouts

**Problem**: Network requests timing out.

**Solution**: Implement proper timeout handling and retry logic.

```javascript
function checkTokenStatus(deviceCode) {
  return fetch("https://<your-subdomain>.kinde.com/oauth2/token", {
    method: "POST",
    headers: {
      "Content-Type": "application/x-www-form-urlencoded"
    },
    body: new URLSearchParams({
      grant_type: "urn:ietf:params:oauth:grant-type:device_code",
      client_id: "<YOUR_CLIENT_ID>",
      device_code: deviceCode
    }),
    timeout: 10000 // 10 second timeout
  })
    .then((response) => response.json())
    .catch((error) => {
      console.error("Network error:", error);
      // Retry after a delay
      setTimeout(() => checkTokenStatus(deviceCode), 5000);
    });
}
```

### DNS resolution issues

**Problem**: Cannot resolve the Kinde domain.

**Solution**: Verify your domain configuration and network connectivity.

```bash
# Test DNS resolution
nslookup <your-subdomain>.kinde.com

# Test connectivity
curl -I https://<your-subdomain>.kinde.com/oauth2/v2/device_authorization
```
