# Chrome Extension Support Changes

## Overview

This document describes the changes made to enable Chrome extension support in chrome-devtools-mcp.

## What Was Changed

### 1. Made Extension Pages Visible

**File:** `src/browser.ts`

**Change:** Removed `'chrome-extension://'` from the `ignoredPrefixes` set

**Before:**
```typescript
function makeTargetFilter(enableExtensions = false) {
  const ignoredPrefixes = new Set(['chrome://', 'chrome-untrusted://']);
  if (!enableExtensions) {
    ignoredPrefixes.add('chrome-extension://');
  }
  // ...
}
```

**After:**
```typescript
function makeTargetFilter(enableExtensions = false) {
  const ignoredPrefixes = new Set(['chrome://', 'chrome-untrusted://']);
  // ...
}
```

**Effect:** Extension tabs now appear in the page list when calling `list_pages`.

### 2. Enabled Extensions by Default

**File:** `src/bin/chrome-devtools-mcp-cli-options.ts`

**Change:** Changed `categoryExtensions` default from `false` to `true`

**Before:**
```typescript
categoryExtensions: {
  type: 'boolean',
  default: false,
  hidden: true,
  describe: 'Set to false to exclude tools related to extensions.',
},
```

**After:**
```typescript
categoryExtensions: {
  type: 'boolean',
  default: true,
  hidden: true,
  describe: 'Set to false to exclude tools related to extensions.',
},
```

**Effect:** Extensions are now enabled by default when launching Chrome (passes `enableExtensions: true` to Puppeteer).

## How to Build and Install

After making these changes:

```bash
npm run build
npm install -g .
```

## Known Limitations

### Network Logging

Network monitoring for extension pages (`chrome-extension://`) has limitations:

- Extension resources are loaded from the local file system, not over the network
- Chrome has security restrictions on monitoring extension page network activity
- Some extension requests use internal Chrome protocols that aren't logged the same way as HTTP/HTTPS requests

This is a **browser-level limitation**, not a code issue that can be fixed in the MCP server.

## Testing

To verify the changes work:

1. Start chrome-devtools-mcp
2. Install or load a Chrome extension (e.g., via `chrome://extensions/`)
3. Call `list_pages` - extension pages should now be visible
4. You can select and interact with extension pages

## Date

March 18, 2026