# System Patterns

## Архітектура на рівні репозиторію

- Монорепо з Yarn workspaces:
  - `excalidraw-app` — standalone веб-застосунок;
  - `packages/excalidraw` — публічна React-бібліотека;
  - `packages/common|element|math|utils` — внутрішні модулі.
- Патерн: `app` збирається поверх бібліотечних пакетів через alias-и, а не через зібрані dist-артефакти під час розробки.

## Патерн модульності та залежностей

- Логічна декомпозиція:
  - `common`: shared constants/утиліти;
  - `math`: геометричні обчислення;
  - `element`: домен елементів сцени;
  - `excalidraw`: UI, редактор, API компонента.
- Dependency direction переважно з високого рівня до нижчого:
  - `excalidraw` -> `element/common/math`;
  - `element` -> `common/math`;
  - `math` -> `common`.

## Патерн композиції UI в app

- Точка входу:
  - `index.tsx` реєструє SW та рендерить `ExcalidrawApp`.
- Компонент `App.tsx` виконує orchestration:
  - провайдери (`Provider`, `ExcalidrawAPIProvider`, error boundary);
  - ініціалізація сцени;
  - підключення колаборації;
  - підключення меню/палет/діалогів/AI-компонентів.
- Патерн "library + host app":
  - `@excalidraw/excalidraw` надає ядро редактора;
  - `excalidraw-app` додає продуктову поведінку й інтеграції.

## Патерн стану та персистентності

- Локальний стан:
  - Jotai-атоми для глобальних UI/collab сигналів.
- Локальне збереження:
  - сцена й appState у `localStorage`;
  - файли та library в IndexedDB.
- Контроль консистентності:
  - debounce/throttle для save/sync;
  - version-keys (`version-dataState`, `version-files`) для синхронізації між вкладками;
  - pause/resume save lock під час колаборації.

## Патерн realtime-колаборації

- `Collab` (контролер) + `Portal` (transport-adapter) + data-layer (`data/*`).
- WebSocket канал через `socket.io-client`.
- Payload-и шифруються (encrypt/decrypt) перед передачею.
- Sync стратегія:
  - інкрементальні апдейти елементів;
  - періодичний full scene sync;
  - окремий потік для статусів курсора/idle/visible bounds.
- Медіа/зображення:
  - файли зберігаються/читаються через Firebase storage;
  - статуси файлів керуються окремим `FileStatusStore`.

## Патерн імпорту/експорту

- Share-link backend:
  - дані сцени стискаються + шифруються;
  - ключ шифрування залишається у hash-фрагменті URL (`#json=id,key`).
- Підтримано кілька джерел ініціалізації:
  - local state;
  - `#json` backend snapshot;
  - `#url` зовнішній файл;
  - `#room` live collaboration.

## Патерн білду та оточень

- Vite для app із alias-резолвом на workspace source.
- Esbuild-скрипти для пакетів (`buildBase.js`, `buildPackage.js`, `buildUtils.js`) генерують dev/prod ESM.
- Env-driven integration: різні `.env` для dev/prod endpoint-ів.
- PWA/runtime caching і manual chunking для locales/CodeMirror/mermaid.

## Ключові технічні рішення

- **Provider-first integration**: API редактора доступний через контекст і hooks.
- **Edge-safe persistence**: flush на blur/unload + попередження про незбережені дані.
- **Bandwidth-aware collab**: передаються syncable елементи, версії трекаються.
- **Separation of concerns**: UI-оркестрація в app, редакторне ядро в library package.

## Перевірено по source code

- Монорепо, workspaces, dependency контури: `package.json`, `packages/*/package.json`.
- Експорт React-компонента та API/хуків: `packages/excalidraw/index.tsx`, `packages/excalidraw/README.md`.
- App orchestration і ініціалізація сцени: `excalidraw-app/App.tsx`, `excalidraw-app/index.tsx`.
- Колаборація, transport, шифрування, sync: `excalidraw-app/collab/Collab.tsx`, `excalidraw-app/collab/Portal.tsx`, `excalidraw-app/data/index.ts`.
- Локальна персистентність та tab-sync: `excalidraw-app/data/LocalData.ts`, `excalidraw-app/app_constants.ts`.
- Build/pwa/chunking/aliases: `excalidraw-app/vite.config.mts`, `scripts/buildPackage.js`.
