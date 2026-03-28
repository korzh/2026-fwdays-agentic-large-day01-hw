# Project Brief

## Що це за проєкт

- Це монорепозиторій `excalidraw-monorepo` з воркспейсами для:
  - веб-застосунку `excalidraw-app`;
  - бібліотеки `@excalidraw/excalidraw`;
  - внутрішніх пакетів `@excalidraw/common`, `@excalidraw/element`, `@excalidraw/math`, `@excalidraw/utils`;
  - прикладів інтеграції (`examples/*`).
- Основний продукт: браузерний whiteboard Excalidraw + окремо embeddable React-компонент для інтеграції в інші застосунки.

## Основна мета

- Надати повнофункціональний онлайн-редактор діаграм/скетчів із:
  - локальним збереженням сцени;
  - шеринґом через лінк;
  - live-колаборацією;
  - експортом і інтеграційними API.
- Підтримувати дві моделі використання:
  - як standalone app (`excalidraw-app`);
  - як npm-бібліотека React-компонента (`@excalidraw/excalidraw`).

## Ключові сценарії користувача

- Малювання та редагування сцени в браузері.
- Відкриття/імпорт сцени з URL (`#json=...`, `#url=...`, `#room=...`).
- Спільне редагування в realtime-кімнатах із шифруванням payload-ів.
- Експорт сцени (у backend/share-link та в Excalidraw+).

## Що важливо для команди

- Монорепо централізує розробку app + reusable packages.
- Є чіткі npm-скрипти для build/test/lint/typecheck.
- CI перевіряє тести й якість коду на Node 20.x.
- Продакшн-доставку можна робити як статичний build (через Docker + nginx).

## Межі та non-goals (на рівні цього репо)

- Сервер WebSocket-колаборації не реалізований у цьому репо (лише клієнтська інтеграція через `VITE_APP_WS_SERVER_URL`).
- Зовнішні backend-сервіси (json API, Firebase storage, AI backend) підключаються через env-конфіг.

## Перевірено по source code

- Монорепо, воркспейси, скрипти, Node/Yarn: `package.json`.
- Роль пакета як React component: `packages/excalidraw/README.md`, `packages/excalidraw/package.json`.
- Точка входу app: `excalidraw-app/index.tsx`.
- Основний orchestration UI/app-flow: `excalidraw-app/App.tsx`.
- CI Node 20.x і тестовий pipeline: `.github/workflows/test.yml`, `.github/workflows/lint.yml`.
- Docker build + nginx runtime: `Dockerfile`, `docker-compose.yml`.
