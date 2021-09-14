---
title: Adobe Asset Link e AEM
description: 'Le risorse Adobe Experience Manager possono essere utilizzate da designer e utenti creativi nelle loro applicazioni desktop Adobe Creative Cloud preferite. L’estensione Adobe Asset Link per Adobe Creative Cloud for enterprise estende la capacità di cercare e sfogliare, ordinare, visualizzare in anteprima, caricare risorse, estrarre, modificare, archiviare e visualizzare i metadati di AEM risorse in strumenti di Creative Cloud come Adobe XD, Photoshop, InDesign e Illustrator. '
feature: Adobe Asset Link
version: 6.4, 6.5, Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '992'
ht-degree: 2%

---


# Adobe Asset Link 3.0

Le risorse Adobe Experience Manager possono essere utilizzate da designer e utenti creativi nelle loro applicazioni desktop Adobe Creative Cloud preferite.

L’estensione Adobe Asset Link per Adobe Creative Cloud for enterprise estende la capacità di cercare e sfogliare, ordinare, visualizzare in anteprima, caricare risorse, estrarre, modificare, archiviare e visualizzare i metadati di AEM risorse nelle applicazioni Creative Cloud.

## Funzionalità di Adobe Asset Link

+ Adobe Asset Link si integra con AEM Assets e Assets Essentials.
+ Adobe Asset Link configura automaticamente la connessione ad ambienti di AEM basati su cloud (AEM Assets as a Cloud Service e Assets Essentials)
+ Adobe Asset Link è un’estensione che funziona all’interno delle applicazioni Adobe Creative Cloud:

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ Autenticazione automatica per AEM utilizzando l’Enterprise ID o il Federated ID Adobe
+ Cercare risorse digitali in AEM
+ Accedi ai dettagli dei file per le risorse che risiedono in AEM da con il pannello:
   + Miniatura 
   + Metadati di base
   + Versioni
+ Inserire, scaricare o trascinare risorse nel layout
+ Modificare le risorse estraendole dalla AEM e lavorando su di esse (WIP) all’interno del loro account Creative Cloud risorse
+ Dopo aver completato la modifica di una risorsa, AEM nuovamente una risorsa e la nuova versione verrà riflessa in AEM
+ Cercare risorse in AEM dal pannello in-app di Adobe Asset Link
+ Sfogliare raccolte e raccolte avanzate di AEM Assets direttamente dal pannello Asset Link
+ Aggiungi risorse appena create a AEM direttamente dal pannello
+ Trascinare risorse direttamente nei frame di InDesign

## Inserimento di risorse in InDesign

Adobe Asset Link fornisce il supporto per il collegamento diretto di InDesign tra Adobe Asset Link e AEM. Con il supporto per i collegamenti diretti di InDesign, puoi inserire (__Inserisci collegamenti__ o __Posiziona copia__) o trascinare risorse digitali in InDesign dal AEM tramite il pannello Asset Link di Adobe. Inoltre, introduce il rendering *For Placement Only+ (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Utilizza solo la tua Enterprise ID o Federated ID Adobe Creative Cloud. Assicurati di [configurare AEM per Adobe Asset Link](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).

Puoi inserire una risorsa nel layout di InDesign utilizzando una delle seguenti opzioni:

+ **Inserisci copia** : se incorpori una risorsa (utilizzando l’opzione Inserisci copia), inserisce una copia della risorsa originale nel layout di InDesign dopo aver scaricato i binari nel sistema locale. Adobe Asset Link non mantiene alcun collegamento tra la copia incorporata e la risorsa originale. Se la risorsa originale viene modificata in AEM, devi eliminare la risorsa incorporata dal file InDesign e incorporarla nuovamente da AEM.

+ **Inserisci collegato** : quando lavori con i documenti di InDesign, puoi fare riferimento alle risorse da AEM oltre a incorporarle direttamente (utilizzando l’opzione Inserisci copia nel menu di scelta rapida). Il riferimento alle risorse consente di collaborare con altri utenti e di incorporare eventuali aggiornamenti apportati alla risorsa originale in AEM. Per fare riferimento a una risorsa da AEM, utilizza l’opzione Posiziona collegamento nel menu di scelta rapida.

### Per immagini solo posizionamento

Quando file di risorse di grandi dimensioni vengono inseriti nei documenti di InDesign AEM utilizzando Adobe Asset Link, gli utenti creativi devono attendere alcuni secondi dopo aver avviato l’operazione di inserimento. Questo influisce sull’esperienza utente complessiva. Con Adobe Asset Link è possibile inserire temporaneamente un’immagine a bassa risoluzione della risorsa originale da AEM, riducendo il tempo necessario per inserire un’immagine. Allo stesso tempo, aumenta l&#39;esperienza utente complessiva e la produttività. L&#39;immagine a risoluzione inferiore viene temporaneamente posizionata e, quando l&#39;output finale è necessario per la stampa o la pubblicazione, è necessario sostituire le rappresentazioni FPO con quelle originali. Per sostituire più immagini FPO con le rispettive immagini originali, passa al pannello **_Windows > Collegamenti_** e scarica le risorse originali. Dopo aver scaricato le immagini originali, scegliere Sostituisci tutte le FPO con originali.

Le rappresentazioni FPO sono sostituzioni leggere delle risorse originali. Hanno le stesse proporzioni, ma sono di dimensioni inferiori rispetto alle immagini originali. Al momento, InDesign supporta l’importazione di rappresentazioni FPO solo per i seguenti tipi di immagini:

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

Se un rendering FPO non è disponibile per una risorsa specifica in AEM, viene fatto riferimento alla risorsa originale ad alta risoluzione. Per le immagini FPO, lo stato FPO viene visualizzato nel pannello Collegamenti InDesign.

## Autenticazione di Adobe Asset Link con AEM Assets

Come funziona l’autenticazione di Adobe Asset Link nel contesto di Adobe Identity Management Services (IMS) e Adobe Experience Manager Author.

![Architettura di Adobe Asset Link](assets/adobe-asset-link-article-understand.png)

1. L’estensione Adobe Asset Link invia una richiesta di autorizzazione, tramite l’app desktop Adobe Creative Cloud, ad Adobe Identity Manage Service (IMS) e, dopo il successo, riceve un token portatore.
1. L’estensione Adobe Asset Link si connette ad AEM Author tramite HTTP(S), incluso il token portatore ottenuto in **Passaggio 1**, utilizzando lo schema (HTTP/HTTPS), l’host e la porta forniti nelle impostazioni JSON dell’estensione.
1. AEM’s Bearer Authentication Handler estrae il token portatore dalla richiesta e lo convalida con Adobe IMS.
1. Una volta che Adobe IMS convalida il token portatore, un utente viene creato in AEM (se non esiste già) e sincronizza i dati di profilo e gruppo/appartenenze da Adobe IMS. All’utente AEM viene rilasciato un token di accesso AEM standard, che viene inviato nuovamente all’estensione Adobe Asset Link come cookie nella risposta HTTP(S).
1. Interazioni successive (ad es. esplorazione, ricerca, archiviazione/estrazione di risorse, ecc.) con l’estensione Adobe Asset Link si ottengono richieste HTTP(S) ad AEM Author che vengono convalidate utilizzando il token di accesso AEM, utilizzando lo standard AEM Token Authentication Handler.

>[!NOTE]
>
>Alla scadenza del token di accesso, **Passaggi 1-5** richiamerà automaticamente, autenticando l’estensione Adobe Asset Link utilizzando il token portatore e rilascerà un nuovo token di accesso valido.

## Altro materiale di riferimento

+ [Sito web Adobe Asset Link](https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html)
