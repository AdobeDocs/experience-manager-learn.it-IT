---
title: Informazioni sui file e i modelli di risorse di InDesign in AEM Assets
description: Questo tutorial video illustra come definire un file InDesign e tutte le relative considerazioni da utilizzare nella funzione Modelli di risorse di AEM Assets.
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
feature: Templates
role: User
level: Intermediate
doc-type: Tutorial
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
duration: 909
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 0%

---

# Informazioni sui file e i modelli di risorse di InDesign in AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

Questo tutorial video illustra come definire un file InDesign e tutte le relative considerazioni da utilizzare nella funzione Modelli di risorse di AEM Assets.

## Creazione del file di modello di InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. Scarica e apri il [**modello di file InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Aprire il pannello Tag,** rivedere la convenzione di denominazione dei tag e notare che gli elementi modificabili nel file InDesign sono già contrassegnati. In AEM è possibile modificare solo gli elementi con tag.

   * **Finestra > Utilità > Tag**

3. A pagina, aggiungi un nuovo elemento di testo, fornisci il testo &quot;Intestazione&quot; e applica lo stile di paragrafo **Intestazione**.

   * **Finestra > Stili > Stili paragrafo**

   Quindi, creare e applicare un nuovo tag denominato **Page2Heading.**

4. Aggiungere l&#39;immagine del logo FPO ([fornita nello zip](assets/asset-templates-tutorial-video--supporting-files.zip)) all&#39;elemento Logo nella pagina master.

   * **Fare clic con il pulsante destro del mouse su** e selezionare **Raccordo > Opzioni raccordo cornice... > Raccordo contenuto > Riempi cornice in modo proporzionale**

   [Ulteriori informazioni sulle opzioni di Frame Fitting](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames) e su quale sia il caso d&#39;uso più adatto.

5. Copiare l&#39;intestazione (Logo e Nome società) dal modello principale in Pagina e Pagina tramite Incolla nella posizione.

   * A pagina 1, fare clic su macOS tenendo premuto Maiusc-Cmd o su Windows tenendo premuto Maiusc-Alt, per selezionare l&#39;intestazione esposta dalla pagina master ed eliminarla.
   * Dalla pagina master, copia l’intestazione nella pagina 1 tramite Incolla nella posizione
   * Ripeti i passaggi per Pagina 2

6. Apri il pannello Struttura facendo doppio clic su ciascuno di essi e assicurati che tutti gli elementi strutturali corrispondano agli elementi reali nel file di InDesign. Rimuovere eventuali elementi non utilizzati o non necessari. Assicurati che tutti i tag siano semantici e che gli elementi siano contrassegnati correttamente.

   >[!NOTE]
   >
   >Tieni presente che la causa più comune di problemi con i modelli di risorse di AEM è un file InDesign mal costruito, quindi assicurati che l’assegnazione tag e la struttura siano pulite e corrette.

## Creazione e authoring di un modello di risorse in AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294?quality=12&learn=on)

1. **Avvia InDesign Server** sulla porta 8080.
2. Assicurati che l&#39;istanza Autore **AEM sia configurata per interagire con il tuo InDesign Server** (e viceversa).

   * [Configurazione Cloud Service di lavoro IDS](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configurazione Cloud Service proxy cloud](http://localhost:4502/etc/cloudservices/proxy.html)
   * [Configurazione OSGi di AEM Externalizer](http://localhost:4502/system/console/configMgr)

3. **Il file InDesign è stato caricato in AEM Assets** e i flussi di lavoro AEM e InDesign Server sono stati autorizzati a elaborare completamente le risorse.
4. **Crea un nuovo modello** in **Assets > Modelli** e seleziona il file InDesign caricato in AEM nel passaggio #4.
5. **Modifica il modello di risorsa** creato nel passaggio #5 e crea i campi modificabili.
6. Fai clic su **Fine** per generare le rappresentazioni finali ad alta fedeltà del modello di risorsa.
7. Fai clic sulla scheda Modello risorse per aprire ed esaminare Rappresentazioni risorse per scaricare le rappresentazioni ad alta fedeltà.

## Risorse aggiuntive {#additional-resources}

File di modello InDesign e immagini di supporto

Scarica [file modello InDesign e immagini di supporto](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Download di prova di InDesign CC](https://creative.adobe.com/products/download/indesign)
* La versione di prova di InDesign Server può essere scaricata dal [sito prerelease di Adobe](https://www.adobeprerelease.com/) o [CC I clienti Enterprise possono contattare il proprio Account Executive per richiedere la licenza di prova di InDesign Server](https://www.adobe.com/products/indesignserver/faq.html)
