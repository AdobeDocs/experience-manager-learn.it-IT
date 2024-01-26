---
title: Adobe Asset Link e AEM
description: Le risorse Adobe Experience Manager possono essere utilizzate da designer e utenti creativi nelle applicazioni desktop Adobe Creative Cloud preferite. L’estensione Adobe Asset Link per Adobe Creative Cloud for enterprise estende la funzionalità di ricerca e navigazione, ordinamento, anteprima, caricamento di risorse, estrazione, modifica, archiviazione e visualizzazione dei metadati delle risorse AEM negli strumenti Creative Cloud come Adobe XD, Photoshop, InDesign e Illustrator.
feature: Adobe Asset Link
version: 6.4, 6.5, Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
jira: KT-8413, KT-3707
last-substantial-update: 2022-06-25T00:00:00Z
doc-type: Feature Video
exl-id: 6c49f8c2-f468-4b29-b7b6-029c8ab39ce9
duration: 1082
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1039'
ht-degree: 1%

---

# Adobe Asset Link 3.0

Le risorse Adobe Experience Manager possono essere utilizzate da designer e utenti creativi nelle applicazioni desktop Adobe Creative Cloud preferite.

L’estensione Adobe Asset Link per Adobe Creative Cloud for enterprise estende la funzionalità di ricerca e navigazione, ordinamento, anteprima, caricamento di risorse, estrazione, modifica, archiviazione e visualizzazione dei metadati delle risorse AEM nelle applicazioni Creative Cloud.

