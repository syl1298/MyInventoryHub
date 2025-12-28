# Bug Issues - Full Stack Application Integration

**Date:** December 28, 2025  
**Project:** ClientApp & ServerApp Full Stack Integration

---

## 🐛 Issues Identified

### 1. **API Route Mismatch**
- **Severity:** High
- **Component:** ClientApp ↔ ServerApp Integration
- **Description:** 
  - Frontend was calling `/api/products`
  - Backend was serving `/api/products` initially
  - Requirement changed to `/api/productlist`
  - Mismatch caused 404 Not Found errors

**Impact:**
- Product data could not be fetched
- Application failed to display inventory information

---

### 2. **CORS Configuration Issues**
- **Severity:** High
- **Component:** ServerApp (Backend)
- **Description:**
  - Initial CORS configuration used named policy "AllowBlazorClient"
  - More flexible CORS policy was needed for development
  - Cross-origin requests from Blazor WebAssembly client were blocked

**Symptoms:**
- Browser console showed CORS policy errors
- Requests from `http://localhost:5166` to `http://localhost:5261` were blocked
- "Access to fetch at '...' has been blocked by CORS policy" errors

---

### 3. **JSON Deserialization Error Handling**
- **Severity:** Medium
- **Component:** ClientApp (Frontend)
- **Description:**
  - No specific error handling for malformed JSON responses
  - Generic exception handling didn't differentiate JSON parsing errors
  - Limited visibility into deserialization failures

**Impact:**
- Difficult to debug JSON-related issues
- Users received generic error messages
- No logging for troubleshooting

---

### 4. **Port Conflicts**
- **Severity:** Low
- **Component:** Development Environment
- **Description:**
  - Multiple instances of applications running simultaneously
  - Port 5166 and 5261 were already in use
  - Processes couldn't bind to required addresses

**Symptoms:**
- "Address already in use" errors
- "Failed to bind to address" exceptions
- Applications couldn't start

---

### 5. **Build Errors from Solution Structure**
- **Severity:** Medium
- **Component:** Build System
- **Description:**
  - Running `dotnet run` from parent directory tried to build entire solution
  - Multiple projects with conflicting dependencies
  - Assembly attribute duplication errors

**Symptoms:**
- CS0579 errors for duplicate attributes
- CS0246 errors for missing types
- Build failures when not in specific project directory

---

## 📊 Error Logs

### CORS Error Example:
```
Access to fetch at 'http://localhost:5261/api/products' from origin 'http://localhost:5166' 
has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the 
requested resource.
```

### Port Conflict Error Example:
```
System.IO.IOException: Failed to bind to address http://127.0.0.1:5166: address already in use.
---> Microsoft.AspNetCore.Connections.AddressInUseException: Only one usage of each socket 
address (protocol/network address/port) is normally permitted.
```

### JSON Deserialization Error Example (Potential):
```
System.Text.Json.JsonException: The JSON value could not be converted to Product[]. 
Path: $ | LineNumber: 0 | BytePositionInLine: 1.
```

---

## 🎯 Root Causes

1. **Insufficient CORS Configuration:** Named policy was too restrictive for development
2. **Lack of Specific Error Handling:** Generic catch blocks masked specific error types
3. **API Route Updates:** Manual synchronization needed between frontend and backend
4. **Process Management:** No cleanup of previous running instances
5. **Solution-Wide Build:** Running from wrong directory caused cascading build issues

---

## ✅ Testing Scenarios That Failed

- ❌ Fetching products from `/api/products` after route change
- ❌ Cross-origin requests without proper CORS
- ❌ Handling malformed JSON responses
- ❌ Starting applications with port conflicts
- ❌ Building from solution root directory

---

**Status:** All issues resolved (see FIXES_SUMMARY.md)
