# CLAUDE.md

## Stack
- **Language**: JavaScript (ES2020+, JSX)
- **Framework**: React 19 (via Create React App `react-scripts` 5.0.1)
- **Key libs**: react, react-dom, web-vitals, @testing-library/react + jest-dom + user-event
- **Package manager**: npm (uses `package-lock.json`; never hand-edit lockfiles)
- **Node**: >= 16 (verified against v25)

## Structure
```
public/            Static assets served as-is (index.html, icons, manifest)
src/               All application code
  Components/
    Assets/        Static PNG icons (person, email, password)
    LoginSignUp/   Login/Sign-Up component + its CSS
  App.js           Root component; renders <LoginSignUp/>
  App.css          Root stylesheet
  index.js         React entry point (ReactDOM.createRoot)
  App.test.js      Component smoke test
  reportWebVitals.js  Web vitals reporting (boilerplate)
```

## Conventions
- Components: PascalCase `.jsx` files in `src/Components/<ComponentName>/`, colocated `.css` (plain CSS, no modules/tailwind)
- Component-local styles imported inside the component (`import './LoginSignUp.css'`)
- State: `useState` only; no router, no state library, no backend
- Error handling / logging: none in place — this is a static UI; no fetch, no forms submitted
- Scripts: `npm start`, `npm test`, `npm run build` (all via react-scripts)

## Known gotchas
- **Dependency tree is legacy CRA 5.0.1**: `react-scripts` pins stale transitive deps (workbox, svgo, glob, eslint 8, etc.). `npm audit` reports ~58 findings that are build-time only and NOT fixable without a breaking `react-scripts` upgrade or eject. Do not run `npm audit fix --force`.
- **fast-uri**: was vulnerable (CVE-2026-6321); pinned to `3.1.5` in the lockfile via `npm update fast-uri`. `ajv` declares `^3.0.1` so re-running `npm install` from the lockfile keeps the fixed version. If the lockfile is regenerated from scratch, verify `fast-uri >= 3.1.1`.
- **No backend/auth logic exists**: the "Login/Sign Up" toggle is purely cosmetic (swaps `action` state); inputs are uncontrolled and do nothing.
- CRA serves the build assuming host root (`homepage` not set); deploy behind a subpath only after setting `homepage` in package.json.

## Module map
| Concern | File |
|---|---|
| App shell / composition | `src/App.js` |
| Login vs Sign-Up UI + toggle state | `src/Components/LoginSignUp/LoginSignUp.jsx` |
| Component styles | `src/Components/LoginSignUp/LoginSignUp.css`, `src/App.css` |
| Static icons | `src/Components/Assets/*.png` |
| Entry point | `src/index.js` |
| Tests | `src/App.test.js` (react-scripts test runner) |
