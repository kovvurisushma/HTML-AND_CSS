# Web Storage - 

## Storage Comparison Table

| Storage | Scope | Persistence | Size | Server Access | Data Types |
|---------|-------|-------------|------|---------------|------------|
| **localStorage** | Origin-wide | Until cleared | ~5-10MB | No | Strings only |
| **sessionStorage** | Origin + Tab | Until tab closes | ~5-10MB | No | Strings only |
| **Cookies** | Domain-wide | Configurable | ~4KB | Yes (auto) | Strings only |
| **Window Object** | Page-only | Until refresh | RAM limit | No | Any JS type |

---

## localStorage

### Key Points
- **Persists**: Until manually cleared or user clears browser data
- **Shared**: Across all tabs of same origin
- **Use for**: User preferences, shopping cart, app settings

### Syntax
```javascript
localStorage.setItem('key', 'value');
localStorage.getItem('key');
localStorage.removeItem('key');
localStorage.clear();
```

---

## sessionStorage

### Key Points
- **Persists**: Until tab closes (survives page refresh)
- **Isolated**: Each tab has separate storage
- **Use for**: Form progress, temporary UI state, tab-specific data

### Syntax
```javascript
sessionStorage.setItem('key', 'value');
sessionStorage.getItem('key');
sessionStorage.removeItem('key');
```

---

## Cookies

### Key Points
- **Persists**: Based on expiration date
- **Server**: Automatically sent with HTTP requests
- **Cross-domain**: Can work across subdomains
- **Use for**: Authentication, session management

### Syntax
```javascript
document.cookie = "name=value; expires=date; path=/; domain=.site.com";
// Reading requires parsing document.cookie string
```

---

## Window Object

### Key Points
- **Persists**: Only until page refresh/navigation
- **Fast**: Direct memory access
- **Flexible**: Can store functions, objects, any JS type
- **Use for**: Temporary UI state, SPA global state

### Syntax
```javascript
window.data = { any: 'javascript', type: true };
delete window.data;
```

---

## Origin vs Domain

### Origin-Based (localStorage, sessionStorage)
**Origin = Protocol + Domain + Port**
 - Same: https://site.com/page1 & https://site.com/page2
 - Different: https://site.com & http://site.com (protocol)
 - Different: https://site.com & https://shop.site.com (subdomain)

### Domain-Based (Cookies)

---

## When to Use Each

### localStorage
- User preferences (theme, language)
- Shopping cart contents
- Data that should survive browser restart

### sessionStorage
- Multi-step form progress
- Temporary workflow data
- Tab-specific information

### Cookies
-  Authentication tokens
-  Session management
-  Data server needs automatically

### Window Object
-  Temporary UI state
-  SPA global variables
-  Fast-access cache

---

## SPA vs Traditional Sites

### Traditional Website
```javascript
// Page navigation = new window object
Page1: window.data = "test" → Navigate → Page2: window.data = undefined
```

### SPA
```javascript
// Route changes = same window object
"Page1": window.data = "test" → "Navigate" → "Page2": window.data = "test"
// But refresh still clears window object
```

---

## Security & Best Practices

###  Never Store
- Passwords
- Credit card numbers
- Private API keys
- Sensitive personal data

###  Always Do
```javascript
// Check if storage is available
try {
    localStorage.setItem('test', 'test');
    localStorage.removeItem('test');
} catch (e) {
    // Handle storage not available
}

// Check if data exists
const data = localStorage.getItem('key');
if (data) {
    const parsed = JSON.parse(data);
}
```

---

## Common Interview Questions

### Q: Why not use localStorage for payment gateway redirects?
**A**: External domains can't access your localStorage. When user goes to payment site and returns, localStorage might be cleared by browser security policies.

### Q: Difference between sessionStorage and window object in SPA?
**A**: sessionStorage survives page refresh, window object doesn't. sessionStorage requires JSON serialization, window object can store any JS type directly.

### Q: Can other domains access your cookies?
**A**: No, cookies follow same-origin policy. External domains cannot read or modify your cookies.

### Q: What happens to window object on page navigation?
**A**: Completely destroyed and recreated. Browser creates new JavaScript execution context for each page load.

---

## Quick Code Examples

### Hybrid Storage Pattern
```javascript
// Use both for reliability
function saveUserData(data) {
    // Fast access
    window.userData = data;
    
    // Persistent storage
    localStorage.setItem('userData', JSON.stringify(data));
}

function getUserData() {
    // Try window first (faster)
    if (window.userData) return window.userData;
    
    // Fallback to localStorage
    const stored = localStorage.getItem('userData');
    return stored ? JSON.parse(stored) : null;
}
```

### Error Handling
```javascript
function safeStorage(key, value) {
    try {
        localStorage.setItem(key, JSON.stringify(value));
        return true;
    } catch (e) {
        console.error('Storage failed:', e);
        return false;
    }
}
```
