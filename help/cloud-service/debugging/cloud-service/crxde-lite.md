---
title: CRXDE Lite
description: CRXDE Liti è uno strumento classico ma potente per il debug degli ambienti di sviluppo as a Cloud Service dell’AEM. CRXDE Liti fornisce una suite di funzionalità che consente al debug di esaminare tutte le risorse e le proprietà, manipolare le parti mutabili del JCR e analizzare le autorizzazioni.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
duration: 125
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# Debug di AEM as a Cloud Service con CRXDE Liti

CRXDE Liti è __SOLO__ disponibile negli ambienti di sviluppo as a Cloud Service dell’AEM (nonché nell’SDK per l’AEM locale).

## Accesso a CRXDE Liti su AEM Author

CRXDE Liti è __solo__ accessibile negli ambienti di sviluppo as a Cloud Service dell’AEM e __non__ disponibile negli ambienti di staging o produzione.

Per accedere a CRXDE Liti su AEM Author:

1. Accedi al servizio AEM as a Cloud Service AEM Author.
1. Passa a Strumenti > Generale > CRXDE Liti.

Verranno aperte CRXDE Liti utilizzando le credenziali e le autorizzazioni utilizzate per accedere a AEM Author.

## Debug del contenuto

CRXDE Liti fornisce accesso diretto a JCR. Il contenuto visibile tramite CRXDE Liti è limitato dalle autorizzazioni concesse all’utente, il che significa che potresti non essere in grado di visualizzare o modificare tutto in JCR a seconda del tuo accesso.

Tieni presente che `/apps`, `/libs` e `/oak:index` sono immutabili, ovvero non possono essere modificate in fase di runtime da alcun utente. Queste posizioni nel JCR possono essere modificate solo tramite le distribuzioni di codice.

+ La struttura JCR viene spostata e manipolata utilizzando il riquadro di navigazione a sinistra
+ Selezionando un nodo nel riquadro di navigazione a sinistra, espone la proprietà del nodo nel riquadro inferiore.
   + Le proprietà possono essere aggiunte, rimosse o modificate dal riquadro
+ Facendo doppio clic su un nodo di file nel menu di navigazione a sinistra, il contenuto del file viene aperto nel riquadro in alto a destra
+ Tocca il pulsante Salva tutto in alto a sinistra per mantenere le modifiche, oppure la freccia giù accanto a Salva tutto per ripristinare eventuali modifiche non salvate.

![CRXDE Liti - Debug del contenuto](./assets/crxde-lite/debugging-content.png)

È necessario prestare particolare attenzione alle modifiche apportate al contenuto mutabile in fase di runtime negli ambienti di sviluppo as a Cloud Service dell’AEM tramite CRXDE Liti.
Qualsiasi modifica apportata direttamente all’AEM tramite CRXDE Liti può essere difficile da tracciare e gestire. Se necessario, assicurati che le modifiche apportate tramite CRXDE Liti tornino ai pacchetti di contenuti mutabili del progetto AEM (`ui.content`) e si impegna a utilizzare Git per garantire la risoluzione del problema. Idealmente, tutte le modifiche al contenuto dell’applicazione hanno origine dalla base di codice e passano all’AEM tramite le implementazioni, anziché apportare modifiche direttamente all’AEM tramite CRXDE Liti.

### Debug dei controlli di accesso

CRXDE Liti offre un modo per testare e valutare il controllo degli accessi su un nodo specifico per un utente o un gruppo specifico (o entità principale).

Per accedere alla console Test controllo di accesso in CRXDE Liti, passa a:

+ CRXDE Liti > Strumenti > Test controllo accesso ...

![CRXDE Liti - Test controllo accesso](./assets/crxde-lite/permissions__test-access-control.png)

1. Utilizzando il campo Percorso, seleziona un percorso JCR da valutare
1. Utilizzando il campo Principal, selezionare l&#39;utente o il gruppo su cui valutare il percorso
1. Toccare il pulsante Test

Di seguito sono riportati i risultati:

+ __Percorso__ ribadisce il percorso valutato
+ __Entità__ ripete l&#39;utente o il gruppo per il quale è stato valutato il percorso
+ __Entità__ elenca tutte le entità principali di cui fa parte l&#39;entità selezionata.
   + È utile per comprendere le appartenenze ai gruppi transitivi che possono fornire autorizzazioni tramite ereditarietà
+ __Privilegi nel percorso__ elenca tutte le autorizzazioni JCR di cui dispone l’entità selezionata sul percorso valutato

### Attività di debug non supportate

Di seguito sono riportate le attività di debug che possono __non__ essere eseguita in CRXDE Liti.

### Debug delle configurazioni OSGi

Le configurazioni OSGi implementate non possono essere riviste tramite CRXDE Liti. AEM Le configurazioni OSGi vengono mantenute nel `ui.apps` pacchetto di codice in `/apps/example/config.xxx`Tuttavia, al momento della distribuzione in ambienti AEM as a Cloud Service, le risorse di configurazione OSGi non vengono mantenute in JCR e non sono quindi visibili tramite CRXDE Liti.

Invece, utilizza [Console per sviluppatori > Configurazioni](./developer-console.md#configurations) per rivedere le configurazioni OSGi implementate.
