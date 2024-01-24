---
title: Informazioni sui file e i modelli di risorse InDesign in AEM Assets
description: Questo tutorial video illustra come definire un file InDesign e tutte le considerazioni che lo accompagnano, da utilizzare nella funzione Modelli di risorse di AEM Assets.
version: 6.4, 6.5
topic: Content Management
feature: Templates
role: User
level: Intermediate
doc-type: Tutorial
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
duration: 925
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 0%

---

# Informazioni sui file e i modelli di risorse InDesign in AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

Questo tutorial video illustra come definire un file InDesign e tutte le considerazioni che lo accompagnano, da utilizzare nella funzione Modelli di risorse di AEM Assets.

## Creazione del file modello InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. Scarica e apri la [**Modello di file InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Aprire il pannello Tag,** rivedi la convenzione per la denominazione dei tag e osserva che gli elementi modificabili nel file InDesign sono già taggati. Ricorda che solo gli elementi taggati sono modificabili in AEM.

   * **Finestra > Utilità > Tag**

3. Nella pagina, aggiungi un nuovo elemento di testo, fornisci il testo &quot;Intestazione&quot; e applica il **Intestazione** Stile paragrafo.

   * **Finestra > Stili > Stili paragrafo**

   Quindi, crea e applica un nuovo tag denominato **Page2Heading**

4. Aggiungere l&#39;immagine del logo FPO ([fornito nel file zip](assets/asset-templates-tutorial-video--supporting-files.zip)) all&#39;elemento Logo nella pagina mastro.

   * **Clic destro** e seleziona **Raccordo > Opzioni raccordo cornice... > Raccordo contenuto > Riempi cornice in modo proporzionale**

   [Ulteriori informazioni sulle opzioni di Raccordo fotogrammi](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames), e che è adatto al tuo caso d’uso.

5. Copiare l&#39;intestazione (Logo e Nome società) dal modello principale in Pagina e Pagina tramite Incolla nella posizione.

   * A pagina 1, fare clic su macOS tenendo premuto Maiusc-Cmd o su Windows tenendo premuto Maiusc-Alt, per selezionare l&#39;intestazione esposta dalla pagina master ed eliminarla.
   * Dalla pagina master, copia l’intestazione nella pagina 1 tramite Incolla nella posizione
   * Ripeti i passaggi per Pagina 2

6. Aprite il pannello Struttura, facendo doppio clic su ciascuno di essi, assicuratevi che tutti gli elementi strutturali corrispondano a elementi reali nel file InDesign. Rimuovere eventuali elementi non utilizzati o non necessari. Assicurati che tutti i tag siano semantici e che gli elementi siano contrassegnati correttamente.

   >[!NOTE]
   >
   >Tieni presente che la causa più comune di problemi con i modelli di risorse AEM è un file InDesign mal costruito, quindi assicurati che l’assegnazione tag e la struttura siano pulite e corrette.

## Creazione e authoring di un modello di risorse in AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294?quality=12&learn=on)

1. **Avvia InDesign Server** sulla porta 8080.
2. Assicurati che **L’istanza di authoring AEM è configurata per interagire con l’InDesign Server** e viceversa.

   * [Configurazione Cloud Service di lavoro IDS](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configurazione Cloud Service proxy cloud](http://localhost:4502/etc/cloudservices/proxy.html)
   * [Configurazione OSGi di AEM Externalizer](http://localhost:4502/system/console/configMgr)

3. **Caricamento del file InDesign in AEM Assets** e consentire a AEM Workflow e InDesign Server di elaborare completamente le risorse.
4. **Crea un nuovo modello** in **Risorse > Modelli** e seleziona il file InDesign caricato nell’AEM al punto #4.
5. **Modificare il modello di risorsa** create nel passaggio #5 e creare i campi modificabili.
6. Clic **Fine** per generare le rappresentazioni finali ad alta fedeltà del modello di risorsa.
7. Fai clic sulla scheda Modello risorse per aprire ed esaminare Rappresentazioni risorse per scaricare le rappresentazioni ad alta fedeltà.

## Risorse aggiuntive {#additional-resources}

File modello InDesign e immagini di supporto

Scarica [File modello InDesign e immagini di supporto](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Download di prova CC InDesign](https://creative.adobe.com/products/download/indesign)
* La versione di prova InDesign Server può essere scaricata da [Adobe di sito prerelease](https://www.adobeprerelease.com/) o [I clienti CC Enterprise possono contattare il proprio Account Executive per richiedere una licenza di prova InDesign Server](https://www.adobe.com/products/indesignserver/faq.html)
