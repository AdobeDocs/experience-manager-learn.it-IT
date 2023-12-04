---
title: Guida alla manutenzione ordinaria del sito
description: Che tu sia un amministratore, un autore o uno sviluppatore, la manutenzione del sito tocca ogni aspetto dell’istanza di AEM Sites. Utilizza questa guida per assicurarti che la tua strategia sia configurata per il successo.
role: Admin
level: Beginner, Intermediate
topic: Administration
feature: Learn From Your Peers
jira: KT-14255
exl-id: 37ee3234-f91c-4f0a-b0b7-b9167e7847a9
duration: 266
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '998'
ht-degree: 3%

---

# Suggerimenti e trucchi per la manutenzione del sito

Sono disponibili tre opzioni per installare e gestire un’istanza AEM

* AEMaaCS (servizio cloud): il sistema è sempre attivo, aggiornato e scalabile in modo dinamico in base alle tue esigenze
* Adobe Managed Services, in cui i tecnici dell&#39;assistenza clienti Adobi eseguono tutti gli interventi di manutenzione giornalieri, settimanali e mensili e garantiscono l&#39;installazione di tutti i Service Pack, la sicurezza e l&#39;operatività del sistema
* l&#39;esecuzione on-premise, dove è necessario occuparsi dell&#39;intero sistema, inclusi backup, aggiornamenti e sicurezza.

Se scegli di implementare il tuo sistema on-premise, ecco alcune cose da tenere a mente per assicurarti di avere un sistema sicuro ed efficiente. Oltre alle voci &quot;cura e alimentazione&quot;, questo documento indicherà anche diversi elementi AEM Developers dovrebbero tenere a mente di aiutare a mantenere il sistema funzionante bene.

## Amministratori

Backup: assicurarsi di avere backup completi e/o parziali in base a una pianificazione frequente:

* ogni giorno
* ogni settimana
* mensile

Molti clienti eseguono i backup delle istantanee, che richiedono solo pochi minuti, supponendo che il sistema operativo sottostante supporti tali backup. Assicurarsi che i backup siano archiviati correttamente (fuori dal sistema AEM). Assicurati che i backup siano funzionali e possano essere utilizzati periodicamente per ricreare un sistema di lavoro: non c’è niente di peggio che arrestare il sistema e i backup sono danneggiati per qualche motivo!

È necessario monitorare diversi elementi per garantire un funzionamento senza problemi:

### Manutenzione ordinaria

#### [manutenzione indice](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=en)

Gli indici consentono di eseguire le query il più rapidamente possibile, liberando risorse per altre operazioni. Assicurati che gli indici siano in forma di punta! L’AEM annulla le query che eseguono invece di utilizzare un indice per evitare che una query non valida influisca sulle prestazioni complessive dell’AEM.

#### [Compattazione TAR/Pulizia revisioni](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=en)

Ogni aggiornamento del repository crea una nuova revisione del contenuto. Di conseguenza, con ogni aggiornamento aumenta la dimensione dell’archivio. Per evitare una crescita incontrollata dell&#39;archivio, è necessario pulire le vecchie revisioni per liberare le risorse su disco.

#### [Pulizia binari Lucene](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-dashboard.html#automated-maintenance-tasks)

Elimina i file binari Lucene e riduci i requisiti di dimensione dell’archivio dati in esecuzione.

#### [Archivio dati spazzatura](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/data-store-garbage-collection.html?lang=it)

Quando una risorsa in AEM viene eliminata, il riferimento al record dell’archivio dati sottostante può essere rimosso dalla gerarchia dei nodi, ma il record dell’archivio dati stesso rimane. Questo record dell’archivio dati senza riferimenti diventa &quot;spazzatura&quot; e non deve essere mantenuto. Nei casi in cui esistono numerose risorse senza riferimenti, è utile eliminarle, preservare lo spazio, ottimizzare il backup e le prestazioni di manutenzione del file system.

#### [Svuotamento flusso di lavoro](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/workflows-administering.html)

Minimizzare il numero di istanze del flusso di lavoro aumenta le prestazioni del motore del flusso di lavoro, in modo da poter eliminare regolarmente dall’archivio le istanze del flusso di lavoro completate o in esecuzione.

