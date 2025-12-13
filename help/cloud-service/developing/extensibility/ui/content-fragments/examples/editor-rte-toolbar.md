---
title: Pulsante personalizzato sulla barra degli strumenti dell’Editor Rich Text
description: Scopri come aggiungere un pulsante personalizzato alla barra degli strumenti Editor Rich Text (RTE) nell’Editor frammenti di contenuto di AEM
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13464
thumbnail: KT-13464.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 6fd93d3b-6d56-43c5-86e6-2e2685deecc9
duration: 345
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# Pulsante personalizzato sulla barra degli strumenti dell’Editor Rich Text

Scopri come aggiungere un pulsante personalizzato alla barra degli strumenti Editor Rich Text (RTE) nell’Editor frammenti di contenuto di AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3420768?quality=12&learn=on)

I pulsanti personalizzati possono essere aggiunti alla barra degli strumenti **Editor Rich Text** nell&#39;Editor frammenti di contenuto utilizzando il punto di estensione `rte`. In questo esempio viene illustrato come aggiungere un pulsante personalizzato denominato _Aggiungi suggerimento_ alla barra degli strumenti dell&#39;editor Rich Text e modificare il contenuto dell&#39;editor Rich Text.

Utilizzando il metodo `rte` del punto di estensione di `getCustomButtons()` è possibile aggiungere uno o più pulsanti personalizzati alla barra degli strumenti **Editor Rich Text**. È inoltre possibile aggiungere o rimuovere pulsanti standard dell&#39;editor Rich Text come _Copia, Incolla, Grassetto e Corsivo_ utilizzando i metodi `getCoreButtons()` e `removeButtons)` rispettivamente.

In questo esempio viene illustrato come inserire una nota o un suggerimento evidenziato utilizzando il pulsante personalizzato _Aggiungi suggerimento_ della barra degli strumenti. Il contenuto della nota o del suggerimento evidenziato presenta una formattazione speciale applicata tramite elementi HTML e le classi CSS associate. Il contenuto segnaposto e il codice HTML vengono inseriti utilizzando il metodo di callback `onClick()` di `getCustomButtons()`.

## Punto di estensione

Questo esempio si estende al punto di estensione `rte` per aggiungere un pulsante personalizzato alla barra degli strumenti dell’editor Rich Text nell’Editor frammento di contenuto.

| Interfaccia utente di AEM estesa | Punto di estensione |
| ------------------------ | --------------------- |
| [Editor frammento di contenuto](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Barra degli strumenti Editor Rich Text](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/) |

## Estensione di esempio

Nell&#39;esempio seguente viene creato un pulsante personalizzato _Aggiungi suggerimento_ nella barra degli strumenti dell&#39;editor Rich Text. L’azione clic inserisce il testo segnaposto nella posizione corrente del punto di inserimento nell’editor Rich Text.

Il codice mostra come aggiungere il pulsante personalizzato con un’icona e registrare la funzione di gestione dei clic.

### Registrazione dell’estensione

`ExtensionRegistration.js`, mappato alla route index.html, è il punto di ingresso per l&#39;estensione AEM e definisce:

+ Definizione del pulsante della barra degli strumenti dell&#39;editor Rich Text nella funzione `getCustomButtons()` con attributi `id, tooltip and icon`.
+ Gestore di clic per il pulsante, nella funzione `onClick()`.
+ La funzione del gestore di clic riceve l&#39;oggetto `state` come argomento per ottenere il contenuto dell&#39;editor Rich Text in formato HTML o testo. Tuttavia, in questo esempio non viene utilizzato.
+ La funzione del gestore di clic restituisce un array di istruzioni. Questo array ha un oggetto con `type` e `value` attributi. Per inserire il contenuto, lo snippet di codice HTML degli attributi `value`, l&#39;attributo `type` utilizza `insertContent`. Se esiste un caso d&#39;uso per sostituire il contenuto, il caso d&#39;uso è il tipo di istruzione `replaceContent`.

Il valore `insertContent` è una stringa HTML, `<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>`. Le classi CSS `cmp-contentfragment__element-tip` utilizzate per visualizzare il valore non sono definite nel widget, ma implementate nell&#39;esperienza Web in cui viene visualizzato il campo Frammento di contenuto.


`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import React from "react";
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

// This function is called when the extension is registered with the host and runs in an iframe in the Content Fragment Editor browser window.
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        rte: {
          // RTE Toolbar custom button
          getCustomButtons: () => [
            {
              id: "wknd-cf-tip", // Provide a unique ID for the custom button
              tooltip: "Add Tip", // Provide a label for the custom button
              icon: "Note", // Provide an icon for the button (see https://spectrum.adobe.com/page/icons/ for a list of available icons)
              onClick: (state) => {
                // Provide a click handler function that returns the instructions array with type and value. This example inserts the HTML snippet for TIP content.
                return [
                  {
                    type: "insertContent",
                    value:
                      '<div class="cmp-contentfragment__element-tip"><div>TIP</div><div>Add your tip text here...</div></div>',
                  },
                ];
              },
            },
          ],
        },
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```
