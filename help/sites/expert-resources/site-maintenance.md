---
title: Guida alla manutenzione ordinaria del sito
seo-title: Your Routine Site Maintenance Guide
description: La manutenzione del sito tocca ogni aspetto dell’istanza di AEM Sites, sia che tu sia un amministratore, un autore o uno sviluppatore. Utilizza questa guida per assicurarti che la tua strategia sia configurata per il successo.
seo-description: Whether you're an admin, author, or developer, site maintenance touches every aspect of your AEM Sites instance. Use this guide to ensure your strategy is set up for success.
audience: author, marketer, developer
source-git-commit: d545e7bb5e937959e2ede2b3c1ecfc312df5a044
workflow-type: tm+mt
source-wordcount: '1094'
ht-degree: 4%

---


# Suggerimenti e trucchi per la manutenzione del sito

Ci sono tre opzioni quando si tratta di installare e mantenere un&#39;istanza AEM

* AEMaaCS (servizio cloud): il sistema è sempre attivo, aggiornato e scalato in modo dinamico in base alle tue esigenze
* Adobe Managed Services, in cui gli ingegneri del servizio clienti di Adobe eseguono tutta la manutenzione giornaliera/settimanale/mensile e assicurano che tutti i service pack siano installati e che il sistema sia sempre sicuro e funzionante senza problemi
* esecuzione on-premise, dove è necessario occuparsi dell&#39;intero sistema, inclusi backup, aggiornamenti e sicurezza.

Se scegli di implementare il tuo sistema locale, ecco alcune cose da tenere a mente per assicurarti di avere un sistema sicuro e performante. Oltre agli articoli &quot;cura e alimentazione&quot;, questo documento indicherà anche diversi articoli AEM Sviluppatori dovrebbero tenere a mente per aiutare a mantenere il sistema in funzione bene.

## Amministratori

Backup: assicurati di disporre di backup completi e/o parziali in base a una pianificazione frequente:

* ogni giorno
* ogni settimana
* mensile

Molti clienti eseguono backup snapshot, che richiedono solo pochi minuti supponendo che il sistema operativo sottostante supporti tali backup. Assicurati che questi backup siano archiviati correttamente (fuori dal sistema AEM). Assicurati che i backup siano funzionali e possano essere utilizzati per ricreare periodicamente un sistema di lavoro - non c&#39;è niente di peggio che avere il crash di sistema e i backup sono corrotti per qualche motivo!

Ci sono diversi elementi che è necessario monitorare per garantire il funzionamento senza problemi:

### Manutenzione ordinaria

#### [manutenzione degli indici](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=it)

Gli indici consentono l’esecuzione delle query il più rapidamente possibile, liberando risorse per altre operazioni. Assicurati che gli indici siano in forma punta-top! AEM annulla le query che travere invece di utilizzare un indice per evitare che una cattiva query influisca sulle prestazioni AEM complessive.

#### [Compattazione Tar/Pulizia revisioni](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=en)

Ogni aggiornamento del repository crea una nuova revisione del contenuto. Di conseguenza, con ogni aggiornamento le dimensioni dell&#39;archivio crescono. Per evitare una crescita incontrollata dell&#39;archivio, è necessario ripulire le vecchie revisioni per liberare le risorse del disco.

#### [Pulizia binari Lucene](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/operations-dashboard.html?lang=en#automated-maintenance-tasks)

Elimina i file binari lucene e riduci il requisito di dimensione dell’archivio dati in esecuzione.

#### [Archivio dati spazzatura](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/data-store-garbage-collection.html?lang=en)

Quando una risorsa in AEM viene eliminata, il riferimento al record dell&#39;archivio dati sottostante può essere rimosso dalla gerarchia dei nodi, ma il record dell&#39;archivio dati stesso rimane. Questo record dell&#39;archivio dati senza riferimento diventa &quot;spazzatura&quot; che non deve essere conservata. Nei casi in cui esistono diverse risorse senza riferimento, è utile eliminarle, conservarne lo spazio, ottimizzare il backup e le prestazioni di manutenzione del file system.

#### [Eliminazione flussi di lavoro](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/workflows-administering.html?lang=en)

Minimizzare il numero di istanze del flusso di lavoro aumenta le prestazioni del motore del flusso di lavoro, in modo da poter eliminare regolarmente dall’archivio le istanze del flusso di lavoro completate o in esecuzione.

