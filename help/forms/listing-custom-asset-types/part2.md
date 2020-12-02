---
title: Elenco dei tipi di risorse personalizzate in  AEM Forms
seo-title: Elenco dei tipi di risorse personalizzate in  AEM Forms
description: Parte 2 dell’elenco dei tipi di risorse personalizzate in  AEM Forms
seo-description: Parte 2 dell’elenco dei tipi di risorse personalizzate in  AEM Forms
uuid: 6467ec34-e452-4c21-9bb5-504f9630466a
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
discoiquuid: 4b940465-0bd7-45a2-8d01-e4d640c9aedf
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '611'
ht-degree: 0%

---


# Elenco dei tipi di risorse personalizzate in  AEM Forms {#listing-custom-asset-types-in-aem-forms}

## Creazione di un modello personalizzato {#creating-custom-template}


Ai fini di questo articolo, verrà creato un modello personalizzato per visualizzare i tipi di risorse personalizzate e i tipi di risorse OOOTB sulla stessa pagina. Per creare un modello personalizzato, attenetevi alle seguenti istruzioni

1. Creare una fionda: in /apps. Denominatelo &quot; myportalcomponent &quot;
1. Aggiungete una proprietà &quot;fpContentType&quot;. Impostate il valore su &quot;**/libs/fd/ fp/formTemplate&quot;.**
1. Aggiungete una proprietà &quot;title&quot; e impostatene il valore su &quot;custom template&quot;. Nome visualizzato nell’elenco a discesa del componente Ricerca e Ricerca
1. Create un &quot;template.html&quot; sotto questa cartella. Questo file contiene il codice per lo stile e visualizza i vari tipi di risorse.

![appsfolder](assets/appsfolder_.png)

Il codice seguente elenca i vari tipi di risorse che utilizzano il componente Cerca e Lister. Per ciascun tipo di risorsa vengono creati elementi HTML separati, come mostrato dal tag data-type = &quot;video&quot;. Per il tipo di risorsa &quot;video&quot;, utilizziamo l&#39;elemento &lt;video> per riprodurre il video in linea. Per il tipo di risorsa &quot;worddocuments&quot; utilizziamo un diverso formato HTML.

```html
<div class="__FP_boxes-container __FP_single-color">
   <div  data-repeatable="true">
     <div class = "__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "videos">
   <video width="400" controls>
       <source src="${path}" type="video/mp4">
    </video>
         <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
     <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "worddocuments">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="/content/dam/worddocuments/worddocument.png/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "xfaForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{formUrl}"><img src="/content/dam/html5.png"></a><p>

     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "printForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{pdfUrl}"><img src="/content/dam/pdf.png"></a><p>
     </div>
   </div>
</div>
```

>[!NOTE]
>
>Linea 11 - Modificare l&#39;immagine src in modo che punti a un&#39;immagine di vostra scelta in DAM.
>
>Per elencare l&#39;Forms adattivo in questo modello, create un nuovo div e impostate l&#39;attributo del tipo di dati su &quot;guide&quot;. È possibile copiare e incollare il div i cui dati-type=&quot;printForm e impostare il tipo di dati del div appena copiato su &quot;guide&quot;

## Configurare il componente Ricerca e Archivia {#configure-search-and-lister-component}

Una volta definito il modello personalizzato, ora è necessario associare questo modello personalizzato al componente &quot;Cerca e chiudi&quot;. Posizionare il browser [su questo URL ](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html).

Passate alla modalità Progettazione e configurate il sistema di paragrafi per includere il componente Cerca e nascondi nel gruppo di componenti consentito. Il componente Ricerca e filtro fa parte del gruppo Document Services.

Passate alla modalità di modifica e aggiungete il componente Cerca e Controlla ai ParSys.

Aprire le proprietà di configurazione del componente &quot;Cerca e chiudi&quot;. Accertatevi che la scheda &quot;Cartelle risorse&quot; sia selezionata. Selezionate le cartelle da cui desiderate elencare le risorse nel componente Ricerca e Elenco. Ai fini di questo articolo, ho selezionato

* /content/dam/VideosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![assetfolder](assets/selectingassetfolders.png)

Passa alla scheda &quot;Display&quot;. Qui potete scegliere il modello in cui visualizzare le risorse nel componente Ricerca e Lister.

Seleziona &quot;modello personalizzato&quot; dall&#39;elenco a discesa come mostrato di seguito.

![ricercatore](assets/searchandlistercomponent.gif)

Configura i tipi di risorse da elencare nel portale. Per configurare i tipi di scheda della risorsa in &quot;Elenco risorse&quot; e configurare i tipi di risorse. In questo esempio sono stati configurati i seguenti tipi di risorse

1. File MP4
1. Documenti Word
1. Document(Tipo di risorsa OOTB)
1. Modello modulo (tipo di risorsa OOTB)

La schermata seguente mostra i tipi di risorse configurati per l’elenco

![assettypes](assets/assettypes.png)

Ora che hai configurato il componente Ricerca e Lister Portal, è ora di vedere il listener in azione. Posizionare il browser [su questo URL ](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled). I risultati dovrebbero essere simili all’immagine mostrata di seguito.

>[!NOTE]
>
>Se il portale elenca tipi di risorse personalizzate su un server di pubblicazione, accertatevi di concedere l&#39;autorizzazione di lettura per l&#39;utente &quot;fd-service&quot; al nodo **/apps/fd/fp/extensions/querybuilder**

![](assets/assettypeslistings.png)
[assettypesScaricate e installate il pacchetto utilizzando il gestore pacchetti.](assets/customassettypekt1.zip) Contiene esempi di documenti mp4 e Word e file xdp che verranno utilizzati come tipi di risorse da elencare utilizzando il componente Cerca e Lister