#### [Manutenzione del registro di controllo](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-audit-log.html

Gli eventi AEM idonei per la registrazione di audit generano molti dati archiviati. Questi dati possono crescere rapidamente nel tempo a causa di repliche, caricamenti di risorse e altre attività del sistema.

#### [Sicurezza](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=en)

Accertati che le best practice relative agli elenchi di controllo per la sicurezza siano seguite attentamente per garantire l’istanza più sicura dell’AEM.

#### Spazio su disco

Monitora lo spazio su disco per assicurarti di averne a sufficienza per l’archivio JCR, più circa la metà di nuovo lo spazio - la compattazione tar utilizza spazio aggiuntivo quando viene eseguita. L&#39;esaurimento dello spazio su disco è il motivo principale per la corruzione di JCR.

## Sviluppatore

Prova a non utilizzare componenti personalizzati: utilizzare [Componenti core](https://www.aemcomponents.dev/). L’obiettivo dovrebbe essere quello di utilizzare l’80-90% dei componenti core e i componenti personalizzati solo con moderazione. Questo spesso richiede un nuovo modo di esaminare i componenti su una pagina: è necessario rendersi conto che i componenti possono essere facilmente rinominati da uno sviluppatore front-end che utilizza CSS. Tieni presente che questi componenti core possono essere incorporati l’uno nell’altro per ottenere risultati piuttosto complessi. Diventa creativo!

### [Sistemi di stili](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=en)

I sistemi di stile consentono ai componenti core, e anche ai componenti personalizzati, di avere il loro aspetto e la sensazione di cambiamento a discrezione degli autori, per creare componenti completamente nuovi. Queste modifiche stilistiche in genere coinvolgono solo un designer front-end e un autore esperto (spesso denominato &quot;Super Author&quot;)

### [Lanci](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=en)

I lanci consentono di completare il lavoro per una nuova promozione, vendita o rollout di siti web senza influire sulle pagine attualmente distribuite. Inoltre, possono essere programmati per andare in diretta automaticamente, senza partecipazione o supervisione, consentendo agli autori di fare il lavoro della settimana prossima (o del trimestre prossimo) oggi e non affrettarsi nello sviluppo della pagina il giorno prima che dovrebbe andare in diretta - è veramente il regalo del TEMPO!)

### [Frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments.html)

I frammenti di contenuto sono &quot;blocchi&quot; di informazioni personalizzabili che possono essere facilmente riutilizzati in tutto il sito. Se hai bisogno di una modifica, devi solo cambiare il blocco originale e l’aggiornamento viene visualizzato ovunque venga utilizzato, immediatamente!

### [Frammenti esperienza](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)

Anche se sembrano quasi identici ai frammenti di contenuto, i frammenti di esperienza sono piccole parti visibili di una pagina. Questi possono essere riutilizzati ampiamente in tutto il sito e mantenuti in una posizione centrale all&#39;interno dell&#39;AEM per facilitare il compito di apportare modifiche potenzialmente globali in tutto il sito in pochi secondi, non in giorni o settimane.

Pensa a lungo e scopri cosa potrebbe essere riutilizzato. Un piè di pagina? Una liberatoria? Intestazione? Alcuni tipi di contenuto? Tutti questi elementi possono essere condivisi in un intero sito mantenendo al minimo la manutenzione. Devi aggiornare una data in una liberatoria, ma è su 1.000 pagine del tuo sito? Si tratta di un’operazione di 5 secondi se hai utilizzato un frammento di esperienza.

## Generale

Rimani al passo con i cambiamenti AEM continuando ad imparare - non rimanere bloccati nel passato. Utilizzare [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=en) e [Servizi di apprendimento digitale Adobe (ADLS)](https://learning.adobe.com/) per affinare le tue abilità.

## Conclusione

L&#39;AEM può essere un sistema vasto, e ci vogliono molti tipi di persone per farlo &quot;cantare&quot;. Dagli amministratori agli sviluppatori (sia sviluppatori front-end che hardcore Java) agli autori: c’è qualcosa per tutti! E se non avete voglia di occuparvi dell&#39;amministrazione quotidiana, c&#39;è sempre AMS e AEM as a Cloud Service.
