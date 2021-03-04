---
title: Utilizzo dell’estensione Adobe Asset Link con AEM Assets
description: 'Le risorse di Adobe Experience Manager possono ora essere utilizzate da designer e utenti creativi nelle loro applicazioni desktop Adobe Creative Cloud preferite. L’estensione Adobe Asset Link per Adobe Creative Cloud Enterprise estende la capacità di cercare e sfogliare, ordinare, visualizzare in anteprima, caricare risorse, estrarre, modificare, archiviare e visualizzare i metadati delle risorse AEM in strumenti Creative Cloud come Adobe Photoshop, InDesign e Illustrator. '
feature: Adobe Asset Link
version: 6.4, 6.5
topic: Gestione dei contenuti
role: Professionista
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1102'
ht-degree: 1%

---


# Utilizzo dell’estensione Adobe Asset Link con AEM Assets{#using-adobe-asset-link-extension-with-aem-assets}

Le risorse di Adobe Experience Manager possono ora essere utilizzate da designer e utenti creativi nelle loro applicazioni desktop Adobe Creative Cloud preferite. L’estensione Adobe Asset Link per Adobe Creative Cloud Enterprise estende la capacità di cercare e sfogliare, ordinare, visualizzare in anteprima, caricare risorse, estrarre, modificare, archiviare e visualizzare i metadati delle risorse AEM in strumenti Creative Cloud come Adobe Photoshop, InDesign e Illustrator.


## Adobe Asset Link 1.1

