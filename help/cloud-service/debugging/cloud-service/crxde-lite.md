---
title: CRXDE Lite
description: CRXDE Lite è uno strumento classico ma potente per il debug di AEM come ambienti per sviluppatori di Cloud Service. CRXDE Lite fornisce una suite di funzionalità che facilita il debug dall’ispezione di tutte le risorse e proprietà, dalla manipolazione delle parti modificabili del JCR e dalle indagini sulle autorizzazioni.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# Debug di AEM come Cloud Service con CRXDE Lite

CRXDE Lite è __SOLO__ disponibile in AEM come ambienti di sviluppo di Cloud Service (nonché nell&#39;SDK AEM locale).

## Accesso ad CRXDE Lite su AEM Author

CRXDE Lite è __accessibile solo__ in AEM come ambiente di sviluppo di Cloud Service ed è __non__ disponibile in ambienti Stage o Production.

Per accedere ad CRXDE Lite su AEM Author:

1. Accedi al AEM come servizio Cloud Service Author.
1. Passa a Strumenti > Generale > CRXDE Lite

Verrà aperto CRXDE Lite utilizzando le credenziali e le autorizzazioni utilizzate per accedere ad AEM Author.

## Debug del contenuto

CRXDE Lite fornisce l’accesso diretto al JCR. Il contenuto visibile tramite CRXDE Lite è limitato dalle autorizzazioni concesse all’utente, il che significa che potrebbe non essere possibile visualizzare o modificare tutto nel JCR a seconda dell’accesso.

Tieni presente che `/apps`, `/libs` e `/oak:index` non sono modificabili, il che significa che non possono essere modificate in fase di runtime da alcun utente. Queste posizioni nel JCR possono essere modificate solo tramite distribuzioni di codice.

+ La struttura JCR viene navigata e manipolata utilizzando il riquadro di navigazione a sinistra
+ Selezionando un nodo nel riquadro di navigazione a sinistra, espone le proprietà del nodo nel riquadro in basso.
   + Le proprietà possono essere aggiunte, rimosse o modificate dal riquadro
+ Facendo doppio clic su un nodo di file nel menu di navigazione a sinistra, si apre il contenuto del file nel riquadro in alto a destra
+ Tocca il pulsante Salva tutto in alto a sinistra per mantenere le modifiche, oppure la freccia rivolta verso il basso accanto a Salva tutto per ripristinare le modifiche non salvate.

![CRXDE Lite - Debug del contenuto](./assets/crxde-lite/debugging-content.png)

È necessario apportare con attenzione le modifiche al contenuto mutabile in fase di runtime in AEM come ambienti di sviluppo di Cloud Service tramite CRXDE Lite.
Qualsiasi modifica apportata direttamente a AEM tramite CRXDE Lite può risultare difficile da tenere traccia e da gestire. Se appropriato, assicurati che le modifiche effettuate tramite CRXDE Lite ritornino ai pacchetti di contenuto variabile del progetto AEM (`ui.content`) e si impegnino con Git, al fine di garantire la risoluzione del problema. Idealmente, tutte le modifiche al contenuto dell’applicazione provengono dalla base di codice e scorrono in AEM tramite implementazioni, anziché apportare modifiche direttamente al AEM tramite CRXDE Lite.

### Debug dei controlli di accesso

CRXDE Lite offre un modo per testare e valutare il controllo di accesso su un nodo specifico per un utente o un gruppo specifico (aka principal).

Per accedere alla console di controllo dell’accesso ai test in CRXDE Lite, passa a:

+ CRXDE Lite > Strumenti > Test Access Control ..

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

Le configurazioni OSGi implementate non possono essere riviste tramite CRXDE Lite. Le configurazioni OSGi vengono mantenute nel pacchetto di codice `ui.apps` del progetto AEM in `/apps/example/config.xxx`. Tuttavia, una volta implementata in AEM come ambiente di Cloud Service, le risorse di configurazione OSGi non vengono mantenute nel JCR, quindi non sono visibili tramite CRXDE Lite.

Utilizza invece [Console per sviluppatori > Configurazioni](./developer-console.md#configurations) per rivedere le configurazioni OSGi distribuite.
