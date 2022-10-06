---
title: Utilizzo di cartelle controllate in AEM Forms
description: Configurare e utilizzare le cartelle controllate in AEM Forms
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: abb74d44-d1b9-44d6-a49f-36c01acfecb4
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---

# Utilizzo di cartelle controllate in AEM Forms{#using-watched-folders-in-aem-forms}

Un amministratore può configurare una cartella di rete, nota come Cartella sottoposta a controllo, in modo che quando un utente inserisce un file (ad esempio un file PDF) nella Cartella sottoposta a controllo, un flusso di lavoro, un servizio o un&#39;operazione di script preconfigurata venga l&#39;elaborazione del file aggiunto. Dopo che il servizio esegue l&#39;operazione specificata, salva il file dei risultati in una cartella di output specificata. Per ulteriori informazioni su flusso di lavoro, servizio e script.

Per ulteriori informazioni sulla creazione di una cartella controllata, [fai clic qui](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Cartelle controllate vengono utilizzate per generare documenti in modalità batch. Utilizzando il meccanismo delle cartelle controllate, puoi generare comunicazioni interattive per il canale di stampa oppure utilizzare il servizio di output per unire i dati con il modello.

Questo articolo tratta il caso d’uso dell’unione dei dati con un modello utilizzando il servizio di output tramite il meccanismo delle cartelle controllate.

Il servizio di output è un servizio OSGi che fa parte di AEM Document Services. Il servizio di output supporta vari formati di output e funzioni di progettazione dell’output di AEM Forms Designer. Il servizio di output può convertire modelli XFA e dati XML per generare documenti di stampa in vari formati.

Per ulteriori informazioni sul servizio di output, [fai clic qui](https://helpx.adobe.com/aem-forms/6/output-service.html).
Per configurare la cartella controllata sul sistema, segui i passaggi seguenti:
* [Scaricare ed estrarre il contenuto del file zip](assets/outputservicewatchedfolderkt.zip).Questo file zip contiene il pacchetto per la creazione di cartelle controllate e file di esempio per testare il servizio di output utilizzando il meccanismo di cartelle controllate
   * Sistema Windows

      * Importa il file outputservicewatchedfolder.zip in AEM utilizzando il gestore di pacchetti
      * Viene creata una cartella controllata denominata outputservicewatchedfolder sull&#39;unità C.
   * Sistema non Windows
      * [Apri l&#39;impostazione di configurazione della cartella controllata](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Impostare la proprietà del percorso della cartella del nodo in uscita per puntare a una posizione adeguata
      * Salva le modifiche
      * La posizione di cui sopra è la tua cartella controllata.

Rilasciare le cartelle SamplePdfFile e SampleXdpFile nella cartella di input della cartella controllata. Una volta elaborati correttamente i file, i risultati vengono inseriti nella cartella dei risultati della cartella controllata.


>[!NOTE]
>
>Se lo script associato alla cartella controllata richiede più di un file, è necessario creare una cartella e inserire tutti i file richiesti nella cartella e rilasciare la cartella nella cartella di input della cartella controllata.
>
>Se lo script associato alla cartella controllata richiede un solo file di input, puoi rilasciarlo direttamente nella cartella di input della cartella controllata