>[!TIP]
>
> Ulteriori informazioni su come [Programma di formazione Adobe XD Premium](https://helpx.adobe.com/support/xd.html) può aiutarti a integrare Asset Link con il tuo flusso di lavoro Adobe Experience Manager.

## Flussi di lavoro di Adobe Asset Link e AEM creative

Il video seguente illustra un flusso di lavoro comune utilizzato dai creativi che lavorano nelle applicazioni Adobe Creative Cloud e si integrano direttamente con l’AEM utilizzando Adobe Asset Link.

>[!VIDEO](https://video.tv.adobe.com/v/335927?quality=12&learn=on)

## Adobe di funzionalità Asset Link

+ Adobe Asset Link si integra con AEM Assets e Assets Essentials.
+ Adobe Asset Link configura automaticamente la connessione agli ambienti AEM basati su cloud (AEM Assets as a Cloud Service e Assets Essentials)
+ Adobe Asset Link è un’estensione che funziona nelle applicazioni Adobe Creative Cloud:

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ Autenticazione automatica per AEM tramite Enterprise ID o Federated ID Adobe
+ Sfogliare e cercare risorse digitali nell’AEM
+ Accedi ai dettagli del file per le risorse residenti in AEM da con il pannello:
   + Miniatura 
   + Metadati di base
   + Versioni
+ Inserire, scaricare o trascinare le risorse nel layout
+ Modificare le risorse estraendole dall&#39;AEM e lavorando su di esse (WIP) all&#39;interno del conto Creative Cloud delle risorse
+ Archivia una risorsa di nuovo in AEM dopo averla modificata, e la nuova versione si riflette nell’AEM
+ Cercare risorse in AEM dal pannello in-app Adobe Asset Link
+ Sfogliare raccolte avanzate e raccolte AEM Assets direttamente dal pannello Asset Link
+ Aggiungere le nuove risorse create all’AEM direttamente dal pannello
+ Trascinare le risorse direttamente nei frame InDesign

## Posizionamento di risorse in InDesign

Adobe Asset Link fornisce il supporto InDesign per i collegamenti diretti tra Adobe Asset Link e AEM. Grazie al supporto dei collegamenti diretti di InDesign, è possibile inserire (__Inserisci con collegamento__ o __Inserisci copia__) o trascina le risorse digitali in InDesign dall’AEM tramite il pannello Adobe Asset Link. Inoltre, introduce la rappresentazione *For Placement Only+ (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/28988?quality=12&learn=on)

>[!NOTE]
>
>Utilizza solo l’Enterprise ID o il Federated ID Adobe Creative Cloud. Assicurati di [configurare AEM per Adobe Asset Link](https://helpx.adobe.com/it/enterprise/using/adobe-asset-link.html).

Puoi inserire una risorsa nel layout InDesign utilizzando una delle opzioni seguenti:

+ **Inserisci copia** : quando si incorpora una risorsa (utilizzando l’opzione Inserisci copia), nel layout InDesign viene inserita una copia della risorsa originale dopo aver scaricato i dati binari nel sistema locale. Adobe Asset Link non mantiene alcun collegamento tra la copia incorporata e la risorsa originale. Se la risorsa originale viene modificata in AEM, devi eliminare la risorsa incorporata dal file InDesign e reincorporarla dall’AEM.

+ **Inserisci con collegamento** - Quando si lavora con documenti InDesign, è possibile fare riferimento alle risorse dell’AEM oltre a incorporarle direttamente (utilizzando l’opzione Inserisci copia nel menu di scelta rapida). Le risorse di riferimento ti consentono di collaborare con altri utenti e di incorporare in AEM eventuali aggiornamenti apportati alla risorsa originale. Per fare riferimento a una risorsa dell’AEM, utilizza l’opzione Inserisci collegamento nel menu di scelta rapida.

### Immagini solo posizionamento

Quando file di risorse di grandi dimensioni vengono inseriti in documenti InDesign dall’AEM utilizzando Adobe Asset Link, gli utenti creativi devono attendere alcuni secondi dopo aver avviato l’operazione di posizionamento. Questo influisce sull’esperienza utente complessiva. Con Adobe Asset Link è possibile inserire temporaneamente un&#39;immagine a bassa risoluzione della risorsa originale dall&#39;AEM, riducendo in tal modo il tempo necessario per inserire un&#39;immagine. Allo stesso tempo, aumenta l’esperienza utente e la produttività complessive. L&#39;immagine a risoluzione inferiore viene inserita temporaneamente e quando è necessario l&#39;output finale per la stampa o la pubblicazione, è necessario sostituire le copie trasformate FPO con quelle originali. Se si desidera sostituire più immagini FPO con le rispettive immagini originali, passare a **_Windows > Collegamenti_** e quindi scarica le risorse originali. Dopo aver scaricato le immagini originali, scegliere Sostituisci tutte le foto con originali.

Le rappresentazioni FPO sono sostituti leggeri delle risorse originali. Hanno le stesse proporzioni, ma sono di dimensioni inferiori rispetto alle immagini originali. Attualmente, InDesign supporta l’importazione di rappresentazioni FPO solo per i seguenti tipi di immagini:

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

Se per una specifica risorsa in AEM non è disponibile una rappresentazione FPO, viene fatto riferimento alla risorsa originale ad alta risoluzione. Per le immagini FPO, lo stato FPO viene visualizzato nel pannello Collegamenti InDesign.

## Adobe dell’autenticazione di Asset Link con AEM Assets

Funzionamento dell’autenticazione Adobe Adobe Asset Link nel contesto di Identity Management Services (IMS) e Adobe Experience Manager Author.

![Architettura Adobe Asset Link](assets/adobe-asset-link-article-understand.png)

1. L’estensione Adobe Asset Link effettua una richiesta di autorizzazione, tramite l’app desktop Adobe Creative Cloud, per Adobe Identity Manage Service (IMS) e, in caso di esito positivo, riceve un token Bearer.
1. L’estensione Adobe Asset Link si connette all’istanza di authoring AEM tramite HTTP(S), incluso il token Bearer ottenuto in **Passaggio 1**, utilizzando lo schema (HTTP/HTTPS), l’host e la porta forniti nel JSON delle impostazioni dell’estensione.
1. Il gestore di autenticazione Bearer dell’AEM estrae il token Bearer dalla richiesta e lo convalida con Adobe IMS.
1. Dopo che Adobe IMS convalida il token Bearer, viene creato un utente in AEM (se non esiste già) e sincronizza i dati di profilo e gruppo/appartenenze da Adobe IMS. All’utente AEM viene rilasciato un token di accesso AEM standard, che viene rimandato all’estensione Adobe Asset Link come cookie nella risposta HTTP(S).
1. Interazioni successive (ossia. esplorazione, ricerca, archiviazione/estrazione di risorse e così via) Con l’estensione Adobe Asset Link, si ottengono richieste HTTP(S) per l’authoring AEM convalidate utilizzando il token di accesso AEM, utilizzando il gestore di autenticazione token AEM standard.

>[!NOTE]
>
>Alla scadenza del token di accesso, **Passaggi 1-5** richiamerà automaticamente, autenticando l’estensione Adobe Asset Link tramite il token Bearer e riemetterà un nuovo token di accesso valido.

## Risorse aggiuntive

+ [Adobe del sito web Asset Link](https://www.adobe.com/it/creativecloud/business/enterprise/adobe-asset-link.html)
