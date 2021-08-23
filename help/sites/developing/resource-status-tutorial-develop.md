---
title: Sviluppo di stati delle risorse in AEM Sites
description: 'API di stato delle risorse di Adobe Experience Manager, è un framework pluggable per l''esposizione dei messaggi di stato in AEM diverse interfacce web dell''editor. '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
source-git-commit: 03db12de4d95ced8fabf36b8dc328581ec7a2749
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# Sviluppo degli stati delle risorse {#developing-resource-statuses-in-aem-sites}

API di stato delle risorse di Adobe Experience Manager, è un framework pluggable per l&#39;esposizione dei messaggi di stato in AEM diverse interfacce web dell&#39;editor.

## Panoramica {#overview}

Il framework Stato risorsa per editor fornisce API lato server e lato client per la visualizzazione e l&#39;interazione con gli stati dell&#39;editor, in modo standard e uniforme.

Le barre di stato dell’editor sono disponibili in modo nativo negli editor Pagina, Frammento esperienza e Modelli di AEM.

Esempi di casi di utilizzo per provider di stato delle risorse personalizzati:

* Notifica agli autori di una pagina entro 2 ore dall’attivazione pianificata
* Notifica agli autori dell’attivazione di una pagina negli ultimi 15 minuti
* Notifica agli autori che una pagina è stata modificata negli ultimi 5 minuti e da chi

![Panoramica sullo stato delle risorse dell’editor AEM](assets/sample-editor-resource-status-screenshot.png)

## Framework del provider dello stato della risorsa {#resource-status-provider-framework}

Durante lo sviluppo di stati delle risorse personalizzati, il lavoro di sviluppo è composto da:

1. Implementazione ResourceStatusProvider, responsabile per determinare se è necessario uno stato, e informazioni di base sullo stato: titolo, messaggio, priorità, variante, icona e azioni disponibili.
2. Facoltativamente, JavaScript GraniteUI che implementa la funzionalità di eventuali azioni disponibili.

   ![architettura dello stato delle risorse](assets/sample-editor-resource-status-application-architecture.png)

3. Alla risorsa di stato fornita come parte degli editor di pagine, frammenti esperienza e modelli viene assegnato un tipo tramite la proprietà &quot;[!DNL statusType]&quot; delle risorse.

   * Editor pagina: `editor`
   * Editor frammento esperienza: `editor`
   * Editor modelli: `template-editor`

4. La proprietà `statusType` della risorsa di stato corrisponde alla proprietà `CompositeStatusType` OSGi registrata configurata `name`.

   Per tutte le corrispondenze, i tipi `CompositeStatusType's` vengono raccolti e utilizzati per raccogliere le implementazioni `ResourceStatusProvider` con questo tipo, tramite `ResourceStatusProvider.getType()`.

5. La `ResourceStatusProvider` corrispondente viene passata alla `resource` nell&#39;editor e determina se la `resource` ha lo stato da visualizzare. Se è necessario lo stato, questa implementazione è responsabile della generazione di 0 o di molti `ResourceStatuses` da restituire, ognuno dei quali rappresenta uno stato da visualizzare.

   In genere, un `ResourceStatusProvider` restituisce 0 o 1 `ResourceStatus` per `resource`.

6. ResourceStatus è un&#39;interfaccia che può essere implementata dal cliente, oppure l&#39;utile `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` può essere utilizzato per creare uno stato. Uno stato è composto da:

   * Titolo
   * Messaggio
   * Icona
   * Variante
   * Priorità
   * Azioni
   * Dati

7. Facoltativamente, se per l&#39;oggetto `ResourceStatus` sono forniti `Actions` , sono necessarie clientlibs di supporto per associare funzionalità ai collegamenti azione nella barra di stato.

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. Qualsiasi JavaScript o CSS di supporto per le azioni, deve essere sottoposto a proxy tramite le rispettive librerie client di ogni editor per garantire che il codice front-end sia disponibile nell&#39;editor.

   * Categoria editor di pagine: `cq.authoring.editor.sites.page`
   * Categoria dell’editor dei frammenti esperienza: `cq.authoring.editor.sites.page`
   * Categoria editor modelli: `cq.authoring.editor.sites.template`

## Visualizza il codice {#view-the-code}

[Vedi il codice su GitHub](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## Risorse aggiuntive {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
