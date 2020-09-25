---
title: 'Informazioni  file InDesign e modelli di risorse in  AEM Assets '
description: Questa esercitazione video illustra come definire un file InDesign  e tutte le relative considerazioni da utilizzare nella funzione Modelli di risorse di AEM.
feature: catalogs, asset-templates
topics: authoring, renditions, documents
audience: all
doc-type: tutorial
activity: understand
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 0%

---


# Informazioni  file InDesign e modelli di risorse in  AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

Questa esercitazione video illustra come definire un file InDesign  e tutte le relative considerazioni da utilizzare nella funzione Modelli di risorse di AEM.

## Creazione del file modello di InDesign  {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. Scaricare e aprire il modello di file InDesign [****](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Aprite il pannello Tag,** controllate la convenzione di denominazione dei tag e tenete presente che gli elementi modificabili nel file InDesign  sono già dotati di tag. In AEM è possibile modificare solo gli elementi con tag.

   * **Finestra > Utility > Tag**

3. Sulla pagina, aggiungere un nuovo elemento di testo, fornire il testo &quot;Intestazione&quot; e applicare lo stile paragrafo **Intestazione** .

   * **Finestra > Stili > Stili paragrafo**

   Quindi, create e applicate un nuovo tag denominato **Page2Heading.**

4. Aggiungete l’immagine Logo FPO ([fornita nel file ZIP](assets/asset-templates-tutorial-video--supporting-files.zip)) all’elemento Logo nella pagina master.

   * **Fare clic con il pulsante destro del mouse** e selezionare **Adatta > Opzioni adattamento cornice... > Adatta contenuto > Riempi cornice proporzionalmente**
   [Ulteriori informazioni sulle opzioni](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)di Adatta cornice, e che è adatto al caso d&#39;uso.

5. Copiate l’intestazione (Logo e Nome società) dal modello principale in Pagina e Pagina tramite Incolla nella stessa posizione.

   * A pagina 1, tenete premuto Maiusc e Comando e fate clic su macOS o Maiusc e Alt e fate clic su Windows per selezionare l’intestazione esposta dalla pagina master ed eliminarla.
   * Dalla pagina master, copiare l’intestazione nella pagina 1 tramite Incolla nella stessa posizione
   * Ripetere i passaggi per Pagina 2

6. Aprite il pannello Struttura facendo doppio clic su di essi, accertatevi che tutti gli elementi strutturali corrispondano agli elementi reali nel file InDesign . Rimuovete eventuali elementi non utilizzati o non necessari. Assicurarsi che tutti i tag siano semantici e che gli elementi siano contrassegnati correttamente.

   >[!NOTE]
   >
   >Tenete presente che un file InDesign  di struttura scadente è la causa più comune di problemi con AEM modelli di risorse, pertanto accertatevi che l’assegnazione di tag e la struttura siano puliti e corretti.

## Creazione e creazione di un modello di risorsa in  AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **Avviate  InDesign Server** sulla porta 8080.
2. Verifica che l&#39;istanza di **AEM Author sia configurata per interagire con il tuo InDesign Server** (e viceversa).

   * [Configurazione Cloud Service di lavoro IDS](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configurazione Cloud Service proxy cloud](http://localhost:4502/etc/cloudservices/proxy.html)
   * [Configurazione AEM Externalizer OSGi](http://localhost:4502/system/console/configMgr)

3. **È stato caricato il file  InDesign in  AEM Assets** e AEM Workflow e  InDesign Server per elaborare completamente le risorse.
4. **Crea un nuovo modello** in **Risorse > Modelli** e seleziona il file InDesign  caricato in AEM al passaggio 4.
5. **Modificate il Modello** risorse creato al passaggio 5 e create i campi modificabili.
6. Fate clic su **Fine** per generare le rappresentazioni finali ad alta fedeltà del Modello risorsa.
7. Fate clic sulla scheda Modello risorse per aprire e rivedere le rappresentazioni delle risorse per scaricare le rappresentazioni ad alta fedeltà.

## Risorse aggiuntive {#additional-resources}

 file di modello InDesign e immagini di supporto

Scaricare [file modello InDesign e le immagini di supporto](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Download  versione di prova InDesign CC](https://creative.adobe.com/products/download/indesign)
* [Download della versione di prova InDesign Server](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)