Adobe Asset Link v1.1 fornisce ora il supporto per i collegamenti diretti in InDesign tra Adobe Asset Link e AEM Assets. Con il supporto per i collegamenti diretti in InDesign, ora puoi inserire (inserire collegamenti o inserire una copia) o trascinare risorse digitali in InDesign da AEM Assets tramite il pannello Adobe Asset Link. Inoltre, introduce il rendering *Solo posizionamento* (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Utilizza solo il tuo Adobe Creative Cloud Enterprise ID o Federated ID. Assicurati di [configurare AEM per Adobe Asset Link](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).


### Funzionalità di Adobe Asset Link

* Adobe Asset Link è un’estensione che funziona in PS, AI, ID e fornisce accesso diretto alle risorse digitali che risiedono in AEM Assets
* I creativi verranno registrati in AEM automaticamente utilizzando il proprio Adobe IMS Enterprise ID o Federated ID
* I creativi possono sfogliare le risorse digitali che risiedono in AEM Assets, oltre a effettuare ricerche in AEM Assets e Creative Cloud Assets
* I creativi possono accedere ai dettagli dei file per le risorse residenti in AEM Assets; miniature, metadati di base e versioni dal pannello
* I creativi possono inserire, scaricare o trascinare risorse nel loro layout
* I creativi possono modificare le risorse estraendole da AEM Assets e lavorando su di esse (WIP) nel loro account Creative Cloud Assets
* I creativi possono ricontrollare una risorsa in AEM Assets dopo che hanno finito di modificarla, e la nuova versione verrà riflessa in AEM Assets
* Supporta le app desktop InDesign, Photoshop e Illustrator di Creative Cloud 2020, 2019 e 2018
* Un utente può eseguire una ricerca delle risorse dal pannello in-app di Adobe Asset Link e ordinarle in base alle dimensioni, all’ordine alfabetico e alla rilevanza
* Gli utenti possono accedere alle raccolte e alle raccolte avanzate di AEM Assets direttamente dal pannello Asset Link
* Aggiungere risorse appena create ad AEM Assets direttamente dal pannello
* Un utente può trascinare e rilasciare risorse direttamente nelle cornici di InDesign

### Inserimento di AEM Assets in InDesign

Per inserire una risorsa nel layout di InDesign utilizzando una delle seguenti opzioni:

* **Inserisci copia** : se incorpori una risorsa (utilizzando l’opzione Inserisci copia), inserisce una copia della risorsa originale nel layout InDesign dopo aver scaricato i binari nel sistema locale. Adobe Asset Link non mantiene alcun collegamento tra la copia incorporata e la risorsa originale. Se la risorsa originale viene modificata in AEM Assets, devi eliminare la risorsa incorporata dal file InDesign e incorporarla nuovamente da AEM Assets.

* **Inserisci collegato** : quando lavori con documenti InDesign, ora puoi fare riferimento alle risorse da AEM Assets oltre a incorporarle direttamente (utilizzando l’opzione Inserisci copia nel menu di scelta rapida). Il riferimento alle risorse consente di collaborare con altri utenti e di incorporare eventuali aggiornamenti apportati alla risorsa originale in AEM Assets. Per fare riferimento a una risorsa da AEM Assets, utilizza l’opzione Inserisci collegamento nel menu di scelta rapida.

### Risoluzione Solo posizionamento (FPO)

Quando file di risorse di grandi dimensioni vengono inseriti nei documenti di InDesign da AEM Assets tramite Adobe Asset Link, gli utenti creativi devono attendere alcuni secondi dopo aver avviato l’operazione di posizionamento. Questo influisce sull’esperienza utente complessiva. Con Adobe Asset Link è ora possibile inserire temporaneamente un’immagine a bassa risoluzione della risorsa originale da AEM Assets, riducendo il tempo necessario per inserire un’immagine. Allo stesso tempo, aumenta l&#39;esperienza utente complessiva e la produttività. L&#39;immagine a risoluzione inferiore viene temporaneamente posizionata e, quando l&#39;output finale è necessario per la stampa o la pubblicazione, è necessario sostituire le rappresentazioni FPO con quelle originali. Per sostituire più immagini FPO con le rispettive immagini originali, passa al pannello **_Windows > Collegamenti_** e scarica le risorse originali. Dopo aver scaricato le immagini originali, scegliere Sostituisci tutte le FPO con originali.

>[!NOTE]
>
> *Il rendering Solo posizionamento (FPO, For Placement Only)*  funziona solo per l&#39;opzione Inserisci collegato. Abilita inoltre il supporto del rendering FPO all’interno del flusso di lavoro AEM Assets *Dam Update Asset* .

Le rappresentazioni FPO sono sostituzioni leggere delle risorse originali. Hanno le stesse proporzioni, ma sono di dimensioni inferiori rispetto alle immagini originali. Attualmente, InDesign supporta l’importazione di rappresentazioni FPO solo per i seguenti tipi di immagine:

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

Se un rendering FPO non è disponibile per una risorsa specifica in AEM Assets, viene fatto riferimento alla risorsa originale ad alta risoluzione. Per le immagini FPO, lo stato FPO viene visualizzato nel pannello Collegamenti di InDesign.

## Informazioni sull’autenticazione di Adobe Asset Link con AEM Assets{#understanding-adobe-asset-link-authentication-with-aem-assets}

Come funziona l’autenticazione di Adobe Asset Link nel contesto di Adobe Identity Management Services (IMS) e Adobe Experience Manager Author.

![Architettura di Adobe Asset Link](assets/adobe-asset-link-article-understand.png)

Scarica [Architettura di Adobe Asset Link](assets/adobe-asset-link-article-understand-1.png)

1. L’estensione Adobe Asset Link invia una richiesta di autorizzazione, tramite l’app desktop Adobe Creative Cloud, ad Adobe Identity Manage Service (IMS) e, in seguito al successo, riceve un token portatore.
2. L’estensione Adobe Asset Link si collega ad AEM Author tramite HTTP(S), incluso il token portatore ottenuto in **Passaggio 1**, utilizzando lo schema (HTTP/HTTPS), l’host e la porta forniti nelle impostazioni JSON dell’estensione.
3. Il gestore di autenticazione al portatore di AEM estrae il token portatore dalla richiesta e lo convalida rispetto ad Adobe IMS.
4. Una volta che Adobe IMS convalida il token portatore, un utente viene creato in AEM (se non esiste già) e sincronizza i dati di profilo e gruppo/iscrizione da Adobe IMS. All’utente AEM viene rilasciato un token di accesso AEM standard, che viene inviato nuovamente all’estensione Adobe Asset Link come cookie nella risposta HTTP(S).
5. Interazioni successive (ad es. esplorazione, ricerca, archiviazione/estrazione di risorse, ecc.) con l’estensione Adobe Asset Link si ottengono richieste HTTP(S) ad AEM Author che vengono convalidate utilizzando il token di accesso AEM, utilizzando il gestore di autenticazione standard AEM Token.

>[!NOTE]
>
>Alla scadenza del token di accesso, **Passaggi 1-5** richiamerà automaticamente, autenticando l’estensione Adobe Asset Link utilizzando il token portatore e rilascerà un nuovo token di accesso valido.

## Risorse aggiuntive{#additional-resources}

* [Sito web Adobe Asset Link](https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html)