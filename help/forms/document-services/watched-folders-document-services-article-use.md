---
title: Utilizzo delle cartelle esaminate in  AEM Forms
seo-title: Utilizzo delle cartelle esaminate in  AEM Forms
description: Configurare e utilizzare le cartelle esaminate in  AEM Forms
seo-description: Configurare e utilizzare le cartelle esaminate in  AEM Forms
uuid: 32c4bda2-363d-4294-925e-405a176f7f8d
feature: output-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a40e2381-0dc8-4784-9b80-15e27b244035
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---


# Utilizzo delle cartelle esaminate in  AEM Forms{#using-watched-folders-in-aem-forms}

Un amministratore può configurare una cartella di rete, nota come Cartella esaminata, in modo che quando un utente inserisce un file (ad esempio un file PDF) nella cartella esaminata, venga avviato un flusso di lavoro, un servizio o un&#39;operazione script preconfigurata per l&#39;elaborazione del file aggiunto. Dopo che il servizio ha eseguito l&#39;operazione specificata, salva il file dei risultati in una cartella di output specificata. Per ulteriori informazioni su flusso di lavoro, servizio e script.

Per ulteriori informazioni sulla creazione di una cartella esaminata, [fate clic qui](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Cartelle esaminate vengono utilizzate per generare documenti in modalità batch. Utilizzando il meccanismo delle cartelle esaminate, potete generare comunicazioni interattive per il canale di stampa oppure utilizzare il servizio di output per unire i dati con il modello.

Questo articolo descriverà il caso di utilizzo dell&#39;unione di dati con un modello utilizzando il servizio di output tramite il meccanismo delle cartelle esaminate.

Il servizio di output è un servizio OSGi che fa parte di AEM Document Services. Il servizio Output supporta vari formati di output e le funzionalità di progettazione dell&#39;output di  AEM Forms Designer. Il servizio di output può convertire i modelli XFA e i dati XML per generare documenti di stampa in vari formati.

Per ulteriori informazioni sul servizio di output, [fare clic qui](https://helpx.adobe.com/aem-forms/6/output-service.html).
Per configurare la cartella esaminata sul sistema, attenetevi alla procedura seguente:
* [Scaricate ed estraete il contenuto del file](assets/outputservicewatchedfolderkt.zip)zip. Questo file zip contiene il pacchetto per la creazione di cartelle esaminate e file di esempio per testare il servizio di output utilizzando il meccanismo delle cartelle esaminate
   * Sistema Windows

      * Importa outputservicewatchedfolder.zip in AEM utilizzando il gestore pacchetti
      * In questo modo si crea una cartella esaminata denominata outputservicewatchedfolder nell’unità C.
   * Sistema non Windows
      * [Aprire l’impostazione di configurazione della cartella esaminata](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Impostare la proprietà percorso cartella del nodo outservice in modo che punti a una posizione appropriata
      * Salvare le modifiche
      * La posizione sopra indicata sarà la cartella esaminata.

Rilasciate le cartelle SamplePdfFile e SampleXdpFile nella cartella di input della cartella esaminata. Una volta elaborati correttamente i file, i risultati vengono inseriti nella cartella dei risultati della cartella esaminata.


>[!NOTE]
>
>Se lo script associato alla cartella esaminata richiede più di un file, è necessario creare una cartella e inserire tutti i file richiesti nella cartella, quindi rilasciare la cartella nella cartella di input della cartella esaminata.
>
>Se lo script associato alla cartella esaminata richiede un solo file di input, è possibile rilasciare il file direttamente nella cartella di input della cartella esaminata

