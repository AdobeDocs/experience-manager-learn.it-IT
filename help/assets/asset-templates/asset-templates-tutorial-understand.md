---
title: Informazioni sui file InDesign e i modelli di risorse in AEM Assets
description: Questa esercitazione video illustra come definire un file InDesign e tutte le relative considerazioni da utilizzare nella funzione Modelli di risorse di AEM Assets.
version: 6.3, 6.4, 6.5
topic: Content Management
role: User
level: Intermediate
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
source-git-commit: bf5b2fca04c09fd52df8ef8d9fca8b4b7bd2de2f
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 1%

---

# Informazioni sui file InDesign e i modelli di risorse in AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

Questa esercitazione video illustra come definire un file InDesign e tutte le relative considerazioni da utilizzare nella funzione Modelli di risorse di AEM Assets.

## Creazione del file modello di InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. Scarica e apri le [**Modello di file InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Apri il pannello Tag ,** controlla la convenzione di denominazione dei tag e osserva che gli elementi modificabili dall’autore nel file InDesign sono già dotati di tag. In AEM è possibile modificare solo gli elementi con tag.

   * **Finestra > Utilità > Tag**

3. Sulla pagina, aggiungi un nuovo elemento di testo, fornisci il testo &quot;Intestazione&quot; e applica il **Intestazione** Stile paragrafo.

   * **Finestra > Stili > Stili paragrafo**

   Quindi, crea e applica un nuovo tag denominato **Page2Header.**

4. Aggiungi l&#39;immagine logo FPO ([nella zip](assets/asset-templates-tutorial-video--supporting-files.zip)) all’elemento Logo nella pagina master.

   * **Clic destro** e seleziona **Adattamento > Opzioni di raccordo cornice.. > Adattamento contenuto > Riempimento cornice proporzionalmente**
   [Ulteriori informazioni sulle opzioni di raccordo frame](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames), ed è corretto per il tuo caso d’uso.

5. Copia l’intestazione (Logo e Nome società) dal modello principale in Pagina e Pagina tramite Incolla nella stessa posizione.

   * A pagina 1, Maiusc+Cmd+clic su macOS o Maiusc+Alt+clic su Windows, per selezionare l’intestazione esposta dalla pagina master ed eliminarla.
   * Dalla pagina master, copia l’intestazione nella pagina 1 tramite Incolla nella stessa posizione
   * Ripeti i passaggi per Pagina 2

6. Apri il pannello Struttura facendo doppio clic su ciascuno di essi per assicurarsi che tutti gli elementi strutturali corrispondano agli elementi reali nel file InDesign. Rimuovi eventuali elementi non utilizzati o non necessari. Assicurati che tutti i tag siano semantici e che gli elementi siano contrassegnati correttamente.

   >[!NOTE]
   >
   >Tieni presente che un file InDesign di struttura non corretta è la causa più comune dei problemi relativi AEM modelli di risorse, pertanto assicurati che l’assegnazione tag e la struttura siano pulite e corrette.

## Creazione e creazione di un modello di risorse in AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **Avvia InDesign Server** sulla porta 8080.
2. Assicurati che **L’istanza di AEM Author è configurata per interagire con il tuo InDesign Server**(e viceversa).

   * [Configurazione del Cloud Service di lavoro IDS](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configurazione del Cloud Service proxy cloud](http://localhost:4502/etc/cloudservices/proxy.html)
   * [Configurazione di AEM Externalizer OSGi](http://localhost:4502/system/console/configMgr)

3. **Caricamento del file InDesign in AEM Assets** e consentire a AEM Workflow e InDesign Server di elaborare completamente le risorse.
4. **Creare un nuovo modello** sotto **Risorse > Modelli** e seleziona il file InDesign caricato in AEM al passaggio n. 4.
5. **Modificare il modello di risorsa** creato nel passaggio 5, e crea i campi modificabili.
6. Fai clic su **Fine** generare le rappresentazioni finali ad alta fedeltà del modello di risorse.
7. Fai clic sulla scheda Modello risorse per aprire e rivedi le rappresentazioni delle risorse per scaricare le rappresentazioni ad alta fedeltà.

## Risorse aggiuntive {#additional-resources}

File modello di InDesign e immagini di supporto

Scarica [File modello di InDesign e immagini di supporto](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Download di versione di prova di InDesign CC](https://creative.adobe.com/products/download/indesign)
* La versione di prova di InDesign Server può essere scaricata da [Sito prerelease di Adobe](https://www.adobeprerelease.com/) o [I clienti CC Enterprise possono contattare il proprio Account Executive per richiedere la licenza di prova di InDesign Server](https://www.adobe.com/products/indesignserver/faq.html)
