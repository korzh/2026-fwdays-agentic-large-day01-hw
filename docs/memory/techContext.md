# Tech Context

## Runtime та інструменти

- Node.js: `>=18.0.0` (engines у root та `excalidraw-app`).
- Yarn: `1.22.22` (через `packageManager` у root).
- TypeScript: `5.9.3`.
- CI середовище: Node `20.x` (GitHub Actions).

## Основний стек

- **Frontend**: React `19.0.0`, ReactDOM `19.0.0`.
- **Build/dev server**: Vite `5.0.12`, `@vitejs/plugin-react`.
- **Styles**: SCSS/Sass (`sass`, esbuild sass plugin у build-скриптах).
- **State management**: Jotai (`jotai`, локальний `app-jotai` store).
- **Realtime**: `socket.io-client` для live-колаборації.
- **Storage**:
  - браузерне `localStorage` для scene/app state;
  - IndexedDB (`idb-keyval`) для файлів та бібліотеки.
- **Cloud/storage integration**: Firebase SDK (`firebase`) + кастомні data-адаптери.
- **PWA**: `vite-plugin-pwa` + `virtual:pwa-register`.
- **Observability**: `@sentry/browser`.

## Тестування та якість коду

- **Unit/integration tests**: Vitest (`vitest`, `jsdom`, `setupTests.ts`).
- **Coverage**: V8 coverage (`@vitest/coverage-v8`) + пороги в `vitest.config.mts`.
- **Lint/format/typecheck**:
  - ESLint (`yarn test:code`);
  - Prettier (`yarn test:other`);
  - TypeScript (`yarn test:typecheck`).

## Ключові команди (root)

- **Розробка**: `yarn start` (делегує в `excalidraw-app start`).
- **Білд застосунку**: `yarn build` або `yarn build:app`.
- **Білд пакетів**: `yarn build:packages`.
- **Тести все разом**: `yarn test:all`.
- **Тільки app-тести**: `yarn test:app`.
- **Локальне виправлення**: `yarn fix` (`prettier` + `eslint --fix`).
- **Docker build для app**: `yarn build:app:docker`.

## Важливі env-конфіги

- `VITE_APP_WS_SERVER_URL` — endpoint для WebSocket колаборації.
- `VITE_APP_BACKEND_V2_GET_URL` / `VITE_APP_BACKEND_V2_POST_URL` — backend для share-link JSON.
- `VITE_APP_FIREBASE_CONFIG` — Firebase конфіг.
- `VITE_APP_AI_BACKEND` — AI backend endpoint.
- `VITE_APP_PORT` — порт dev server.
- `VITE_APP_ENABLE_PWA` — увімкнення PWA у dev.

## Деплой/інфраструктура

- Docker multi-stage:
  - build stage: `node:18`, виконує `yarn build:app:docker`;
  - runtime stage: `nginx:1.27-alpine`, віддає статичний `excalidraw-app/build`.
- `docker-compose.yml` мапить порт `3000:80` і монтує workspace для dev-сценарію.

## Перевірено по source code

- Версії runtime/tooling/scripts: `package.json`, `excalidraw-app/package.json`.
- Vite/plugins/PWA/chunking: `excalidraw-app/vite.config.mts`.
- Vitest конфіг і coverage thresholds: `vitest.config.mts`.
- Локальний реєстр SW: `excalidraw-app/index.tsx`.
- Realtime/state/storage залежності: `excalidraw-app/package.json`, `excalidraw-app/data/LocalData.ts`, `excalidraw-app/collab/Collab.tsx`.
- CI Node 20.x + lint/test steps: `.github/workflows/test.yml`, `.github/workflows/lint.yml`.
- Docker runtime: `Dockerfile`, `docker-compose.yml`.
