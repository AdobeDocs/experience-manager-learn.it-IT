---
title: Adobe Asset Link e AEM
description: Le risorse di Adobe Experience Manager possono essere utilizzate da designer e utenti creativi nelle applicazioni desktop Adobe Creative Cloud preferite. L’estensione Adobe Asset Link per Adobe Creative Cloud for enterprise estende la funzionalità di ricerca e navigazione, ordinamento, anteprima, caricamento di risorse, consultazione modifica, archiviazione e visualizzazione dei metadati delle risorse AEM in strumenti Creative Cloud come Adobe XD, Photoshop, InDesign e Illustrator.
feature: Adobe Asset Link
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
duration: 673
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '983'
ht-degree: 100%

---


# Adobe Asset Link 3.0

Le risorse di Adobe Experience Manager possono essere utilizzate da designer e utenti creativi nelle applicazioni desktop Adobe Creative Cloud preferite.

L’estensione Adobe Asset Link per Adobe Creative Cloud for enterprise estende la funzionalità di ricerca e navigazione, ordinamento, anteprima, caricamento di risorse, consultazione, modifica, archiviazione e visualizzazione dei metadati delle risorse AEM nelle applicazioni Creative Cloud.

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
+ Accedi ai dettagli del file per le risorse che si trovano in AEM da con il pannello:
   + Miniatura 
   + Metadati di base
   + Versioni
+ Inserire, scaricare o trascinare le risorse nel layout
+ Modificare le risorse estraendole da AEM e lavorando su di esse (WIP) all’interno del rispettivo account Creative Cloud Assets
+ Verificare una risorsa in AEM dopo averla modificata e che la nuova versione sia riflessa in AEM
+ Cercare risorse in AEM dal pannello in-app Adobe Asset Link
+ Sfogliare raccolte avanzate e raccolte AEM Assets direttamente dal pannello Asset Link
+ Aggiungere le nuove risorse create in AEM direttamente dal pannello
+ Trascinare le risorse direttamente nei riquadri di InDesign

## Posizionamento di risorse in InDesign

Adobe Asset Link fornisce il supporto per collegamenti diretti tra InDesign e Adobe AEM Asset Link. Con il supporto per i collegamenti diretti di InDesign, puoi inserire (__Inserisci collegato__ o __Inserisci copia__) o trascinare risorse digitali in InDesign da AEM tramite il pannello Adobe Asset Link. Inoltre, introduce la rappresentazione *Solo per posizionamento+ (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/28988?quality=12&learn=on)

>[!NOTE]
>
>Utilizza solo il tuo Adobe Creative Cloud Enterprise ID o Federated ID. Assicurati di [configurare AEM per Adobe Asset Link](https://helpx.adobe.com/it/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).

Puoi inserire una risorsa nel layout di InDesign utilizzando una delle opzioni seguenti:

+ **Inserisci copia**: quando viene incorporata una risorsa (utilizzando l’opzione Inserisci copia), nel layout InDesign viene inserita una copia della risorsa originale dopo aver scaricato i file binari nel sistema locale. Adobe Asset Link non mantiene alcun collegamento tra la copia incorporata e la risorsa originale. Se la risorsa originale viene modificata in AEM, devi eliminare la risorsa incorporata dal file InDesign e incorporarla nuovamente da AEM.

+ **Inserisci collegato**: quando si utilizzano i documenti InDesign, è possibile fare riferimento alle risorse da AEM oltre a incorporarle direttamente (utilizzando l’opzione Inserisci copia nel menu di scelta rapida). Le risorse di riferimento ti consentono di collaborare con altri utenti e di incorporare in AEM eventuali aggiornamenti apportati alla risorsa originale. Per fare riferimento a una risorsa da AEM, utilizza l’opzione Inserisci collegato nel menu di scelta rapida.

### Immagini solo per posizionamento

Quando si inseriscono file di risorse di grandi dimensioni nei documenti InDesign da AEM utilizzando Adobe Asset Link, gli utenti creativi devono attendere alcuni secondi dopo aver avviato l’operazione di posizionamento. Questo influisce sull’esperienza utente complessiva. Con Adobe Asset Link è possibile inserire temporaneamente un’immagine a bassa risoluzione della risorsa originale da AEM, riducendo in tal modo il tempo necessario per inserire un’immagine. Allo stesso tempo, migliora l’esperienza utente e la produttività complessive. L’immagine a risoluzione inferiore viene inserita temporaneamente e, quando è necessario l’output finale per la stampa o la pubblicazione, è necessario sostituire le rappresentazioni FPO con quelle originali. Se desideri sostituire più immagini FPO con le rispettive immagini originali, passa al pannello **_Windows > Collegamenti_** e scarica le risorse originali. Dopo aver scaricato le immagini originali, scegli Sostituisci tutte le FPO con originali.

Le rappresentazioni FPO sono sostituti semplici delle risorse originali. Hanno le stesse proporzioni, ma sono di dimensioni inferiori rispetto alle immagini originali. Attualmente, InDesign supporta l’importazione di rappresentazioni FPO solo per i seguenti tipi di immagini:

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

Se per una specifica risorsa in AEM non è disponibile una rappresentazione FPO, viene fatto riferimento alla risorsa originale ad alta risoluzione. Per le immagini FPO, lo stato FPO viene visualizzato nel pannello Collegamenti di InDesign.

## Autenticazione di Adobe Asset Link con AEM Assets

Funzionamento dell’autenticazione Adobe Asset Link nel contesto di Adobe Identity Management Services (IMS) e l’authoring di Adobe Experience Manager.

![Architettura Adobe Asset Link](assets/adobe-asset-link-article-understand.png)

1. L’estensione Adobe Asset Link effettua una richiesta di autorizzazione, tramite l’app desktop Adobe Creative Cloud, per Adobe Identity Manage Service (IMS) e, in caso di esito positivo, riceve un token di connessione.
1. L’estensione Adobe Asset Link si connette all’authoring AEM tramite HTTP(S), incluso il token di connessione ottenuto nel **Passaggio 1**, utilizzando lo schema (HTTP/HTTPS), l’host e la porta forniti nel JSON delle impostazioni dell’estensione.
1. Il gestore di autenticazione di connessione di AEM estrae il token di connessione dalla richiesta e lo convalida con Adobe IMS.
1. Una volta che Adobe IMS ha convalidato il token di connessione, un utente viene creato in AEM (se non esiste già) e i dati di profilo e gruppi/appartenenze vengono sincronizzati da Adobe IMS. All’utente di AEM viene rilasciato un token di accesso AEM standard, che viene inviato nuovamente all’estensione Adobe Asset Link come cookie nella risposta HTTP(S).
1. Interazioni successive (come sfogliare, ricercare, verifica/consultazione risorse e così via) con l’estensione Adobe Asset Link determinano richieste HTTP(S) per l’authoring AEM che vengono convalidate tramite il token di accesso di AEM, utilizzando il gestore di autenticazione token di AEM standard.

>[!NOTE]
>
>Alla scadenza del token di accesso, i **passaggi 1-5** verranno richiamati automaticamente, autenticando l’estensione Adobe Asset Link tramite il token di connessione e questo rilascia nuovamente un token di accesso valido.

## Risorse aggiuntive

+ [Sito web Adobe Asset Link](https://www.adobe.com/it/creativecloud/business/enterprise/adobe-asset-link.html)
