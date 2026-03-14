# Manual Deployment to GitHub Pages

This document explains the specific changes made to the project to enable manual deployment to GitHub Pages.

## Core Adjustments for Deployment

To ensure the application works correctly when hosted on GitHub Pages (either as a project page or a user/org page), the following technical adjustments were implemented:

### 1. Router Configuration (Hash History)

GitHub Pages does not support single-page application (SPA) fallback by default. To avoid `404 Not Found` errors when refreshing pages or navigating directly to a sub-route, the application uses **Hash Routing**.

- **File**: [src/router/routes.ts]
- **Change**: Switched from default browser history to `createHashHistory` from `@tanstack/react-router`.

```typescript
import { createRouter, createHashHistory } from "@tanstack/react-router";

// ...

const history = createHashHistory();

export const router = createRouter({
  routeTree,
  history,
});
```

### 2. Webpack Public Path

To ensure that generated assets (scripts, styles, images) are loaded correctly regardless of the URL nesting on GitHub Pages, the Webpack `publicPath` is set to `./`.

- **File**: [webpack.config.js]
- **Change**: Set `output.publicPath` to `./`.

```javascript
module.exports = {
  // ...
  output: {
    filename: "[name].[chunkhash].js",
    publicPath: "./",
  },
  // ...
};
```

## How to Deploy Manually

1. **Build the Project**:
   Run the build script to generate the production-ready files in the `dist` directory.
   ```bash
   npm run build
   ```

2. **Deploy to GitHub**:
   - You can manually upload the contents of the `dist` folder to your GitHub repository.
   - Alternatively, use a tool like `gh-pages` or manually push the `dist` folder to the `gh-pages` branch.
