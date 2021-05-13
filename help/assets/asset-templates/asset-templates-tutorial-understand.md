---
title: 'Informazioni sui file InDesign e i modelli di risorse in AEM Assets '
description: Questa esercitazione video illustra come definire un file InDesign e tutte le relative considerazioni da utilizzare nella funzione Modelli di risorse di AEM Assets.
version: 6.3, 6.4, 6.5
topic: Gestione dei contenuti
role: Business Practitioner
level: Intermediate
source-git-commit: b4fa992abe22e3a546d651e465d6ffc9e415aee2
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---


# Informazioni sui file InDesign e i modelli di risorse in AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

Questa esercitazione video illustra come definire un file InDesign e tutte le relative considerazioni da utilizzare nella funzione Modelli di risorse di AEM Assets.

## Creazione del file modello di InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. Scarica e apri il [**modello di file InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Apri il pannello Tag,** controlla la convenzione di denominazione dei tag e osserva che gli elementi modificabili dall’autore nel file InDesign sono già dotati di tag. In AEM è possibile modificare solo gli elementi con tag.

   * **Finestra > Utilità > Tag**

3. Nella pagina, aggiungi un nuovo elemento di testo, fornisci il testo &quot;Intestazione&quot; e applica lo stile di paragrafo **Intestazione**.

   * **Finestra > Stili > Stili paragrafo**

   Quindi, crea e applica un nuovo tag denominato **Page2Header.**

4. Aggiungi l&#39;immagine logo FPO ([fornita nel file ZIP](assets/asset-templates-tutorial-video--supporting-files.zip)) all&#39;elemento Logo nella pagina master.

   * **Fai clic con il pulsante destro del mouse** e **seleziona Adatta > Opzioni raccordo cornice.. > Raccordo contenuto > Riempi cornice proporzionalmente**
   [Scopri di più sulle opzioni](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames) di raccordo frame e qual è il caso d’uso più adatto alle tue esigenze.

5. Copia l’intestazione (Logo e Nome società) dal modello principale in Pagina e Pagina tramite Incolla nella stessa posizione.

   * A Pagina 1, Maiusc+Cmd+Clic su macOS o Maiusc+Alt+Clic su Windows, per selezionare l&#39;intestazione esposta dalla pagina master ed eliminarla.
   * Dalla pagina master, copia l’intestazione nella pagina 1 tramite Incolla nella stessa posizione
   * Ripeti i passaggi per Pagina 2

6. Apri il pannello Struttura facendo doppio clic su ciascuno di essi per assicurarsi che tutti gli elementi strutturali corrispondano agli elementi reali nel file InDesign. Rimuovi eventuali elementi non utilizzati o non necessari. Assicurati che tutti i tag siano semantici e che gli elementi siano contrassegnati correttamente.

   >[!NOTE]
   >
   >Tieni presente che un file InDesign di struttura non corretta è la causa più comune dei problemi relativi AEM modelli di risorse, pertanto assicurati che l’assegnazione tag e la struttura siano pulite e corrette.

## Creazione e creazione di un modello di risorse in AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **Avvia la porta 8080** del server InDesign.
2. Assicurati che l’ **istanza di authoring AEM sia configurata per interagire con il tuo InDesign Server**(e viceversa).

   * [Configurazione del Cloud Service di lavoro IDS](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configurazione del Cloud Service proxy cloud](http://localhost:4502/etc/cloudservices/proxy.html)
   * [Configurazione di AEM Externalizer OSGi](http://localhost:4502/system/console/configMgr)

3. **È stato caricato il file InDesign in AEM** Assets e consente AEM flusso di lavoro e ad InDesign Server di elaborare completamente le risorse.
4. **Crea un nuovo** modello in  **Risorse >** Modelli e seleziona il file InDesign caricato in AEM al passaggio #4.
5. **Modifica il** modello di risorsa creato al passaggio 5 e crea i campi modificabili.
6. Fai clic su **Fine** per generare le rappresentazioni finali ad alta fedeltà del modello di risorse.
7. Fai clic sulla scheda Modello risorse per aprire e rivedi le rappresentazioni delle risorse per scaricare le rappresentazioni ad alta fedeltà.

## Risorse aggiuntive {#additional-resources}

File modello di InDesign e immagini di supporto

Scarica [il file modello di InDesign e supporta le immagini](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Download di versione di prova di InDesign CC](https://creative.adobe.com/products/download/indesign)
* [I clienti CC Enterprise possono contattare il proprio Account Executive per richiedere la licenza di prova di InDesign Server](https://www.adobe.com/products/indesignserver/faq.html)
