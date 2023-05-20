---
title: Elencare tipi di risorse personalizzati in AEM Forms
description: Parte 2 dell’elenco dei tipi di risorse personalizzate in AEM Forms
uuid: 6467ec34-e452-4c21-9bb5-504f9630466a
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4b940465-0bd7-45a2-8d01-e4d640c9aedf
topic: Development
role: Developer
level: Experienced
exl-id: f221d8ee-0452-4690-a936-74bab506d7ca
last-substantial-update: 2019-07-10T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---

# Elencare tipi di risorse personalizzati in AEM Forms {#listing-custom-asset-types-in-aem-forms}

## Creazione di un modello personalizzato {#creating-custom-template}

Ai fini di questo articolo, stiamo creando un modello personalizzato per visualizzare i tipi di risorse personalizzati e i tipi di risorse OOTB sulla stessa pagina. Per creare un modello personalizzato, segui le seguenti istruzioni

1. Crea una cartella sling: in /apps. Denomina il componente myportalcomponent
1. Aggiungi una proprietà &quot;fpContentType&quot;. Imposta il suo valore su &quot;**/libs/fd/ fp/formTemplate&quot;.**
1. Aggiungi una proprietà &quot;title&quot; e impostane il valore su &quot;custom template&quot;. Questo è il nome che verrà visualizzato nell’elenco a discesa del componente Ricerca ed elenco
1. Crea un &quot;template.html&quot; in questa cartella. Questo file contiene il codice per applicare uno stile e visualizzare i vari tipi di risorse.

![appsfolder](assets/appsfolder_.png)

Di seguito è riportato un elenco dei vari tipi di risorse che utilizzano il componente Ricerca e lister. Creiamo elementi HTML separati per ogni tipo di risorsa, come mostrato dal tag data-type = &quot;videos&quot;. Per il tipo di risorsa &quot;video&quot; utilizziamo &lt;video> per riprodurre il video in linea. Per il tipo di risorsa &quot;worddocuments&quot; usiamo un markup HTML diverso.

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
>Riga 11 - Cambia l&#39;immagine src in modo che punti a un&#39;immagine a tua scelta in DAM.
>
>Per elencare il Forms adattivo in questo modello, crea un nuovo div e imposta il relativo attributo del tipo di dati su &quot;guide&quot;. È possibile copiare e incollare il div il cui data-type=&quot;printForm e impostare il data-type del div appena copiato su &quot;guide&quot;

## Configurare Il Componente Ricerca Ed Elenco {#configure-search-and-lister-component}

Dopo aver definito il modello personalizzato, è necessario associarlo al componente &quot;Ricerca ed elenco&quot;. Puntare il browser [a questo url ](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html).

Passa alla modalità Progettazione e configura il sistema paragrafo in modo da includere il componente Ricerca ed elenco nel gruppo di componenti consentiti. Il componente Ricerca ed elenco fa parte del gruppo Servizi basati su documenti.

Passa alla modalità di modifica e aggiungi il componente Ricerca ed Elenco ai ParSys.

Apri le proprietà di configurazione del componente &quot;Ricerca ed elenco&quot;. Assicurati che la scheda &quot;Cartelle risorse&quot; sia selezionata. Seleziona le cartelle da cui desideri elencare le risorse nel componente Ricerca e lister. Ai fini del presente articolo, ho selezionato

* /content/dam/VideosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![assetfolder](assets/selectingassetfolders.png)

Passa alla scheda &quot;Visualizzazione&quot;. Qui puoi scegliere il modello in cui visualizzare le risorse nel componente Ricerca e lister.

Seleziona &quot;modello personalizzato&quot; dal menu a discesa, come illustrato di seguito.

![searchandlister](assets/searchandlistercomponent.gif)

Configura i tipi di risorse da elencare nel portale. Per configurare i tipi di scheda delle risorse in &quot;Elenco risorse&quot; e i tipi di risorse. In questo esempio abbiamo configurato i seguenti tipi di risorse

1. File MP4
1. Documenti di Word
1. Documento (tipo di risorsa OOTB)
1. Modello modulo (tipo di risorsa OOTB)

La schermata seguente mostra i tipi di risorse configurati per l’inserzione

![assettipi](assets/assettypes.png)

Dopo aver configurato il componente Portale di ricerca ed elenco, è ora di visualizzare il lister in azione. Puntare il browser [a questo url ](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled). I risultati dovrebbero essere simili all’immagine mostrata di seguito.

>[!NOTE]
>
>Se il portale elenca tipi di risorse personalizzati su un server di pubblicazione, assicurati di concedere l’autorizzazione di lettura all’utente &quot;fd-service&quot; sul nodo **/apps/fd/fp/extensions/querybuilder**

![assettipi](assets/assettypeslistings.png)
[Scarica e installa questo pacchetto utilizzando Gestione pacchetti.](assets/customassettypekt1.zip) Contiene documenti mp4 e word di esempio e file xdp utilizzati come tipi di risorse da elencare utilizzando il componente Ricerca e lister
