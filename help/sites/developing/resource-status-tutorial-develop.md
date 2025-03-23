---
title: Sviluppo degli stati delle risorse in AEM Sites
description: Le API di Stato risorse di Adobe Experience Manager sono un framework collegabile per l’esposizione dei messaggi di stato nelle varie interfacce web dell’editor di AEM.
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
duration: 88
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 2%

---


# Sviluppo degli stati delle risorse {#developing-resource-statuses-in-aem-sites}

Le API di Stato risorse di Adobe Experience Manager sono un framework collegabile per l’esposizione dei messaggi di stato nelle varie interfacce web dell’editor di AEM.

## Panoramica {#overview}

Il framework Resource Status for Editors fornisce API lato server e lato client per visualizzare e interagire con gli stati dell&#39;editor in modo standard e uniforme.

Le barre di stato dell’editor sono disponibili in modo nativo negli editor di pagine, frammenti di esperienza e modelli di AEM.

Di seguito sono riportati alcuni esempi di casi d&#39;uso per i provider di stato delle risorse personalizzati:

* Avviso agli autori quando una pagina si trova entro 2 ore dall&#39;attivazione pianificata
* Avviso agli autori che una pagina è stata attivata negli ultimi 15 minuti
* Avviso agli autori della modifica di una pagina negli ultimi 5 minuti e indicazione di chi ha eseguito la modifica

![Panoramica sullo stato delle risorse dell&#39;editor di AEM](assets/sample-editor-resource-status-screenshot.png)

## Framework provider stato risorse {#resource-status-provider-framework}

Quando si sviluppano stati delle risorse personalizzati, il lavoro di sviluppo è composto da:

1. Implementazione di ResourceStatusProvider, responsabile di determinare se è necessario uno stato e informazioni di base sullo stato: titolo, messaggio, priorità, variante, icona e azioni disponibili.
2. Facoltativamente, GraniteUI JavaScript che implementa la funzionalità di tutte le azioni disponibili.

   ![architettura stato risorse](assets/sample-editor-resource-status-application-architecture.png)

3. Alla risorsa di stato fornita come parte degli editor di pagine, frammenti di esperienza e modelli viene assegnato un tipo tramite la proprietà &quot;[!DNL statusType]&quot; delle risorse.

   * Editor pagina: `editor`
   * Editor frammento esperienza: `editor`
   * Editor modelli: `template-editor`

4. La risorsa di stato `statusType` corrisponde alla proprietà `name` configurata dall&#39;OSGi `CompositeStatusType` registrata.

   Per tutte le corrispondenze, i tipi `CompositeStatusType's` vengono raccolti e utilizzati per raccogliere le implementazioni `ResourceStatusProvider` che hanno questo tipo, tramite `ResourceStatusProvider.getType()`.

5. `ResourceStatusProvider` corrispondente ha superato `resource` nell&#39;editor e determina se `resource` ha lo stato da visualizzare. Se lo stato è necessario, questa implementazione è responsabile della compilazione di 0 o molti `ResourceStatuses` da restituire, ciascuno dei quali rappresenta uno stato da visualizzare.

   In genere, un `ResourceStatusProvider` restituisce 0 o 1 `ResourceStatus` per `resource`.

6. ResourceStatus è un&#39;interfaccia che può essere implementata dal cliente oppure l&#39;utile `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` può essere utilizzato per creare uno stato. Uno stato è composto da:

   * Titolo
   * Messaggio
   * Icona
   * Variante
   * Priorità
   * Azioni
   * Dati

7. Se si forniscono `Actions` per l&#39;oggetto `ResourceStatus`, è possibile che per associare la funzionalità ai collegamenti delle azioni nella barra di stato siano necessarie clientlibs di supporto.

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. Qualsiasi JavaScript o CSS che supporti le azioni deve essere inviato come proxy attraverso le rispettive librerie client di ogni editor per garantire che il codice front-end sia disponibile nell’editor.

   * Categoria editor pagina: `cq.authoring.editor.sites.page`
   * Categoria editor frammento esperienza: `cq.authoring.editor.sites.page`
   * Categoria editor modelli: `cq.authoring.editor.sites.template`

## Visualizza il codice {#view-the-code}

[Visualizza il codice su GitHub](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## Risorse aggiuntive {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
