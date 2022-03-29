---
title: Altri strumenti per il debug AEM SDK
description: Diversi altri strumenti possono facilitare il debug dell'avvio rapido locale dell'SDK AEM.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 2%

---

# Altri strumenti per il debug AEM SDK

L’avvio rapido locale dell’SDK di AEM può essere utile con diversi altri strumenti per il debug dell’applicazione.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite è un’interfaccia basata su Web per interagire con JCR, AEM archivio dati. CRXDE Lite offre una visibilità completa del JCR, compresi nodi, proprietà, valori di proprietà e autorizzazioni.

CRXDE Lite si trova in:

+ Strumenti > Generale > CRXDE Lite
+ o direttamente a [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

### Debug del contenuto

CRXDE Lite fornisce l’accesso diretto al JCR. Il contenuto visibile tramite CRXDE Lite è limitato dalle autorizzazioni concesse all’utente, il che significa che potrebbe non essere possibile visualizzare o modificare tutto nel JCR a seconda dell’accesso.

+ La struttura JCR viene navigata e manipolata utilizzando il riquadro di navigazione a sinistra
+ Selezionando un nodo nel riquadro di navigazione a sinistra, espone le proprietà del nodo nel riquadro in basso.
   + Le proprietà possono essere aggiunte, rimosse o modificate dal riquadro
+ Facendo doppio clic su un nodo di file nel menu di navigazione a sinistra, si apre il contenuto del file nel riquadro in alto a destra
+ Tocca il pulsante Salva tutto in alto a sinistra per mantenere le modifiche, oppure la freccia rivolta verso il basso accanto a Salva tutto per ripristinare le modifiche non salvate.

![CRXDE Lite - Debug del contenuto](./assets/other-tools/crxde-lite__debugging-content.png)

Qualsiasi modifica apportata direttamente all’SDK AEM tramite CRXDE Lite può risultare difficile da tenere traccia e da gestire. Se appropriato, assicurati che le modifiche effettuate tramite CRXDE Lite ritornino ai pacchetti di contenuto variabile del progetto AEM (`ui.content`) e si impegna a Git. Idealmente, tutte le modifiche al contenuto dell’applicazione provengono dalla base di codice e passano all’SDK AEM tramite implementazioni , anziché apportare modifiche direttamente all’SDK AEM tramite CRXDE Lite.

### Debug dei controlli di accesso

CRXDE Lite offre un modo per testare e valutare il controllo di accesso su un nodo specifico per un utente o un gruppo specifico (aka principal).

Per accedere alla console di controllo dell’accesso ai test in CRXDE Lite, passa a:

+ CRXDE Lite > Strumenti > Test Access Control ..

![CRXDE Lite - Controllo dell&#39;accesso ai test](./assets/other-tools/crxde-lite__test-access-control.png)

1. Utilizzando il campo Percorso, seleziona un percorso JCR da valutare
1. Utilizzando il campo Principale, selezionare l&#39;utente o il gruppo con cui valutare il percorso
1. Toccare il pulsante Test

Di seguito sono riportati i risultati:

+ __Percorso__ ribadisce il percorso valutato
+ __Principale__ ribadisce all&#39;utente o al gruppo che il percorso è stato valutato
+ __Principali__ elenca tutte le entità principali di cui fa parte l&#39;entità selezionata.
   + È utile per comprendere le appartenenze al gruppo transitorio che possono fornire autorizzazioni tramite ereditarietà
+ __Privilegi nel percorso__ elenca tutte le autorizzazioni JCR di cui dispone l&#39;entità selezionata sul percorso valutato

## Spiega query

![Spiega query](./assets/other-tools/explain-query.png)

Spiega lo strumento basato su web Query nell’avvio rapido locale AEM’SDK, che fornisce informazioni chiave su come AEM interpreta ed esegue le query, e uno strumento prezioso per garantire che le query vengano eseguite in modo performante da AEM.

Spiega query si trova in:

+ Strumenti > Diagnosi > Prestazioni query > Scheda Spiega query
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) > Scheda Spiega query

## Debugger di QueryBuilder

![Debugger di QueryBuilder](./assets/other-tools/query-debugger.png)

Il debugger di QueryBuilder è uno strumento basato su Web che consente di eseguire il debug e comprendere le query di ricerca utilizzando AEM [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) sintassi.

Il debugger di QueryBuilder si trova in:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
