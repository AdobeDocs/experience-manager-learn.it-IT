---
title: Assegnazione di tag e memorizzazione  AEM Forms DoR in DAM
seo-title: Assegnazione di tag e memorizzazione  AEM Forms DoR in DAM
description: Questo articolo illustra i casi di utilizzo per memorizzare e assegnare tag ai file DoR generati da  AEM Forms in AEM DAM. Il tag del documento viene eseguito in base ai dati del modulo inviati.
seo-description: Questo articolo illustra i casi di utilizzo per memorizzare e assegnare tag ai file DoR generati da  AEM Forms in AEM DAM. Il tag del documento viene eseguito in base ai dati del modulo inviati.
uuid: b9ba13ed-52d5-4389-a7d5-bf85e58fea49
feature: adaptive-forms,workflow
topics: developing
audience: implementer
doc-type: article
activity: develop
version: 6.4,6.5
discoiquuid: 53961454-633b-4cd8-aef7-e64ab4e528e4
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '655'
ht-degree: 0%

---


# Assegnazione di tag e memorizzazione  AEM Forms DoR in DAM {#tagging-and-storing-aem-forms-dor-in-dam}

Questo articolo illustra i casi di utilizzo per memorizzare e assegnare tag ai file DoR generati da  AEM Forms in AEM DAM. Il tag del documento viene eseguito in base ai dati del modulo inviati.

Una richiesta comune dei clienti è quella di memorizzare e assegnare tag al documento di record (DoR) generato da  AEM Forms in AEM DAM. I tag del documento devono essere basati sui dati inviati da Forms adattivo. Ad esempio, se lo stato dell&#39;occupazione nei dati inviati è &quot;Ritirato&quot;, è necessario assegnare al documento un tag &quot;Ritirato&quot; e memorizzare il documento in DAM.

Il caso di utilizzo è il seguente:

* Un utente compila il modulo adattivo. Nel modulo adattivo, viene acquisito lo stato civile dell&#39;utente (ex Single) e lo stato di occupazione (Ex Retired).
* All&#39;invio del modulo viene attivato un flusso di lavoro AEM. Questo flusso di lavoro tag il documento con lo stato civile (singolo) e lo stato di occupazione (ritirato) e memorizza il documento in DAM.
* Una volta memorizzato il documento in DAM, l&#39;amministratore deve essere in grado di effettuare ricerche nel documento tramite questi tag. Ad esempio, la ricerca su Single o Retired recupera i DoR appropriati.

Per soddisfare questo caso di utilizzo è stato scritto un passaggio di processo personalizzato. In questo passaggio raccogliamo i valori degli elementi di dati appropriati dai dati inviati. Quindi costruiamo la sezione di tag utilizzando questo valore. Ad esempio, se il valore dell&#39;elemento di stato civile è &quot;Single&quot;, il titolo del tag diventa **Peak:EmploymentStatus/Single. **Utilizzando l&#39;API TagManager , troviamo il tag e applichiamo il tag al DoR.

Il frammento di codice seguente illustra come trovare il tag e applicare il tag al documento.

```java
Tag tagFound = tagManager.resolveByTitle(tagTitle+xmlElement.getTextContent());
//tagTitle is "Peak:EmploymentStatus/" and the xmlElement.getTextContent() will return the value Single. So the tag title becomes Peak:EmploymentStatus/Single. Once the tag is found we put the tag in array and apply the tags to the resource as shown below
tagArray[i] = tagFound;
tagManager.setTags(metadata, tagArray, true);
```

Per far funzionare questo esempio sul sistema, segui i passaggi elencati di seguito:
* [Distribuzione del bundle Developingwithservice](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Scaricate e distribuite il bundle](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) setvalue. Si tratta del pacchetto OSGI personalizzato che imposta i tag dai dati del modulo inviati.

* [Download del modulo adattivo di esempio](assets/tag-and-store-in-dam-assets.zip)

* [Vai ad Forms e documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Fate clic su Crea | Caricamento del file e caricamento del file sampleadaptiveform.zip

* [Importare la ](assets/tag-and-store-in-dam-assets.zip) risorsa articolo utilizzando AEM gestore pacchetti
* Aprire il modulo di esempio [in modalità di anteprima](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled). Compila la sezione Persone e invia il modulo.
* [Passa alla cartella Picco in DAM](http://localhost:4502/assets.html/content/dam/Peak). Nella cartella Picco dovrebbe essere visualizzato DoR. Verificare le proprietà del documento. Deve essere contrassegnato in modo appropriato.
Congratulazioni!! Installazione dell&#39;esempio nel sistema completata

* Esaminiamo il [flusso di lavoro](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) che viene attivato all&#39;invio del modulo.
* Il primo passaggio del flusso di lavoro crea un nome di file univoco concatenando il nome del candidato e la contea di residenza.
* Il secondo passaggio del flusso di lavoro supera la gerarchia di tag e gli elementi dei campi modulo che devono essere contrassegnati. Il passaggio del processo estrae il valore dai dati inviati e crea il titolo del tag che deve essere applicato al documento.
* Se si desidera memorizzare il DoR in un&#39;altra cartella del DAM, è possibile specificare il percorso della cartella utilizzando le proprietà di configurazione specificate nella schermata seguente.

Gli altri due parametri sono specifici di DoR e Percorso file dati come specificato nelle opzioni di invio del modulo adattivo. Assicurarsi che i valori specificati qui corrispondano ai valori specificati nelle opzioni di invio del modulo adattivo.

![Tag Dor](assets/tag_dor_service_configuration.gif)

