---
title: CRXDE Lite
description: 'CRXDE Lite è uno strumento classico ma potente per il debug degli ambienti per sviluppatori di AEM as a Cloud Service. CRXDE Lite fornisce una suite di funzionalità che facilita il debug dall''ispezione di tutte le risorse e proprietà, dalla manipolazione delle parti mutabili del JCR e dalle indagini sulle autorizzazioni. '
feature: Strumenti per gli sviluppatori
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Sviluppo
role: Developer (Sviluppatore)
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---


# Debug di AEM as a Cloud Service con CRXDE Lite

CRXDE Lite è __ONLY__ disponibile negli ambienti di sviluppo di AEM as a Cloud Service (e nell’SDK locale di AEM).

## Accesso a CRXDE Lite su AEM Author

CRXDE Lite è __accessibile solo__ negli ambienti di sviluppo di AEM as a Cloud Service ed è __non__ disponibile negli ambienti Stage o Production.

Per accedere a CRXDE Lite su AEM Author:

1. Accedi al servizio AEM as a Cloud Service Author.
1. Passa a Strumenti > Generale > CRXDE Lite

Verrà aperto CRXDE Lite utilizzando le credenziali e le autorizzazioni utilizzate per accedere ad AEM Author.

## Debug del contenuto

CRXDE Lite fornisce l&#39;accesso diretto al JCR. Il contenuto visibile tramite CRXDE Lite è limitato dalle autorizzazioni concesse all&#39;utente, il che significa che potresti non essere in grado di vedere o modificare tutto nel JCR a seconda del tuo accesso.

Tieni presente che `/apps`, `/libs` e `/oak:index` non sono modificabili, il che significa che non possono essere modificate in fase di runtime da alcun utente. Queste posizioni nel JCR possono essere modificate solo tramite distribuzioni di codice.

+ La struttura JCR viene navigata e manipolata utilizzando il riquadro di navigazione a sinistra
+ Selezionando un nodo nel riquadro di navigazione a sinistra, espone le proprietà del nodo nel riquadro in basso.
   + Le proprietà possono essere aggiunte, rimosse o modificate dal riquadro
+ Facendo doppio clic su un nodo di file nel menu di navigazione a sinistra, si apre il contenuto del file nel riquadro in alto a destra
+ Tocca il pulsante Salva tutto in alto a sinistra per mantenere le modifiche, oppure la freccia rivolta verso il basso accanto a Salva tutto per ripristinare le modifiche non salvate.

![CRXDE Lite - Debug del contenuto](./assets/crxde-lite/debugging-content.png)

È necessario fare con attenzione le modifiche apportate al contenuto mutabile in fase di runtime negli ambienti di sviluppo di AEM as a Cloud Service tramite CRXDE Lite.
Qualsiasi modifica apportata direttamente ad AEM tramite CRXDE Lite può essere difficile da monitorare e gestire. Se appropriato, assicurati che le modifiche effettuate tramite CRXDE Lite facciano il loro ritorno ai pacchetti di contenuto variabile del progetto AEM (`ui.content`) e si impegnino per Git, al fine di garantire che il problema sia risolto. Idealmente, tutte le modifiche al contenuto dell’applicazione provengono dalla base di codice e scorrono in AEM tramite implementazioni, anziché apportare modifiche direttamente all’AEM tramite CRXDE Lite.

### Debug dei controlli di accesso

CRXDE Lite fornisce un modo per testare e valutare il controllo di accesso su un nodo specifico per un utente o un gruppo specifico (aka principal).

Per accedere alla console di controllo degli accessi di prova in CRXDE Lite, passa a:

+ CRXDE Lite > Strumenti > Controllo accesso di prova ...

![CRXDE Lite - Controllo dell&#39;accesso ai test](./assets/crxde-lite/permissions__test-access-control.png)

1. Utilizzando il campo Percorso, seleziona un percorso JCR da valutare
1. Utilizzando il campo Principale, selezionare l&#39;utente o il gruppo con cui valutare il percorso
1. Toccare il pulsante Test

Di seguito sono riportati i risultati:

+ ____ Pathribadisce il percorso valutato
+ ____ Principalribadisce all’utente o al gruppo per cui è stato valutato il percorso
+ ____ Principalselenca tutti i principals di cui fa parte l&#39;entità selezionata.
   + È utile per comprendere le appartenenze al gruppo transitorio che possono fornire autorizzazioni tramite ereditarietà
+ __Privilegi in__ percorsi elenca tutte le autorizzazioni JCR di cui dispone l&#39;entità selezionata sul percorso valutato

### Attività di debug non supportate

Di seguito sono riportate le attività di debug che possono essere eseguite in CRXDE Lite __not__ .

### Debug delle configurazioni OSGi

Le configurazioni OSGi implementate non possono essere riviste tramite CRXDE Lite. Le configurazioni OSGi vengono mantenute nel pacchetto di codice `ui.apps` del progetto AEM in `/apps/example/config.xxx`. Tuttavia, dopo l’implementazione in ambienti AEM as a Cloud Service, le risorse di configurazione OSGi non vengono mantenute nel JCR, quindi non sono visibili tramite CRXDE Lite.

Utilizza invece [Console per sviluppatori > Configurazioni](./developer-console.md#configurations) per rivedere le configurazioni OSGi distribuite.
