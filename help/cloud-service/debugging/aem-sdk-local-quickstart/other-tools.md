---
title: Altri strumenti per il debug di AEM SDK
description: Diversi altri strumenti possono essere utili per eseguire il debug dell’avvio rapido locale di AEM SDK.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
duration: 107
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 1%

---

# Altri strumenti per il debug di AEM SDK

Diversi altri strumenti possono essere utili per eseguire il debug dell’applicazione sull’avvio rapido locale di AEM SDK.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite è un’interfaccia basata su web per interagire con JCR, l’archivio dati di AEM. CRXDE Lite fornisce visibilità completa al JCR, inclusi nodi, proprietà, valori di proprietà e autorizzazioni.

CRXDE Lite si trova in:

+ Strumenti > Generale > CRXDE Lite
+ o direttamente da [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

### Debug del contenuto

CRXDE Lite fornisce accesso diretto a JCR. Il contenuto visibile tramite CRXDE Lite è limitato dalle autorizzazioni concesse all’utente, il che significa che potresti non essere in grado di visualizzare o modificare tutto in JCR a seconda del tuo accesso.

+ La struttura JCR viene spostata e manipolata utilizzando il riquadro di navigazione a sinistra
+ Selezionando un nodo nel riquadro di navigazione a sinistra, espone la proprietà del nodo nel riquadro inferiore.
   + Le proprietà possono essere aggiunte, rimosse o modificate dal riquadro
+ Facendo doppio clic su un nodo di file nel menu di navigazione a sinistra, il contenuto del file viene aperto nel riquadro in alto a destra
+ Tocca il pulsante Salva tutto in alto a sinistra per mantenere le modifiche, oppure la freccia giù accanto a Salva tutto per ripristinare eventuali modifiche non salvate.

![CRXDE Lite - Debug del contenuto](./assets/other-tools/crxde-lite__debugging-content.png)

Qualsiasi modifica apportata direttamente ad AEM SDK tramite CRXDE Lite potrebbe essere difficile da tracciare e gestire. Se necessario, assicurati che le modifiche apportate tramite CRXDE Lite tornino ai pacchetti di contenuti mutabili del progetto AEM (`ui.content`) e siano salvate in Git. Idealmente, tutte le modifiche al contenuto dell’applicazione hanno origine dalla base di codice e fluiscono in AEM SDK tramite le implementazioni, anziché apportare modifiche direttamente in AEM SDK tramite CRXDE Lite.

### Debug dei controlli di accesso

CRXDE Lite offre un modo per testare e valutare il controllo degli accessi su un nodo specifico per un utente o un gruppo specifico (o entità principale).

Per accedere alla console Test controllo di accesso in CRXDE Lite, vai a:

+ CRXDE Lite > Strumenti > Test controllo accesso ...

![CRXDE Lite - Verifica controllo accesso](./assets/other-tools/crxde-lite__test-access-control.png)

1. Utilizzando il campo Percorso, seleziona un percorso JCR da valutare
1. Utilizzando il campo Principal, selezionare l&#39;utente o il gruppo su cui valutare il percorso
1. Toccare il pulsante Test

Di seguito sono riportati i risultati:

+ __Il percorso__ ripete il percorso valutato
+ __Entità__ ribadisce l&#39;utente o il gruppo per cui è stato valutato il percorso
+ __Entità__ elenca tutte le entità di cui fa parte l&#39;entità selezionata.
   + È utile per comprendere le appartenenze ai gruppi transitivi che possono fornire autorizzazioni tramite ereditarietà
+ __I privilegi nel percorso__ elencano tutte le autorizzazioni JCR di cui dispone l&#39;entità selezionata nel percorso valutato

## Spiega query

![Spiega query](./assets/other-tools/explain-query.png)

Spiega lo strumento basato sul web Query nell’avvio rapido locale di AEM SDK, che fornisce informazioni chiave sul modo in cui AEM interpreta ed esegue le query e uno strumento prezioso per garantire che le query vengano eseguite in modo performante da AEM.

Spiega query si trova in:

+ Strumenti > Diagnosi > Prestazioni query > Scheda Spiega query
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) > Spiega scheda Query

## Debugger QueryBuilder

![Debugger QueryBuilder](./assets/other-tools/query-debugger.png)

QueryBuilder Debugger è uno strumento basato sul Web che consente di eseguire il debug e comprendere le query di ricerca utilizzando la sintassi [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) di AEM.

QueryBuilder Debugger si trova in:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
