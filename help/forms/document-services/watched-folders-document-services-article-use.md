---
title: Utilizzo delle cartelle visualizzate in AEM Forms
seo-title: Utilizzo delle cartelle visualizzate in AEM Forms
description: Configurare e utilizzare le cartelle controllate in AEM Forms
seo-description: Configurare e utilizzare le cartelle controllate in AEM Forms
uuid: 32c4bda2-363d-4294-925e-405a176f7f8d
feature: Servizio di output
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a40e2381-0dc8-4784-9b80-15e27b244035
topic: Sviluppo
role: Developer (Sviluppatore)
level: Intermedio
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 0%

---


# Utilizzo delle cartelle visualizzate in AEM Forms{#using-watched-folders-in-aem-forms}

Un amministratore può configurare una cartella di rete, nota come Cartella sottoposta a controllo, in modo che quando un utente inserisce un file (ad esempio un file PDF) nella Cartella sottoposta a controllo, un flusso di lavoro, un servizio o un&#39;operazione di script preconfigurata venga l&#39;elaborazione del file aggiunto. Dopo che il servizio esegue l&#39;operazione specificata, salva il file dei risultati in una cartella di output specificata. Per ulteriori informazioni su flusso di lavoro, servizio e script.

Per ulteriori informazioni sulla creazione di una cartella guardata, [fai clic qui](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Cartelle controllate vengono utilizzate per generare documenti in modalità batch. Utilizzando il meccanismo delle cartelle controllate, puoi generare comunicazioni interattive per il canale di stampa oppure utilizzare il servizio di output per unire i dati con il modello.

Questo articolo tratta il caso d’uso dell’unione dei dati con un modello utilizzando il servizio di output tramite il meccanismo delle cartelle controllate.

Il servizio di output è un servizio OSGi che fa parte di AEM Document Services. Il servizio di output supporta vari formati di output e funzioni di progettazione dell’output di AEM Forms Designer. Il servizio di output può convertire modelli XFA e dati XML per generare documenti di stampa in vari formati.

Per ulteriori informazioni sul servizio di output, [fare clic qui](https://helpx.adobe.com/aem-forms/6/output-service.html).
Per configurare la cartella controllata sul sistema, segui i passaggi seguenti:
* [Scarica ed estrae il contenuto del file zip](assets/outputservicewatchedfolderkt.zip). Questo file zip contiene il pacchetto per la creazione di cartelle controllate e file di esempio per testare il servizio di output utilizzando il meccanismo di cartelle controllate
   * Sistema Windows

      * Importa outputservicewatchedfolder.zip in AEM utilizzando il gestore di pacchetti
      * Verrà creata una cartella controllata denominata outputservicewatchedfolder sull&#39;unità C.
   * Sistema non Windows
      * [Apri l&#39;impostazione di configurazione della cartella controllata](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Impostare la proprietà del percorso della cartella del nodo in uscita per puntare a una posizione adeguata
      * Salva le modifiche
      * La posizione di cui sopra sarà la tua cartella controllata.

Rilasciare le cartelle SamplePdfFile e SampleXdpFile nella cartella di input della cartella controllata. Una volta elaborati correttamente i file, i risultati vengono inseriti nella cartella dei risultati della cartella controllata.


>[!NOTE]
>
>Se lo script associato alla cartella controllata richiede più di un file, è necessario creare una cartella e inserire tutti i file richiesti nella cartella e rilasciare la cartella nella cartella di input della cartella controllata.
>
>Se lo script associato alla cartella controllata richiede un solo file di input, puoi rilasciarlo direttamente nella cartella di input della cartella controllata

