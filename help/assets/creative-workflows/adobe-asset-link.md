---
title: Adobe Asset Link e AEM
description: Le risorse Adobe Experience Manager possono essere utilizzate da designer e utenti creativi nelle applicazioni desktop Adobe Creative Cloud preferite. L’estensione Adobe Asset Link per Adobe Creative Cloud for enterprise estende la funzionalità di ricerca e navigazione, ordinamento, anteprima, caricamento di risorse, estrazione, modifica, archiviazione e visualizzazione dei metadati delle risorse AEM in strumenti Creative Cloud come Adobe XD, Photoshop, InDesign e Illustrator.
feature: Adobe Asset Link
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
jira: KT-8413, KT-3707
last-substantial-update: 2022-06-25T00:00:00Z
doc-type: Feature Video
exl-id: 6c49f8c2-f468-4b29-b7b6-029c8ab39ce9
duration: 1027
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1039'
ht-degree: 1%

---

# Adobe Asset Link 3.0

Le risorse Adobe Experience Manager possono essere utilizzate da designer e utenti creativi nelle applicazioni desktop Adobe Creative Cloud preferite.

L’estensione Adobe Asset Link per Adobe Creative Cloud for enterprise estende la funzionalità di ricerca e navigazione, ordinamento, anteprima, caricamento di risorse, estrazione, modifica, archiviazione e visualizzazione dei metadati delle risorse AEM nelle applicazioni Creative Cloud.

