# Housekeeping Task App: Offline-First Staff Operations

[![Status: Demo](https://img.shields.io/badge/Status-Demo-gold)](#)
[![Stack: React Native](https://img.shields.io/badge/Stack-React%20Native%20%7C%20Expo-black)](#)
[![Sync: WebSocket](https://img.shields.io/badge/Sync-WebSocket%20%7C%20Offline--First-blue)](#)

## 🧹 Project Overview
A specialized staff tool designed for room assignment and real-time task tracking. Unlike guest-facing apps, this was built for high-pressure, low-connectivity environments where operational speed is the primary metric.

### Key Features:
*   **Real-Time Sync:** WebSocket-driven status updates between floor staff and the front desk.
*   **Offline-First Architecture:** Utilizes a local SQLite store to ensure staff can work in WiFi dead zones (stairwells, back-of-house) without data loss.
*   **Operational UX:** Designed for distracted, one-handed use with high-contrast displays and minimal navigation depth.

## ⚙️ Engineering Decisions
*   **Conflict Resolution:** Implemented a last-write-wins policy with a conflict log to manage simultaneous offline updates to room statuses.
*   **Sync Layer:** A queue-and-replay system that syncs local mutations back to the server as soon as connectivity returns.

## 🛠️ Technical Stack
*   **Mobile:** React Native & Expo
*   **Local Storage:** `expo-sqlite`
*   **Real-Time:** WebSockets

---
**Role:** Software Engineer & AI Strategist | [michellevision.com](https://michellevision.com)
