---
layout:
  width: default
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# Internacionalitzacio (i18n)

## Internacionalització (i18n)

### Introducció

* Aprendre a internacionalitzar una aplicació React utilitzant i18next amb Vite.
* Beneficis de i18n:
  * Adaptar la teva app a diversos idiomes.
  * Millorar l’accessibilitat i l’abast global.
  * Separar el codi de les traduccions per a manteniment fàcil.

### Afegir dependències

```bash
npm install i18next react-i18next i18next-browser-languagedetector
```

### Estructura de fitxers

```
src/
 ├─ App.jsx
 ├─ main.jsx
 ├─ i18n.js
 └─ locales/
     ├─ en.json
     └─ ca.json
```

* **locales/**: Conté les traduccions per idioma.
* **i18n.js**: Configuració d’i18next.
* **App.jsx**: Components React que utilitzen les traduccions.

## Fitxers de traducció

```javascript
en.json
{
    "welcome": "Welcome to React + Vite",
    "login": "Log in",
    "logout": "Log out",
    "user": {
        "name": "User name"
    }
}
```

```javascript
ca.json
{
    "welcome": "Benvingut a React + Vite",
    "login": "Inicia sessió",
    "logout": "Tanca sessió",
    "user": {
        "name": "Nom d'usuari"
    }
}
```

## Configuració d’i18next

```jsx
// src/i18n.js

import i18n from "i18next";
import { initReactI18next } from "react-i18next";
import LanguageDetector from "i18next-browser-languagedetector";
import enJSON from "./locales/en.json";
import caJSON from "./locales/ca.json";

i18n
  .use(LanguageDetector)
  .use(initReactI18next)
  .init({
    resources: {
      en: { translation: enJSON },
      ca: { translation: caJSON }
    },
    fallbackLng: "en",
    debug: import.meta.env.DEV,
    interpolation: { escapeValue: false }
  });

export default i18n;
```

## Modificació main.jsx

```jsx
import './i18n.js' // Important
```

## Ús a components React

```jsx
// src/App.jsx

import { useTranslation } from "react-i18next";

export default function App() {
  const { t, i18n } = useTranslation();

  return (
    <div>
      <h1>{t("welcome")}</h1>

      <button onClick={() => i18n.changeLanguage("en")}>English</button>
      <button onClick={() => i18n.changeLanguage("ca")}>Català</button>
    </div>
  );
}
```

* `t("welcome")`: retorna la traducció segons l’idioma actual.
* `t("user.name")`: retorna la traducció segons l’idioma actual de la propietat `name` de `user`.
* `i18n.changeLanguage("ca")`: canvia l’idioma dinàmicament.

### Detectar idioma del navegador

* `i18next-browser-languagedetector` permet detectar automàticament l’idioma preferit de l’usuari.
* Si no està disponible, s’usa fallbackLng (en aquest cas, anglès).

### Com gestionar claus que falten

* Si una clau no existeix al JSON:
  * Es mostra la clau literal per defecte.
  * O es pot utilitzar fallbackLng per buscar en un idioma alternatiu.
* Opcionalment, activar `saveMissing: true` per detectar claus no definides a dev.

## Recomanacions

* Organitzar JSON per seccions: common, auth, navbar, etc.
* Crear fitxers JSON separats per idioma per mantenir el projecte net.
* Afegir un debug mode a dev per veure errors de traducció.