>[!TIP]
>
> Ulteriori informazioni su come il [Programma di formazione Adobe XD Premium](https://helpx.adobe.com/it/support/xd.html) può aiutarti a integrare Asset Link con il tuo flusso di lavoro Adobe Experience Manager.

## Flussi di lavoro creativi di Adobe Asset Link e AEM

Il video seguente illustra un flusso di lavoro comune utilizzato dai creativi che lavorano nelle applicazioni Adobe Creative Cloud e si integrano direttamente con AEM utilizzando Adobe Asset Link.

>[!VIDEO](https://video.tv.adobe.com/v/3418441?quality=12&learn=on&captions=ita)

## Funzionalità di Adobe Asset Link

+ Adobe Asset Link si integra con AEM Assets e Assets Essentials.
+ Adobe Asset Link configura automaticamente la connessione agli ambienti AEM basati su cloud (AEM Assets as a Cloud Service e Assets Essentials)
+ Adobe Asset Link è un’estensione che funziona nelle applicazioni Adobe Creative Cloud:

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ Autenticazione automatica per AEM con Adobe Enterprise ID o Federated ID
+ Sfogliare e cercare risorse digitali in AEM
+ Accedi ai dettagli del file per le risorse residenti in AEM da con il pannello:
   + Miniatura 
   + Metadati di base
   + Versioni
+ Inserire, scaricare o trascinare le risorse nel layout
+ Modificare le risorse estraendole da AEM e lavorando su di esse (WIP) all’interno del loro account Creative Cloud Assets
+ Archivia di nuovo una risorsa in AEM dopo averla modificata completamente, e la nuova versione si riflette in AEM
+ Cercare risorse in AEM dal pannello in-app Adobe Asset Link
+ Sfogliare raccolte avanzate e raccolte AEM Assets direttamente dal pannello Asset Link
+ Aggiungere le nuove risorse create ad AEM direttamente dal pannello
+ Trascinare le risorse direttamente nei frame di InDesign

## Posizionamento di risorse in InDesign

Adobe Asset Link fornisce il supporto per collegamenti diretti tra InDesign e Adobe AEM Asset Link. Con il supporto per i collegamenti diretti di InDesign, puoi inserire (__Inserisci collegamento__ o __Inserisci copia__) o trascinare risorse digitali in InDesign da AEM tramite il pannello Adobe Asset Link. Inoltre, introduce la rappresentazione *For Placement Only+ (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/37238?quality=12&learn=on&captions=ita)

>[!NOTE]
>
>Utilizza solo il tuo Adobe Creative Cloud Enterprise ID o Federated ID. Assicurati di [configurare AEM per Adobe Asset Link](https://helpx.adobe.com/it/enterprise/using/adobe-asset-link.html).

Puoi inserire una risorsa nel layout di InDesign utilizzando una delle opzioni seguenti:

+ **Inserisci copia** - Quando si incorpora una risorsa (utilizzando l&#39;opzione Inserisci copia), nel layout InDesign viene inserita una copia della risorsa originale dopo aver scaricato i file binari nel sistema locale. Adobe Asset Link non mantiene alcun collegamento tra la copia incorporata e la risorsa originale. Se la risorsa originale viene modificata in AEM, devi eliminare la risorsa incorporata dal file InDesign e reincorporarla da AEM.

+ **Inserisci con collegamento**: quando si lavora con documenti InDesign, è possibile fare riferimento alle risorse da AEM oltre a incorporarle direttamente (utilizzando l&#39;opzione Inserisci copia nel menu di scelta rapida). Le risorse di riferimento ti consentono di collaborare con altri utenti e di incorporare in AEM eventuali aggiornamenti apportati alla risorsa originale. Per fare riferimento a una risorsa da AEM, utilizza l’opzione Inserisci collegamento nel menu di scelta rapida.

### Immagini solo posizionamento

Quando si inseriscono file di risorse di grandi dimensioni in InDesign Documents da AEM utilizzando Adobe Asset Link, gli utenti creativi devono attendere alcuni secondi dopo aver avviato l’operazione di posizionamento. Questo influisce sull’esperienza utente complessiva. Con Adobe Asset Link è possibile inserire temporaneamente un’immagine a bassa risoluzione della risorsa originale da AEM, riducendo in tal modo il tempo necessario per inserire un’immagine. Allo stesso tempo, aumenta l’esperienza utente e la produttività complessive. L&#39;immagine a risoluzione inferiore viene inserita temporaneamente e quando è necessario l&#39;output finale per la stampa o la pubblicazione, è necessario sostituire le copie trasformate FPO con quelle originali. Se si desidera sostituire più immagini FPO con le rispettive immagini originali, passare al pannello **_Windows > Collegamenti_** e scaricare le risorse originali. Dopo aver scaricato le immagini originali, scegliere Sostituisci tutte le foto con originali.

Le rappresentazioni FPO sono sostituti leggeri delle risorse originali. Hanno le stesse proporzioni, ma sono di dimensioni inferiori rispetto alle immagini originali. Attualmente, InDesign supporta l’importazione di rappresentazioni FPO solo per i seguenti tipi di immagini:

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

Se per una specifica risorsa in AEM non è disponibile una rappresentazione FPO, viene fatto riferimento alla risorsa originale ad alta risoluzione. Per le immagini FPO, lo stato FPO viene visualizzato nel pannello Collegamenti di InDesign.

## Autenticazione di Adobe Asset Link con AEM Assets

Funzionamento dell’autenticazione Adobe Asset Link nel contesto di Adobe Identity Management Services (IMS) e Adobe Experience Manager Author.

![Architettura di Adobe Asset Link](assets/adobe-asset-link-article-understand.png)

1. L’estensione Adobe Asset Link effettua una richiesta di autorizzazione, tramite l’app desktop Adobe Creative Cloud, per Adobe Identity Manage Service (IMS) e, in caso di esito positivo, riceve un token Bearer.
1. L&#39;estensione Adobe Asset Link si connette ad AEM Author tramite HTTP(S), incluso il token Bearer ottenuto nel **passaggio 1**, utilizzando lo schema (HTTP/HTTPS), l&#39;host e la porta forniti nel JSON delle impostazioni dell&#39;estensione.
1. Il gestore di autenticazione Bearer di AEM estrae il token Bearer dalla richiesta e lo convalida con Adobe IMS.
1. Dopo che Adobe IMS convalida il token Bearer, viene creato un utente in AEM (se non esiste già) e sincronizza i dati di profilo e gruppi/appartenenze da Adobe IMS. All’utente di AEM viene rilasciato un token di accesso AEM standard, che viene inviato nuovamente all’estensione Adobe Asset Link come cookie nella risposta HTTP(S).
1. Interazioni successive (ossia. L’esplorazione, la ricerca, il check-in/check-out di risorse e così via) con l’estensione Adobe Asset Link genera richieste HTTP(S) per AEM Author che vengono convalidate utilizzando il token di accesso di AEM, utilizzando il gestore di autenticazione token di AEM standard.

>[!NOTE]
>
>Alla scadenza del token di accesso, **I passaggi 1-5** verranno richiamati automaticamente, autenticando l’estensione Adobe Asset Link tramite il token Bearer e riemette un nuovo token di accesso valido.

## Risorse aggiuntive

+ [Sito Web Adobe Asset Link](https://www.adobe.com/it/creativecloud/business/enterprise/adobe-asset-link.html)
