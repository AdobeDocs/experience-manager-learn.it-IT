---
title: Utilizzo delle cartelle controllate in AEM Forms
description: Configurare e utilizzare le cartelle controllate in AEM Forms
feature: Output Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: abb74d44-d1b9-44d6-a49f-36c01acfecb4
last-substantial-update: 2020-07-07T00:00:00Z
duration: 86
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# Utilizzo delle cartelle controllate in AEM Forms{#using-watched-folders-in-aem-forms}

Un amministratore può configurare una cartella di rete, nota come cartella controllata, in modo che, quando un utente inserisce un file (ad esempio un file PDF) nella cartella controllata, venga avviato un flusso di lavoro, un servizio o un’operazione di script preconfigurati per elaborare il file aggiunto. Dopo aver eseguito l&#39;operazione specificata, il servizio salva il file dei risultati in una cartella di output specificata. Per ulteriori informazioni su flusso di lavoro, servizio e script.

Per ulteriori informazioni sulla creazione di una cartella controllata, [fai clic qui](https://helpx.adobe.com/it/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Le cartelle controllate vengono utilizzate per generare documenti in modalità batch. Utilizzando il meccanismo di cartelle controllate, puoi generare comunicazioni interattive per il canale di stampa o utilizzare il servizio di output per unire i dati con il modello.

Questo articolo tratta il caso di utilizzo dell’unione di dati con un modello utilizzando il servizio di output tramite il meccanismo delle cartelle controllate.

Il servizio di output è un servizio OSGi incluso in AEM Document Services. Il servizio di output supporta vari formati di output e funzioni di progettazione dell’output di AEM Forms Designer. Il servizio di output può convertire modelli XFA e dati XML per generare documenti di stampa in vari formati.

Per ulteriori informazioni sul servizio di output, [fare clic qui](https://helpx.adobe.com/it/aem-forms/6/output-service.html).
Per impostare la cartella controllata sul sistema, attenersi alla seguente procedura:
* [Scarica ed estrai il contenuto del file zip](assets/outputservicewatchedfolderkt.zip). Questo file zip contiene un pacchetto per la creazione di cartelle controllate e file di esempio per testare il servizio di output utilizzando il meccanismo delle cartelle controllate
   * Sistema Windows

      * Importare outputservicewatchedfolder.zip in AEM utilizzando Gestione pacchetti
      * In questo modo viene creata nell&#39;unità C una cartella controllata denominata outputservicewatchedfolder.
   * Sistema non Windows
      * [Aprire l&#39;impostazione di configurazione della cartella controllata](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Impostare la proprietà del percorso della cartella del nodo di assistenza in uscita in modo che punti a una posizione appropriata
      * Salva le modifiche
      * Il percorso indicato sopra è la cartella controllata.

Trascina le cartelle SamplePdfFile e SampleXdpFile nella cartella di input della cartella controllata. Una volta completata l’elaborazione dei file, i risultati vengono inseriti nella cartella dei risultati della cartella controllata.


>[!NOTE]
>
>Se lo script associato alla cartella controllata richiede più di un file, è necessario creare una cartella, inserire tutti i file richiesti nella cartella e rilasciare la cartella nella cartella di input della cartella controllata.
>
>Se lo script associato alla cartella controllata richiede un solo file di input, è possibile rilasciare il file direttamente nella cartella di input della cartella controllata
