---
title: CRXDE Lite
description: CRXDE Lite è uno strumento classico ma potente per il debug degli ambienti di AEM as a Cloud Service Developer. CRXDE Lite fornisce una suite di funzionalità che consente al debug di esaminare tutte le risorse e le proprietà, manipolare le parti mutabili del JCR e analizzare le autorizzazioni.
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

# Debug di AEM as a Cloud Service con CRXDE Lite

CRXDE Lite è __ONLY__ disponibile negli ambienti di sviluppo AEM as a Cloud Service (nonché nell&#39;SDK AEM locale).

## Accesso a CRXDE Lite su AEM Author

CRXDE Lite è accessibile __only__ negli ambienti di sviluppo AEM as a Cloud Service ed è __not__ disponibile negli ambienti di staging o produzione.

Per accedere a CRXDE Lite su AEM Author:

1. Accedi al servizio AEM as a Cloud Service AEM Author.
1. Passa a Strumenti > Generale > CRXDE Liti.

Verranno aperte CRXDE Lite utilizzando le credenziali e le autorizzazioni utilizzate per accedere a AEM Author.

## Debug del contenuto

CRXDE Lite fornisce accesso diretto a JCR. Il contenuto visibile tramite CRXDE Lite è limitato dalle autorizzazioni concesse all’utente, il che significa che potresti non essere in grado di visualizzare o modificare tutto in JCR a seconda del tuo accesso.

`/apps`, `/libs` e `/oak:index` non sono modificabili, ovvero non possono essere modificati in fase di esecuzione da alcun utente. Queste posizioni nel JCR possono essere modificate solo tramite le distribuzioni di codice.

+ La struttura JCR viene spostata e manipolata utilizzando il riquadro di navigazione a sinistra
+ Selezionando un nodo nel riquadro di navigazione a sinistra, espone la proprietà del nodo nel riquadro inferiore.
   + Le proprietà possono essere aggiunte, rimosse o modificate dal riquadro
+ Facendo doppio clic su un nodo di file nel menu di navigazione a sinistra, il contenuto del file viene aperto nel riquadro in alto a destra
+ Tocca il pulsante Salva tutto in alto a sinistra per mantenere le modifiche, oppure la freccia giù accanto a Salva tutto per ripristinare eventuali modifiche non salvate.

![CRXDE Liti - Debug del contenuto](./assets/crxde-lite/debugging-content.png)

È necessario prestare particolare attenzione alle modifiche apportate al contenuto mutabile in fase di runtime negli ambienti di sviluppo AEM as a Cloud Service tramite CRXDE Lite.
Qualsiasi modifica apportata direttamente all’AEM tramite CRXDE Lite può essere difficile da tracciare e gestire. Se necessario, assicurati che le modifiche apportate tramite CRXDE Lite tornino ai pacchetti di contenuti mutabili del progetto AEM (`ui.content`) e siano applicate a Git, al fine di garantire la risoluzione del problema. Idealmente, tutte le modifiche al contenuto dell’applicazione hanno origine dalla base di codice e passano all’AEM tramite le implementazioni, anziché apportare modifiche direttamente all’AEM tramite CRXDE Liti.

### Debug dei controlli di accesso

CRXDE Lite offre un modo per testare e valutare il controllo degli accessi su un nodo specifico per un utente o un gruppo specifico (o entità principale).

Per accedere alla console Test controllo di accesso in CRXDE Lite, passa a:

+ CRXDE Liti > Strumenti > Test controllo accesso ...

![CRXDE Liti - Verifica controllo accesso](./assets/crxde-lite/permissions__test-access-control.png)

1. Utilizzando il campo Percorso, seleziona un percorso JCR da valutare
1. Utilizzando il campo Principal, selezionare l&#39;utente o il gruppo su cui valutare il percorso
1. Toccare il pulsante Test

Di seguito sono riportati i risultati:

+ __Il percorso__ ripete il percorso valutato
+ __Entità__ ribadisce l&#39;utente o il gruppo per cui è stato valutato il percorso
+ __Entità__ elenca tutte le entità di cui fa parte l&#39;entità selezionata.
   + È utile per comprendere le appartenenze ai gruppi transitivi che possono fornire autorizzazioni tramite ereditarietà
+ __I privilegi nel percorso__ elencano tutte le autorizzazioni JCR di cui dispone l&#39;entità selezionata nel percorso valutato

### Attività di debug non supportate

Di seguito sono riportate le attività di debug che possono __non__ essere eseguite in CRXDE Lite.

### Debug delle configurazioni OSGi

Le configurazioni OSGi implementate non possono essere riviste tramite CRXDE Lite. Le configurazioni OSGi vengono mantenute nel pacchetto di codice `ui.apps` del progetto AEM in `/apps/example/config.xxx`. Tuttavia, al momento della distribuzione negli ambienti AEM as a Cloud Service, le risorse di configurazione OSGi non vengono mantenute in JCR e non sono quindi visibili tramite CRXDE Lite.

Utilizza invece [Developer Console > Configurations](./developer-console.md#configurations) per rivedere le configurazioni OSGi distribuite.
