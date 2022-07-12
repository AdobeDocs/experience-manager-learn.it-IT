---
title: Utilizzo di contenuti localizzati con AEM Headless
description: Scopri come utilizzare GraphQL per eseguire query AEM per contenuti localizzati.
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
kt: 10254
thumbnail: KT-10254.jpeg
source-git-commit: 68970493802c7194bcb3ac3ac9ee10dbfb0fc55d
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 2%

---


# Contenuto localizzato con AEM headless

AEM fornisce un [Framework di integrazione della traduzione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html) per contenuti headless, che consentono di tradurre facilmente Frammenti di contenuto e risorse di supporto per le diverse impostazioni internazionali. Si tratta dello stesso framework utilizzato per tradurre altri contenuti AEM come pagine, frammenti esperienza, risorse e Forms. Una volta [contenuti headless tradotti](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html?lang=it), e pubblicato, è pronto per il consumo da applicazioni headless.

## Struttura delle cartelle di risorse{#assets-folder-structure}

Assicurati che i frammenti di contenuto localizzati in AEM seguano il [struttura di localizzazione consigliata](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure).

![Cartelle AEM risorse localizzate](./assets/localized-content/asset-folders.jpg)

Le cartelle internazionali devono essere di pari livello e il nome della cartella, anziché il titolo, deve essere valido [Codice ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) rappresenta le impostazioni internazionali del contenuto contenuto della cartella.

Il codice internazionale è anche il valore utilizzato per filtrare i frammenti di contenuto restituiti dalla query GraphQL.

| Codice locale | AEM | Impostazioni del contenuto |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | Contenuto tedesco |
| en | /content/dam/.../**en**/... | Contenuto in inglese |
| es | /content/dam/.../**es**/... | Contenuto spagnolo |

## Query persistente GraphQL

AEM fornisce un `_locale` Filtro GraphQL che filtra automaticamente il contenuto in base al codice delle impostazioni internazionali . Ad esempio, se si esegue una query su tutte le avventure in inglese nel [Progetto demo di riferimento WKND](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/create-site.html) può essere eseguito con una nuova query persistente `wknd-shared/adventures-by-locale` definito come:

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

La `$locale` nella variabile `_locale` Il filtro richiede il codice delle impostazioni internazionali (ad esempio `en`, `en_us`oppure `de`) come specificato in [AEM convenzione di localizzazione basata su cartelle](#assets-folder-structure).

## React example

Creiamo una semplice applicazione React che controlli il contenuto Adventure su cui eseguire la query da AEM in base a un selettore delle impostazioni locali utilizzando `_locale` filtro.

Quando __Inglese__ è selezionato nel selettore delle impostazioni internazionali, quindi Frammenti di contenuto di avventura inglese in `/content/dam/wknd/en` sono restituiti quando __Spagnolo__ è selezionato, quindi Frammenti di contenuto spagnolo in `/content/dam/wknd/es`e così via.

![App di esempio React localizzata](./assets/localized-content/react-example.png)

### Crea un `LocaleContext`{#locale-context}

Innanzitutto, crea un [Reagire nel contesto](https://reactjs.org/docs/context.html) per consentire l’utilizzo delle impostazioni internazionali nei componenti dell’applicazione React.

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

### Crea un `LocaleSwitcher` Componente React{#locale-switcher}

Quindi, crea un componente Commutatore impostazioni internazionali React che imposta come [LocaleContext](#locale-context) nella selezione dell&#39;utente.

Questo valore internazionale viene utilizzato per indirizzare le query GraphQL, garantendo che restituiscano solo il contenuto corrispondente alle impostazioni internazionali selezionate.

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

### Invia una query al contenuto utilizzando `_locale` filter{#adventures}

Il componente Avventures richiede AEM per tutte le avventure in base alle impostazioni internazionali ed elenca i relativi titoli. A tal fine, passa alla query il valore delle impostazioni internazionali memorizzato nel contesto React utilizzando `_locale` filtro.

Questo approccio può essere esteso ad altre query nell&#39;applicazione, garantendo che tutte le query includano solo il contenuto specificato dalla selezione delle impostazioni internazionali di un utente.

La query su AEM viene eseguita nel gancio React personalizzato [getAdventuresByLocale, descritto più dettagliatamente nella documentazione Query AEM GraphQL](./aem-headless-sdk.md).

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

### Definisci la `App.js`{#app-js}

Infine, legalo tutto avvolgendo l&#39;applicazione React in con il `LanguageContext.Provider` e l&#39;impostazione del valore delle impostazioni internazionali. Ciò consente agli altri componenti React, [LocaleSwitcher](#locale-switcher)e [Avventure](#adventures) per condividere lo stato di selezione delle impostazioni internazionali.

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
