---
description: DeSo Frontend Starter for NextJS and React apps
---

# 3️⃣ Frontend: NextJS Example

**Primary Contributor & Maintainer:** [**@brootle**](https://focus.xyz/brootle)&#x20;

A modern frontend web application built using **Next.js App Router** and designed to integrate with the [**DeSo Protocol**](https://github.com/deso-protocol) — a decentralized social blockchain platform.

📦 **Repository**: [brootle/deso-starter-nextjs-plus](https://github.com/brootle/deso-starter-nextjs-plus)

This starter includes:

* DeSo authentication via Identity service
* Profile selector and alternate identity switching
* Advanced commenting system with optimistic updates
* Network-resilient data fetching and caching
* Clean UI component system (Buttons, Inputs, Dropdowns, Select, etc.)
* Support for Floating UI dropdowns and portals
* Dark/light theming via CSS variables
* Storybook for component exploration

***

### 🔥 Features

* 🔐 **DeSo Auth**: Log in using DeSo Identity
* 👥 **Alt Profile Switcher**: Switch between multiple public keys
* 🔎 **Search Profiles**: Find users by public key or username
* 📝 **Post Support**: Read and create posts on the DeSo blockchain
* 💬 **Advanced Comments**: Inline replies with optimistic updates and smart caching
* 🌐 **Network Resilient**: Handles offline/online transitions gracefully
* 👻 **Profileless Accounts**: Fully functional even without a user profile
* 🎨 **Component Library**: Custom Select, MenuItem, Avatar, and Dropdown components
* 🌐 **Responsive UI**: Built with modular CSS and theme tokens
* 📦 **Floating UI**: Precise positioning via `@floating-ui/react`
* 🧱 **Scalable Structure**: Clean folder structure for extending easily

#### 🧠 **State Management with React Query**

This starter uses [**TanStack React Query**](https://tanstack.com/query/latest) for efficient, declarative data fetching and caching with enterprise-grade reliability.

✅ **Core Benefits:**

* Smart caching and deduplication of network requests
* Declarative `useQuery` / `useMutation` hooks
* Built-in error/loading states
* React Query Devtools support (optional)

✅ **Network Resilience Features:**

* **Wake-from-sleep protection** - No more "failed to fetch" errors when laptop wakes up
* **Smart retry logic** - Won't retry when offline or for client errors
* **Progressive retry delays** - Intelligent backoff (1s, 2s, 4s, 8s, max 15s)
* **Offline awareness** - Graceful handling of network transitions
* **Centralized configuration** - Consistent behavior across all pages

#### 💬 **Advanced Comment System**

The commenting system features sophisticated state management and user experience optimizations:

✅ **Real-time Features:**

* **Optimistic updates** - Comments appear instantly while syncing to blockchain
* **Local/remote merging** - Seamlessly combines user's new comments with server data
* **Infinite pagination** - Load more comments on demand
* **Smart deduplication** - Prevents duplicate comments across page loads

✅ **User Experience:**

* **Inline replies** - Reply directly from any post without page navigation
* **Expand/collapse** - Show/hide comment threads with state persistence
* **Comment promotion** - Local comments become permanent after server sync
* **Visual feedback** - Clear loading states and error handling

Usage examples include:

* Fetching user profiles by public key or username
* Fetching posts and comments with infinite scroll
* Creating replies with optimistic UI updates
* Managing complex UI state like comment visibility
* Handling username → public key resolution for notifications and feeds

Query keys are centralized in `/queries/queryKeys.js`, UI keys in `/queries/uiKeys.js`, and network configuration in `/queries/queryClientConfig.js` for consistency and maintainability.

> 🔧 Profile editing and mutations use `invalidateQueries()` for cache synchronization and support optimistic updates.

### 🚀 Getting Started

#### 1. Clone the repository

```bash
git clone https://github.com/brootle/deso-starter-nextjs-plus.git
cd deso-starter-nextjs-plus
```

#### 2. Install dependencies

```bash
npm install
```

#### 3. Start the dev server

```bash
npm run dev
```

Visit `http://localhost:3000` to view the app.

***

### 🧪 Storybook

Run Storybook to browse UI components in isolation:

```bash
npm run storybook
```

Opens at: `http://localhost:6006`

***

### 🛠 Tech Stack

* **Framework**: [Next.js App Router](https://nextjs.org/docs/app) (v15.2.4)
* **UI Logic**: React 19 + CSS Modules
* **Data Fetching & Caching**: [React Query v5](https://tanstack.com/query/latest) with network-aware configuration
* **State Management**: React Context (Auth, User) + React Query for UI state
* **Floating Dropdowns**: [`@floating-ui/react`](https://floating-ui.com/)
* **DeSo Identity**: Authentication via DeSo Identity service
* **Theming**: CSS variable-based dark/light support

***

### 🧩 Folder Structure

```
/api               → DeSo API abstraction hooks and handlers
/app               → Next.js App Router structure (routes, pages, layout)
/assets            → Static assets like icons and illustrations
/components        → Reusable UI components (Button, Select, Input, etc.)
/config            → Environment-independent constants (e.g. API base URLs)
/context           → Global state via React Context API (Auth, User, QueryProvider)
/hooks             → Custom React hooks (e.g. useClickOutside, useToast)
/layouts           → Shared layout components (MainLayout, etc.)
/queries           → React Query configuration and key definitions
  ├── queryKeys.js         → API query keys
  ├── uiKeys.js           → UI state keys  
  ├── queryClientConfig.js → Network-aware configuration
  └── index.js            → Clean exports
/styles            → Theme system and shared styles (CSS Modules + variables)
/utils             → Helper functions (auth, DeSo profiles, tokens)
```

***

### 🔧 Query Configuration

The app features a sophisticated React Query setup optimized for reliability:

#### **Network-Aware Retry Logic**

```javascript
// Won't retry when offline or for client errors (4xx)
// Uses progressive delays: 1s → 2s → 4s → 8s → max 15s
const networkAwareRetry = (failureCount, error) => {
  if (!navigator.onLine) return false;
  if (error?.status >= 400 && error?.status < 500) return false;
  return failureCount < 2;
};
```

#### **Wake-from-Sleep Protection**

```javascript
// Prevents "failed to fetch" errors when laptop wakes up
refetchOnReconnect: false,  // Key setting
refetchOnWindowFocus: false,
```

#### **Smart Cache Management**

* **API queries**: 2-minute stale time, 10-minute cache time
* **Search queries**: 30-second stale time for fresh results
* **Comments**: Infinite stale time for persistent threading
* **UI state**: Cached for consistent user experience

***

### 📜 License

This project is open-sourced under the MIT License.

***

### 🤝 Contributing

Pull requests are welcome! Open issues or suggestions any time.

***

### 🌍 Credits

Built using the [DeSo Protocol](https://github.com/deso-protocol) — the decentralized social blockchain.

***

### 🪲 Known Issues and Bug Reports

During development, several minor issues were identified with the DeSo backend API:

#### Open Issues

* **Unresolved:** See [this bug report](https://github.com/deso-protocol/backend/issues/736) for details on an issue that remains open.

#### Resolved Issues

* **Fixed:** A previously reported bug has been addressed. Refer to [this issue](https://github.com/deso-protocol/backend/issues/725) for more information.

If you encounter additional issues, please report them via the appropriate GitHub repository.
