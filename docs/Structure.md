Here’s the recommended project structure for the housekeeping app, designed around the offline-first, WebSocket-driven, and staff‑tool UX principles we just covered.

```
HousekeepingApp/
├── app.json                   # Expo config
├── package.json
├── tsconfig.json
├── babel.config.js
├── metro.config.js            # if needed for WatermelonDB
│
├── assets/                    # icons, sounds (few – staff tools are sparse)
│
└── src/
    ├── App.tsx                # entry point: providers, navigation, initializers
    │
    ├── db/
    │   ├── index.ts           # WatermelonDB setup, database instance
    │   ├── schema.ts          # table definitions (rooms, tasks)
    │   └── migrations.ts     # schema versioning
    │
    ├── models/                # WatermelonDB model classes
    │   ├── Room.ts
    │   └── Task.ts
    │
    ├── store/                 # Zustand stores (fast UI state)
    │   ├── useRoomStore.ts    # rooms array, selected room, filters
    │   ├── useTaskStore.ts    # tasks for current room
    │   └── useAuthStore.ts    # user, PIN, token
    │
    ├── services/
    │   ├── socket.ts          # WebSocket connection, event handlers
    │   ├── api.ts             # REST fallback for sync (when WS not available)
    │   └── syncEngine.ts      # Listens to NetInfo, flushes offline queue
    │
    ├── utils/
    │   ├── offlineQueue.ts    # MMKV-based queue of pending actions
    │   ├── auth.ts            # token/PIN storage helpers
    │   ├── networkListener.ts # NetInfo subscription wrapper
    │   └── colors.ts          # staff-optimized palette (high contrast)
    │
    ├── hooks/
    │   ├── useUpdateRoomStatus.ts   # optimistic update + queue + WS emit
    │   ├── useSyncOnReconnect.ts    # triggers sync engine
    │   └── useSocket.ts             # provides socket instance from store
    │
    ├── components/            # lean, touch-optimized UI pieces
    │   ├── RoomRow.tsx        # swipeable, color-coded list item
    │   ├── StatusBadge.tsx    # bright DIRTY/CLEAN/INSPECTED pill
    │   ├── QuickAssignDrawer.tsx  # supervisor drag (or press) reassign
    │   └── BarcodeScannerModal.tsx
    │
    ├── screens/
    │   ├── LoginScreen.tsx    # simple PIN entry
    │   ├── RoomListScreen.tsx # attendant’s main dashboard
    │   ├── TaskDetailScreen.tsx
    │   ├── AssignmentScreen.tsx    # supervisor view of all rooms/attendants
    │   └── RoomVerificationScreen.tsx  # after QR scan
    │
    └── navigation/
        ├── RootNavigator.tsx  # stack for auth / main
        └── MainTabNavigator.tsx  # bottom tabs for attendant vs supervisor
```

### Why it’s structured this way

1. **Offline-first at the core**: `db/` and `models/` hold the single source of truth – WatermelonDB. The UI reads from there, never directly from API responses.

2. **Zustand `store/`** bridges the reactive database and the screens. It provides quick, observable lists that update instantly when a room status changes locally (even offline).

3. **`services/` are isolated**:  
   - `socket.ts` handles all real‑time push,  
   - `api.ts` covers REST fallback,  
   - `syncEngine.ts` watches connectivity and flushes the `utils/offlineQueue`.

4. **`hooks/` encapsulate complex interactions** like optimistic UI updates + queuing, so screens stay clean and declarative.

5. **`components/` and `screens/`** are kept flat and pragmatic. No over‑engineered atomic design; just reusable pieces that survive heavy use (e.g., `RoomRow` with swipe actions).

6. **No onboarding screens, minimal assets** – the whole point is to get staff working as fast as if they had a clipboard and pen.

This structure scales easily: you can add more models, more stores, or a supervisor’s reporting dashboard without disrupting the offline/real‑time core.