#### [Manutenzione del registro di controllo](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/operations-audit-log.html?lang=en)

AEM eventi idonei per la registrazione di controllo generano molti dati archiviati. Questi dati possono crescere rapidamente nel tempo a causa di repliche, caricamenti di risorse e altre attività del sistema.

#### [Sicurezza](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=en)

Assicurati che le best practice relative alla lista di controllo della sicurezza siano seguite da vicino per garantire l&#39;istanza più sicura di AEM.

#### Diskspace

Monitora lo spazio su disco per assicurarti di averne abbastanza per l&#39;archivio JCR, più la metà di nuovo dello spazio - la compattazione tar utilizza spazio aggiuntivo quando viene eseguito. L&#39;esaurimento dello spazio su disco è il motivo numero uno per la corruzione di JCR!

## Sviluppatore

Prova a non utilizzare componenti personalizzati - utilizza [componenti core](https://www.aemcomponents.dev/). L’obiettivo dovrebbe essere quello di utilizzare i componenti core per l’80-90% del tempo e i componenti personalizzati solo con moderazione. Spesso questo richiede un nuovo modo di guardare i componenti su una pagina; è necessario comprendere che i componenti possono essere facilmente riattivati da uno sviluppatore front-end che utilizza i CSS. Tieni presente che questi componenti core possono essere incorporati l’uno nell’altro per ottenere risultati piuttosto complessi. Diventa creativo!

### [Sistemi di stile](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=en)

I sistemi di stile consentono ai componenti core e anche ai componenti personalizzati di avere un aspetto diverso a discrezione degli autori e di creare componenti completamente nuovi. Questi cambiamenti stilistici in genere coinvolgono solo un designer front-end e un autore esperto (spesso denominato &quot;Super Author&quot;)

### [Lanci](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=en)

I lanci consentono di completare il lavoro per un nuovo rollout Promozione, Vendita o sito web senza influire sulle pagine attualmente distribuite. Inoltre, possono essere programmati per andare in diretta automaticamente, senza presenze o supervisione, permettendo agli autori di fare il lavoro della prossima settimana (o del prossimo trimestre) oggi e non affrettarsi nello sviluppo di pagina il giorno prima che dovrebbe andare in diretta - è davvero il dono del TEMPO!)

### [Frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-64/assets/fragments/content-fragments.html?lang=en)

I frammenti di contenuto sono &quot;blocchi&quot; personalizzabili di informazioni che possono essere facilmente riutilizzate in tutto il sito. Se hai bisogno di un cambiamento, devi solo cambiare il blocco originale e l&#39;aggiornamento è visto ovunque sia utilizzato - immediatamente!

### [Frammenti esperienza](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)

Anche se l’audio è quasi identico ai frammenti di contenuto, i frammenti esperienza sono piccoli, visibili, parti di una pagina. Questi possono anche essere riutilizzati ampiamente in tutto il sito e mantenuti in una posizione centrale entro AEM per facilitare l&#39;attività di apportare modifiche potenzialmente globali nel sito in pochi secondi, non giorni o settimane.

Pensate in avanti e guardate cosa potrebbe essere riutilizzato. Un piè di pagina? Una liberatoria? Un&#39;Intestazione? Alcuni tipi di contenuto? Tutte queste funzionalità possono essere condivise su un intero sito mantenendo al minimo la manutenzione. Devi aggiornare una data in una dichiarazione di responsabilità, ma si trova su 1.000 pagine sul tuo sito? È un’operazione di 5 secondi se hai utilizzato un Frammento esperienza!

## Generale

Resta al corrente dei cambiamenti AEM attraverso l&#39;apprendimento continuo - non rimanere bloccato in passato. Utilizzo [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=en) e [Adobe Digital Learning Services (ADLS)](https://learning.adobe.com/) affinare le tue abilità.

## Conclusione

AEM può essere un grande sistema, e ci vogliono molti tipi di persone per farlo &quot;cantare&quot;. Dagli amministratori agli sviluppatori (sviluppatori Java front-end e hardcore) agli autori - c’è qualcosa per tutti! E se non hai voglia di gestire l&#39;amministrazione quotidiana, ci sono sempre AMS e AEM as a Cloud Service.
