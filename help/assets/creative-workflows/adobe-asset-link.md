---
title: Utilizzo ’estensione collegamento risorse Adobe con  AEM Assets
description: 'Le risorse Adobe Experience Manager ora possono essere utilizzate da designer e utenti creativi nelle loro applicazioni desktop Adobe Creative Cloud preferite. ’estensione Collegamento risorse di Adobe per Adobe Creative Cloud Enterprise estende la capacità di cercare e navigare, ordinare, visualizzare in anteprima, caricare risorse, estrarre, modificare, archiviare e visualizzare i metadati di AEM risorse in strumenti di Creative Cloud come  Adobe Photoshop,  InDesign e  Illustrator. '
feature: adobe-asset-link
topics: authoring, collaboration, operations, sharing, metadata, images
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '1092'
ht-degree: 1%

---


# Utilizzo ’estensione collegamento risorse Adobe con  AEM Assets{#using-adobe-asset-link-extension-with-aem-assets}

Le risorse Adobe Experience Manager ora possono essere utilizzate da designer e utenti creativi nelle loro applicazioni desktop Adobe Creative Cloud preferite. ’estensione Collegamento risorse di Adobe per Adobe Creative Cloud Enterprise estende la capacità di cercare e navigare, ordinare, visualizzare in anteprima, caricare risorse, estrarre, modificare, archiviare e visualizzare i metadati di AEM risorse in strumenti di Creative Cloud come  Adobe Photoshop,  InDesign e  Illustrator.


