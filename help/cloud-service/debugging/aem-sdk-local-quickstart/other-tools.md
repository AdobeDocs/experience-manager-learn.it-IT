---
title: Altri strumenti per il debug dell’SDK AEM
description: Diversi altri strumenti possono essere utili per eseguire il debug dell’avvio rapido locale dell’SDK dell’AEM.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
duration: 141
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 1%

---

# Altri strumenti per il debug dell’SDK AEM

Diversi altri strumenti possono essere utili per eseguire il debug dell’applicazione sull’avvio rapido locale dell’SDK dell’AEM.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Liti è un’interfaccia basata su web per interagire con l’archivio dati JCR, AEM. CRXDE Liti fornisce una visibilità completa in JCR, inclusi nodi, proprietà, valori di proprietà e autorizzazioni.

CRXDE Liti si trova in:

+ Strumenti > Generale > CRXDE Lite
+ o direttamente da [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

### Debug del contenuto

CRXDE Liti fornisce accesso diretto a JCR. Il contenuto visibile tramite CRXDE Liti è limitato dalle autorizzazioni concesse all’utente, il che significa che potresti non essere in grado di visualizzare o modificare tutto in JCR a seconda del tuo accesso.

+ La struttura JCR viene spostata e manipolata utilizzando il riquadro di navigazione a sinistra
+ Selezionando un nodo nel riquadro di navigazione a sinistra, espone la proprietà del nodo nel riquadro inferiore.
   + Le proprietà possono essere aggiunte, rimosse o modificate dal riquadro
+ Facendo doppio clic su un nodo di file nel menu di navigazione a sinistra, il contenuto del file viene aperto nel riquadro in alto a destra
+ Tocca il pulsante Salva tutto in alto a sinistra per mantenere le modifiche, oppure la freccia giù accanto a Salva tutto per ripristinare eventuali modifiche non salvate.

![CRXDE Liti - Debug del contenuto](./assets/other-tools/crxde-lite__debugging-content.png)

Qualsiasi modifica apportata direttamente all’SDK dell’AEM tramite CRXDE Liti può essere difficile da tracciare e gestire. Se necessario, assicurati che le modifiche apportate tramite CRXDE Liti tornino ai pacchetti di contenuti mutabili del progetto AEM (`ui.content`) e si impegnano a favore di Git. Idealmente, tutte le modifiche al contenuto dell’applicazione hanno origine dalla base di codice e confluiscono nell’SDK dell’AEM tramite le implementazioni, anziché apportare modifiche direttamente all’SDK dell’AEM tramite CRXDE Liti.

### Debug dei controlli di accesso

CRXDE Liti offre un modo per testare e valutare il controllo degli accessi su un nodo specifico per un utente o un gruppo specifico (o entità principale).

Per accedere alla console Test controllo di accesso in CRXDE Liti, passa a:

+ CRXDE Liti > Strumenti > Test controllo accesso ...

![CRXDE Liti - Test controllo accesso](./assets/other-tools/crxde-lite__test-access-control.png)

1. Utilizzando il campo Percorso, seleziona un percorso JCR da valutare
1. Utilizzando il campo Principal, selezionare l&#39;utente o il gruppo su cui valutare il percorso
1. Toccare il pulsante Test

Di seguito sono riportati i risultati:

+ __Percorso__ ribadisce il percorso valutato
+ __Entità__ ripete l&#39;utente o il gruppo per il quale è stato valutato il percorso
+ __Entità__ elenca tutte le entità principali di cui fa parte l&#39;entità selezionata.
   + È utile per comprendere le appartenenze ai gruppi transitivi che possono fornire autorizzazioni tramite ereditarietà
+ __Privilegi nel percorso__ elenca tutte le autorizzazioni JCR di cui dispone l’entità selezionata sul percorso valutato

## Spiega query

![Spiega query](./assets/other-tools/explain-query.png)

Spiega lo strumento basato sul web Query nell’avvio rapido locale dell’SDK dell’AEM, che fornisce informazioni chiave sul modo in cui l’AEM interpreta ed esegue le query, e uno strumento prezioso per garantire che le query vengano eseguite in modo performante dall’AEM.

Spiega query si trova in:

+ Strumenti > Diagnosi > Prestazioni query > Scheda Spiega query
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) > Scheda Spiega query

## Debugger QueryBuilder

![Debugger QueryBuilder](./assets/other-tools/query-debugger.png)

QueryBuilder Debugger è uno strumento basato su Web che consente di eseguire il debug e comprendere le query di ricerca utilizzando AEM [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) sintassi.

QueryBuilder Debugger si trova in:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
