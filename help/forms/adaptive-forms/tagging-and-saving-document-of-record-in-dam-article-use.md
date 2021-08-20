---
title: Assegnazione tag e archiviazione di AEM Forms DoR in DAM
description: Questo articolo illustra il caso d’uso per l’archiviazione e l’assegnazione di tag al DoR generato da AEM Forms in AEM DAM. L’assegnazione tag al documento viene eseguita in base ai dati del modulo inviati.
feature: Moduli adattivi
version: 6.4,6.5
topic: Sviluppo
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---


# Assegnazione tag e archiviazione di AEM Forms DoR in DAM {#tagging-and-storing-aem-forms-dor-in-dam}

Questo articolo illustra il caso d’uso per l’archiviazione e l’assegnazione di tag al DoR generato da AEM Forms in AEM DAM. L’assegnazione tag al documento viene eseguita in base ai dati del modulo inviati.

Una richiesta comune dei clienti è quella di memorizzare e assegnare tag al documento di record (DoR) generato da AEM Forms in AEM DAM. L’assegnazione tag del documento deve essere basata sui dati inviati da Adaptive Forms. Ad esempio, se lo stato di occupazione nei dati inviati è &quot;Ritirato&quot;, vogliamo assegnare al documento il tag &quot;Ritirato&quot; e memorizzare il documento in DAM.

Il caso d’uso è il seguente:

* Un utente compila il modulo adattivo. Nel modulo adattivo, viene acquisito lo stato civile dell&#39;utente (ex Single) e lo stato di occupazione (Ex Retired).
* All’invio del modulo, viene attivato un flusso di lavoro AEM. Questo flusso di lavoro contrassegna il documento con lo stato civile (Single) e lo stato di occupazione (Retired) e lo archivia in DAM.
* Una volta memorizzato il documento in DAM, l’amministratore deve essere in grado di eseguire ricerche nel documento tramite questi tag. Ad esempio, la ricerca su Single o Retired recupererebbe i DoR appropriati.

Per soddisfare questo caso di utilizzo è stato scritto un passaggio di processo personalizzato. In questo passaggio recuperiamo i valori degli elementi di dati appropriati dai dati inviati. Quindi creiamo il riquadro del tag utilizzando questo valore. Ad esempio, se il valore dell’elemento di stato civile è &quot;Single&quot;, il titolo del tag diventa **Peak:EmploymentStatus/Single. **Utilizzando l’ API TagManager , troviamo il tag e lo applichiamo al DoR.

Lo snippet di codice seguente illustra come trovare il tag e applicarlo al documento.

```java
Tag tagFound = tagManager.resolveByTitle(tagTitle+xmlElement.getTextContent());
//tagTitle is "Peak:EmploymentStatus/" and the xmlElement.getTextContent() will return the value Single. So the tag title becomes Peak:EmploymentStatus/Single. Once the tag is found we put the tag in array and apply the tags to the resource as shown below
tagArray[i] = tagFound;
tagManager.setTags(metadata, tagArray, true);
```

Per far funzionare questo esempio sul sistema, segui i passaggi elencati di seguito:
* [Distribuzione del bundle Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Scarica e distribuisci il bundle setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Questo è il bundle OSGI personalizzato che imposta i tag dai dati del modulo inviati.

* [Scarica il modulo adattivo di esempio](assets/tag-and-store-in-dam-assets.zip)

* [Vai a Forms e documenti](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Fai clic su Crea | Caricamento del file e caricamento del file sampleadaptiveform.zip

* [Importa l&#39;articolo ](assets/tag-and-store-in-dam-assets.zip) tramite AEM gestione pacchetti
* Apri il modulo di esempio [in modalità anteprima](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled). Compila la sezione Persone e invia il modulo.
* [Passa alla cartella Picco in DAM](http://localhost:4502/assets.html/content/dam/Peak). Dovresti visualizzare DoR nella cartella Picco. Controllare le proprietà del documento. Deve essere contrassegnato in modo appropriato.
Congratulazioni!! Installazione dell&#39;esempio sul sistema completata

* Esploriamo il [flusso di lavoro](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) che viene attivato all’invio del modulo.
* Il primo passaggio nel flusso di lavoro crea un nome file univoco concatenando il nome del candidato e la contea di residenza.
* Il secondo passaggio del flusso di lavoro passa la gerarchia dei tag e gli elementi dei campi modulo che devono essere contrassegnati. Il passaggio del processo estrae il valore dai dati inviati e crea il titolo del tag che deve assegnare al documento il tag.
* Se desideri memorizzare DoR in una cartella diversa nel DAM, specifica il percorso della cartella utilizzando le proprietà di configurazione specificate nella schermata seguente.

Gli altri due parametri sono specifici di DoR e Percorso file dati come specificato nelle opzioni di invio del modulo adattivo. Assicurati che i valori qui specificati corrispondano ai valori specificati nelle opzioni di invio del modulo adattivo.

![Barra dei tag](assets/tag_dor_service_configuration.gif)

