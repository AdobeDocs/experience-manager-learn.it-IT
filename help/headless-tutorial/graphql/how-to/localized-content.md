---
title: Utilizzo di contenuti localizzati con AEM headless
description: Scopri come utilizzare GraphQL per eseguire query su AEM per contenuti localizzati.
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
jira: KT-10254
thumbnail: KT-10254.jpeg
exl-id: 5e3d115b-f3a1-4edc-86ab-3e0713a36d54
duration: 192
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 0%

---

# Contenuto localizzato con AEM headless

L&#39;AEM fornisce [Framework integrazione traduzione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html) per contenuti headless, che consentono di tradurre facilmente frammenti di contenuto e risorse di supporto per l’utilizzo in più lingue. Si tratta dello stesso framework utilizzato per tradurre altri contenuti AEM, come pagine, frammenti di esperienza, risorse e Forms. Una volta [il contenuto headless è stato tradotto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html?lang=it), e pubblicato, è pronto per essere utilizzato dalle applicazioni headless.

## Struttura delle cartelle di Assets{#assets-folder-structure}

Assicurati che i Frammenti di contenuto localizzati nell’AEM seguano il [struttura di localizzazione consigliata](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure).

![Cartelle di risorse AEM localizzate](./assets/localized-content/asset-folders.jpg)

Le cartelle delle impostazioni locali devono essere di pari livello e il nome della cartella, anziché il titolo, deve essere valido [Codice ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) che rappresenta le impostazioni locali del contenuto contenuto della cartella.

Il codice locale è anche il valore utilizzato per filtrare i Frammenti di contenuto restituiti dalla query GraphQL.

| Codice lingua | Percorso AEM | Impostazioni locali contenuto |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | Contenuto tedesco |
| en | /content/dam/.../**it**/... | Contenuto inglese |
| es | /content/dam/.../**es**/... | Contenuto spagnolo |

## Query persistente GraphQL

L&#39;AEM fornisce `_locale` Filtro di GraphQL che filtra automaticamente il contenuto in base al codice locale. Ad esempio, eseguendo una query su tutte le avventure in inglese nel [Progetto sito WKND](https://github.com/adobe/aem-guides-wknd) può essere eseguito con una nuova query persistente `wknd-shared/adventures-by-locale` definito come:

```graphql
query($locale: String!) {
  adventureList(_locale: $locale) {
    items {      
      _path
      title
    }
  }
}
```

Il `$locale` variabile utilizzata nel `_locale` il filtro richiede il codice locale (ad esempio `en`, `en_us`, o `de`) come specificato in [Convenzione di localizzazione basata su cartelle di risorse AEM](#assets-folder-structure).

## Esempio di React

Creiamo una semplice applicazione React che controlla quali contenuti Adventure eseguire query dall’AEM in base a un selettore delle impostazioni internazionali utilizzando `_locale` filtro.

Quando __Inglese__ è selezionato nel selettore delle impostazioni internazionali, quindi Frammenti di contenuto di Adventure in inglese in `/content/dam/wknd/en` vengono restituiti, quando __Spagnolo__ è selezionato, quindi Frammenti di contenuto spagnolo in `/content/dam/wknd/es`e così via.

![App di esempio React localizzato](./assets/localized-content/react-example.png)

### Creare un `LocaleContext`{#locale-context}

Innanzitutto, crea un [Contesto di React](https://reactjs.org/docs/context.html) per consentire l&#39;utilizzo delle impostazioni locali nei componenti dell&#39;applicazione React.

```javascript
// src/LocaleContext.js

import React from 'react'

const DEFAULT_LOCALE = 'en';

const LocaleContext = React.createContext({
    locale: DEFAULT_LOCALE, 
    setLocale: () => {}
});

export default LocaleContext;
```

### Creare un `LocaleSwitcher` Componente React{#locale-switcher}

Quindi, crea un componente React per il commutatore delle impostazioni internazionali che imposti [LocaleContext](#locale-context) alla selezione dell&#39;utente.

Questo valore delle impostazioni locali viene utilizzato per indirizzare le query GraphQL e garantisce che restituiscano solo il contenuto corrispondente alle impostazioni locali selezionate.

```javascript
// src/LocaleSwitcher.js

import { useContext } from "react";
import LocaleContext from "./LocaleContext";

export default function LocaleSwitcher() {
  const { locale, setLocale } = useContext(LocaleContext);

  return (
    <select value={locale}
            onChange={e => setLocale(e.target.value)}>
      <option value="de">Deutsch</option>
      <option value="en">English</option>
      <option value="es">Español</option>
    </select>
  );
}
```

### Eseguire query sul contenuto utilizzando `_locale` filter{#adventures}

Il componente Avventure richiede all’AEM tutte le avventure per lingua ed elenca i relativi titoli. Ciò si ottiene passando il valore locale memorizzato nel contesto React alla query utilizzando `_locale` filtro.

Questo approccio può essere esteso ad altre query nell’applicazione, garantendo che tutte le query includano solo il contenuto specificato dalla selezione locale di un utente.

L’interrogazione contro l’AEM viene eseguita nel gancio React personalizzato [getAdventuresByLocale, descritto più dettagliatamente nella sezione Query della documentazione di AEM GraphQL](./aem-headless-sdk.md).

```javascript
// src/Adventures.js

import { useContext } from "react"
import { useAdventuresByLocale } from './api/persistedQueries'
import LocaleContext from './LocaleContext'

export default function Adventures() {
    const { locale } = useContext(LocaleContext);

    // Get data from AEM using GraphQL persisted query as defined above 
    // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
    let { data, error } = useAdventuresByLocale(locale);

    return (
        <ul>
            {data?.adventureList?.items?.map((adventure, index) => { 
                return <li key={index}>{adventure.title}</li>
            })}
        </ul>
    )
}
```

### Definisci il `App.js`{#app-js}

Infine, legare il tutto racchiudendo l’applicazione React con la `LanguageContext.Provider` e l&#39;impostazione del valore locale. Ciò consente agli altri componenti React di: [LocaleSwitcher](#locale-switcher), e [Avventure](#adventures) per condividere lo stato di selezione delle impostazioni internazionali.

```javascript
// src/App.js

import { useState, useContext } from "react";
import LocaleContext from "./LocaleContext";
import LocaleSwitcher from "./LocaleSwitcher";
import Adventures from "./Adventures";

export default function App() {
  const [locale, setLocale] = useState(useContext(LocaleContext).locale);

  return (
    <LocaleContext.Provider value={{locale, setLocale}}>
      <LocaleSwitcher />
      <Adventures />
    </LocaleContext.Provider>
  );
}
```