##  collegamento risorsa Adobe 1.1

 collegamento risorsa Adobe v1.1 ora fornisce  supporto per il collegamento diretto tra  collegamento risorse Adobe InDesign e  AEM Assets. Con  supporto per il collegamento diretto InDesign, ora potete inserire (Inserisci collegato o Inserisci copia) o trascinare risorse digitali in  InDesign da  AEM Assets tramite il pannello Collegamento risorse Adobe . Inoltre, introduce la rappresentazione *Solo* posizionamento (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Utilizzate solo l&#39;Enterprise ID Adobe Creative Cloud  o il Federated ID. Accertatevi di [configurare AEM per  collegamento](https://helpx.adobe.com/enterprise/using/configure-aem-for-aal-prerelease.html)risorse Adobe.


###  collegamento risorse Adobe

*  collegamento risorsa Adobe è un’estensione che funziona in PS, AI, ID e fornisce l’accesso diretto alle risorse digitali che risiedono in  AEM Assets
* I creativi verranno AEM automaticamente utilizzando il loro  Adobe IMS  Enterprise ID o Federated ID
* I creativi possono sfogliare le risorse digitali che risiedono in  AEM Assets, oltre a effettuare ricerche  AEM Assets e Creative Cloud risorse
* i creativi possono accedere ai dettagli dei file per le risorse residenti  AEM Assets; miniature, metadati di base e versioni direttamente dal pannello
* I creativi possono inserire, scaricare o trascinare risorse nel layout
* I creativi possono modificare le risorse estraendole da  AEM Assets e lavorandole (WIP) all&#39;interno del loro account Creative Cloud risorse
* I creativi possono archiviare nuovamente una risorsa in  AEM Assets dopo che hanno terminato di modificarla, e la nuova versione verrà riprodotta in  AEM Assets
* Supporta le app desktop Creative Cloud 2020, 2019 e 2018  InDesign, Photoshop e  Illustrator
* Un utente può eseguire una ricerca di risorse dal pannello in-app Collegamento risorsa  Adobe e ordinarle in base alle dimensioni, all’ordine alfabetico e alla pertinenza
* Gli utenti possono accedere e sfogliare  raccolte AEM Assets e raccolte smart direttamente dal pannello Collegamento risorsa
* Aggiungere risorse appena create a  AEM Assets direttamente dal pannello
* Un utente può trascinare risorse direttamente nelle cornici  InDesign

### Inserimento  AEM Assets nel InDesign 

Potete inserire una risorsa nel layout InDesign  utilizzando una delle seguenti opzioni:

* **Inserisci copia** - L&#39;incorporazione di una risorsa (mediante l&#39;opzione Inserisci copia) consente di inserire una copia della risorsa originale nel layout InDesign  dopo il download dei file binari nel sistema locale.  collegamento risorsa Adobe non mantiene alcun collegamento tra la copia incorporata e la risorsa originale. Se la risorsa originale viene modificata in  AEM Assets, dovete eliminare la risorsa incorporata dal file del InDesign  e incorporarla nuovamente  AEM Assets.

* **Inserisci collegato** : quando lavorate con  documenti InDesign, ora potete fare riferimento alle risorse da  AEM Assets e incorporarle direttamente (tramite l&#39;opzione Inserisci copia nel menu di scelta rapida). Il riferimento alle risorse consente di collaborare con altri utenti e di incorporare eventuali aggiornamenti apportati alla risorsa originale in  AEM Assets. Per fare riferimento a una risorsa da  AEM Assets, usate l’opzione Inserisci collegato nel menu di scelta rapida.

### Risoluzione Solo Posizionamento (FPO)

Quando file di risorse di grandi dimensioni vengono inseriti  documenti InDesign da  AEM Assets tramite  collegamento risorse Adobe, gli utenti creativi devono attendere alcuni secondi dopo aver avviato l’operazione di posizionamento. Questo influisce sull’esperienza utente complessiva. Con  collegamento risorsa Adobe ora potete inserire temporaneamente un’immagine a bassa risoluzione della risorsa originale da  AEM Assets, riducendo così il tempo necessario per inserire un’immagine. Allo stesso tempo, aumenta l&#39;esperienza e la produttività complessiva dell&#39;utente. L’immagine a risoluzione inferiore viene temporaneamente inserita e, quando l’output finale è richiesto per la stampa o la pubblicazione, è necessario sostituire le rappresentazioni FPO con quelle originali. Per sostituire più immagini FPO con le rispettive immagini originali, andate a **_Windows > Pannello Collegamenti_** e scaricate le risorse originali. Dopo aver scaricato le immagini originali, scegliete Sostituisci tutti gli FPO con originali.

>[!NOTE]
>
> *Per la rappresentazione Solo posizionamento (FPO)* funziona solo per l&#39;opzione Inserisci collegato. È inoltre necessario abilitare il supporto delle rappresentazioni FPO nel flusso di lavoro  AEM Assets *Dam Update Asset* .

Le rappresentazioni FPO sono sostituti leggeri delle risorse originali. Hanno le stesse proporzioni, ma hanno dimensioni inferiori rispetto alle immagini originali. Al momento,  InDesign supporta l’importazione di rappresentazioni FPO solo per i seguenti tipi di immagini:

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

Se una rappresentazione FPO non è disponibile per una risorsa specifica in  AEM Assets, viene fatto riferimento alla risorsa originale ad alta risoluzione. Per le immagini FPO, lo stato FPO viene visualizzato nel pannello Collegamenti InDesign .



## Informazioni ’autenticazione dei collegamenti alle risorse di Adobe con  AEM Assets{#understanding-adobe-asset-link-authentication-with-aem-assets}

 funzionamento dell’autenticazione Collegamento risorse di Adobe nel contesto  Adobe  Identity Management Services (IMS) e Adobe Experience Manager Author.

![Architettura  collegamento risorse Adobe](assets/adobe-asset-link-article-understand.png)

Scarica [Architettura dei collegamenti delle risorse di Adobe](assets/adobe-asset-link-article-understand-1.png)

1. L’estensione  Collegamento risorsa Adobe richiede un’autorizzazione, tramite l’app Adobe Creative Cloud Desktop, per  servizio di gestione dell’identità Adobe (IMS) e, in caso di esito positivo, riceve un token del portatore.
2. ’estensione Collegamento risorse di Adobe si connette ad AEM Author tramite HTTP(S), incluso il token del portatore ottenuto al **passaggio 1**, utilizzando lo schema (HTTP/HTTPS), l’host e la porta forniti nel JSON delle impostazioni dell’estensione.
3. Il gestore autenticazione del portatore di AEM estrae il token del portatore dalla richiesta e lo convalida  IMS Adobe.
4. Dopo  Adobe IMS convalida il token del portatore, un utente viene creato in AEM (se non esiste già) e sincronizza i dati del profilo e del gruppo/appartenenze da   IMS. All’utente AEM viene rilasciato un token di login AEM standard, che viene inviato nuovamente all’estensione  collegamento risorsa Adobe come cookie nella risposta HTTP(S).
5. Interazioni successive (ad esempio esplorazione, ricerca, archiviazione e così via) con l’estensione Collegamento risorse di Adobe  risultano richieste HTTP(S) ad AEM Author convalidate tramite il token di accesso AEM, utilizzando il gestore di autenticazione AEM token standard.

>[!NOTE]
>
>Alla scadenza del token di accesso, i **passaggi da 1 a 5** richiameranno automaticamente, autenticando l’estensione del collegamento delle risorse del Adobe  con il token del portatore e rilasciando nuovamente un nuovo token di accesso valido.

## Risorse aggiuntive{#additional-resources}

* [Sito Web  collegamento risorse Adobe](https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html)