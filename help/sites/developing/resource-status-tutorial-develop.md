---
title: Sviluppo di stati delle risorse in  AEM Sites
description: 'Le API Stato risorsa di Adobe Experience Manager, è un framework plug-in per l''esposizione dei messaggi di stato in AEM diverse interfacce utente Web dell''editor. '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 03db12de4d95ced8fabf36b8dc328581ec7a2749
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# Sviluppo degli stati delle risorse {#developing-resource-statuses-in-aem-sites}

Le API Stato risorsa di Adobe Experience Manager, è un framework plug-in per l&#39;esposizione dei messaggi di stato in AEM diverse interfacce utente Web dell&#39;editor.

## Panoramica {#overview}

Il framework Stato risorsa per editor fornisce API lato server e lato client per la visualizzazione e l&#39;interazione con gli stati dell&#39;editor, in modo standard e uniforme.

Le barre di stato dell&#39;editor sono disponibili in modo nativo negli editor di pagine, frammenti esperienza e modelli di AEM.

Esempi di utilizzo per i provider di stato delle risorse personalizzati:

* Notifica agli autori quando una pagina si trova entro 2 ore dall’attivazione pianificata
* Notificare agli autori che una pagina è stata attivata negli ultimi 15 minuti
* Notificare agli autori che una pagina è stata modificata negli ultimi 5 minuti e specificare l’autore

![Panoramica sullo stato delle risorse dell’editor AEM](assets/sample-editor-resource-status-screenshot.png)

## Framework provider stato risorsa {#resource-status-provider-framework}

Durante lo sviluppo di stati di risorse personalizzati, il lavoro di sviluppo è composto da:

1. L&#39;implementazione ResourceStatusProvider, responsabile per determinare se uno stato è richiesto, e le informazioni di base sullo stato: titolo, messaggio, priorità, variante, icona e azioni disponibili.
2. Facoltativamente, JavaScript GraniteUI che implementa la funzionalità di tutte le azioni disponibili.

   ![architettura dello stato delle risorse](assets/sample-editor-resource-status-application-architecture.png)

3. Alla risorsa di stato fornita come parte degli editor di pagine, frammenti esperienza e modelli viene assegnato un tipo tramite la proprietà delle risorse &quot;[!DNL statusType]&quot;.

   * Editor pagina: `editor`
   * Editor frammento esperienza: `editor`
   * Editor modelli: `template-editor`

4. La `statusType` della risorsa di stato corrisponde alla proprietà `CompositeStatusType` OSGi configurata `name` registrata.

   Per tutte le corrispondenze, i tipi `CompositeStatusType's` vengono raccolti e utilizzati per raccogliere le implementazioni `ResourceStatusProvider` con questo tipo, tramite `ResourceStatusProvider.getType()`.

5. La `ResourceStatusProvider` corrispondente viene passata la `resource` nell&#39;editor e determina se la `resource` ha lo stato da visualizzare. Se lo stato è necessario, questa implementazione è responsabile della generazione di 0 o molti `ResourceStatuses` da restituire, ognuno dei quali rappresenta uno stato da visualizzare.

   In genere, un `ResourceStatusProvider` restituisce 0 o 1 `ResourceStatus` per `resource`.

6. ResourceStatus è un&#39;interfaccia che può essere implementata dal cliente, oppure l&#39;utile `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` può essere utilizzato per creare uno stato. Uno stato è composto da:

   * Titolo
   * Messaggio
   * Icona
   * Variante
   * Priorità
   * Azioni
   * Dati

7. Facoltativamente, se per l&#39;oggetto `ResourceStatus` sono forniti `Actions`, i clientlibs di supporto devono associare la funzionalità ai collegamenti dell&#39;azione nella barra di stato.

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. Qualsiasi codice JavaScript o CSS di supporto per le azioni deve essere proxy attraverso le rispettive librerie client di ciascun editor per garantire che il codice front-end sia disponibile nell&#39;editor.

   * Categoria editor pagina: `cq.authoring.editor.sites.page`
   * Categoria editor di frammenti esperienza: `cq.authoring.editor.sites.page`
   * Categoria editor modelli: `cq.authoring.editor.sites.template`

## Visualizzare il codice {#view-the-code}

[Vedi il codice su GitHub](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## Risorse aggiuntive {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
