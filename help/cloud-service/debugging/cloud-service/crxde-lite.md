---
title: CRXDE Lite
description: 'CRXDE Lite è uno strumento classico ma potente per il debug dei AEM come ambienti per sviluppatori Cloud Service. CRXDE Lite fornisce una suite di funzionalità che facilita il debug dall''ispezione di tutte le risorse e proprietà, dalla manipolazione delle parti modificabili del JCR e dalle autorizzazioni di indagine. '
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
translation-type: tm+mt
source-git-commit: 10d0970c1b0debfec7cf94cc106bcae414fb55f6
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---


# Debug AEM come Cloud Service con CRXDE Lite

CRXDE Lite è __ONLY__ disponibile su AEM come ambienti di sviluppo Cloud Service (così come l&#39;SDK AEM locale).

## Accesso ai CRXDE Lite in AEM Author

CRXDE Lite __è accessibile solo__ in AEM come ambiente di sviluppo Cloud Service ed è __non__ disponibile in ambienti Stage o Produzione.

Per accedere al CRXDE Lite su AEM Author:

1. Accedete al AEM come servizio di Cloud Service AEM Author.
1. Passa a Strumenti > Generale > CRXDE Lite

Questo aprirà il CRXDE Lite utilizzando le credenziali e le autorizzazioni utilizzate per accedere ad AEM Author.

## Debug del contenuto

CRXDE Lite fornisce l&#39;accesso diretto al JCR. Il contenuto visibile tramite CRXDE Lite è limitato dalle autorizzazioni concesse all&#39;utente, il che significa che potrebbe non essere possibile visualizzare o modificare tutto nel JCR a seconda dell&#39;accesso.

Tenere presente che `/apps`, `/libs` e `/oak:index` sono immutabili, il che significa che non possono essere modificati in fase di esecuzione da alcun utente. Tali posizioni nel JCR possono essere modificate solo tramite distribuzioni di codice.

+ La struttura JCR viene esplorata e manipolata utilizzando il riquadro di navigazione a sinistra
+ Quando si seleziona un nodo nel riquadro di navigazione a sinistra, vengono visualizzate le proprietà del nodo nel riquadro inferiore.
   + Le proprietà possono essere aggiunte, rimosse o modificate dal riquadro
+ Facendo doppio clic sul nodo di un file nella barra di navigazione a sinistra, si apre il contenuto del file nel riquadro in alto a destra
+ Toccate il pulsante Salva tutto in alto a sinistra per mantenere le modifiche, oppure la freccia in basso accanto a Salva tutto per ripristinare le modifiche non salvate.

![CRXDE Lite - Debugging del contenuto](./assets/crxde-lite/debugging-content.png)

È necessario apportare con cautela modifiche ai contenuti modificabili in fase di esecuzione in AEM come ambienti di sviluppo Cloud Service tramite CRXDE Lite.
Qualsiasi modifica apportata direttamente a AEM tramite CRXDE Lite può essere difficile da monitorare e governare. Se necessario, accertatevi che le modifiche effettuate tramite CRXDE Lite tornino ai pacchetti di contenuto modificabile del progetto AEM (`ui.content`) e si impegnino in Git, al fine di garantire la risoluzione del problema. Idealmente, tutte le modifiche al contenuto dell&#39;applicazione derivano dalla base di codice e fluiscono in AEM tramite distribuzioni, anziché apportare modifiche direttamente al AEM tramite CRXDE Lite.

### Debug dei controlli di accesso

CRXDE Lite fornisce un modo per testare e valutare il controllo di accesso su un nodo specifico per un utente o un gruppo specifico (alias entità).

Per accedere alla console Test Access Control in CRXDE Lite, passare a:

+ CRXDE Lite > Strumenti > Controllo accesso di prova ...

![CRXDE Lite - Controllo dell&#39;accesso ai test](./assets/crxde-lite/permissions__test-access-control.png)

1. Utilizzando il campo Percorso, selezionare un percorso JCR da valutare
1. Utilizzando il campo Principal, selezionate l’utente o il gruppo con cui valutare il percorso
1. Toccate il pulsante Test

Di seguito sono riportati i risultati:

+ __Pathrema__ ribadisce il percorso che è stato valutato
+ ____ Principalribadisce all&#39;utente o al gruppo che il percorso è stato valutato
+ ____ Principalselenca tutte le entità di cui l&#39;entità selezionata fa parte.
   + Questo è utile per comprendere le appartenenze di gruppo transitive che possono fornire le autorizzazioni tramite l&#39;ereditarietà
+ __Privilegi in__ Pathlist tutte le autorizzazioni JCR di cui dispone l&#39;entità selezionata sul percorso valutato

### Attività di debug non supportate

Di seguito sono riportate le attività di debug che è possibile eseguire in CRXDE Lite __not__.

### Debug delle configurazioni OSGi

Le configurazioni OSGi distribuite non possono essere riviste tramite CRXDE Lite. Le configurazioni OSGi vengono conservate nel pacchetto di codice `ui.apps` del progetto AEM in `/apps/example/config.xxx`, tuttavia, al momento della distribuzione in AEM come ambienti Cloud Service, le risorse delle configurazioni OSGi non sono persistenti nel JCR, pertanto non sono visibili tramite CRXDE Lite.

Utilizzate invece [Developer Console > Configurazioni](./developer-console.md#configurations) per esaminare le configurazioni OSGi distribuite.
