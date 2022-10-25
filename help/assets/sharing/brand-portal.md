---
title: Utilizzo di Brand Portal
description: Video introduttivi sull’integrazione di AEM Author e AEM Assets Brand Portal.
feature: Brand Portal
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-15T00:00:00Z
exl-id: 42f13a19-52bf-413d-a141-63f1f0910dce
source-git-commit: f37483f90f2a707c906e1e206795fdebb5f698e9
workflow-type: tm+mt
source-wordcount: '1764'
ht-degree: 2%

---

# Utilizzo di Brand Portal con AEM Assets{#using-brand-portal-with-aem-assets}

Guide video dell’integrazione di Adobe Experience Manager (AEM) Assets Brand Portal.

## Funzionalità e miglioramenti di Brand Portal settembre 2019

Brand Portal a settembre 2019 introduce in particolare Asset Sourcing, che aumenta la velocità dei contenuti e consente uno scambio rapido e semplice di risorse tra autori di Experienci Manager e creativi e collaboratori di terze parti.

### Origine risorse Brand Portal{#asset-sourcing}

Asset Sourcing di Brand Portal viene utilizzato per raccogliere le risorse da agenzie e team di terze parti, sincronizzandole senza soluzione di continuità con Experience Manager Author per la revisione e l’utilizzo.

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*Per utilizzare Asset Sourcing è necessario Experience Manager Author 6.5 SP2 (6.5.2) o versione successiva*

Revisione [Abilita authoring di Experience Manager per Asset Sourcing](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/brand-portal-asset-sourcing.html?lang=it) per istruzioni su come configurare e impostare Asset Sourcing su Experience Manager Author.

## Funzionalità e miglioramenti di Brand Portal febbraio 2019{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

La versione di febbraio 2019 di Brand Portal è incentrata sui miglioramenti alla ricerca di testo e alle richieste principali dei clienti.

### Miglioramenti alla ricerca

Brand Portal migliora la ricerca con la ricerca parziale del testo sul predicato delle proprietà nel riquadro di filtraggio. Per consentire la ricerca parziale del testo è necessario abilitare la ricerca parziale nel predicato Proprietà nel modulo di ricerca.

Continua a leggere per ulteriori informazioni sulla ricerca parziale del testo e sui caratteri jolly.

#### Ricerca di frasi parziali

È ora possibile cercare le risorse specificando solo una parte, ovvero una parola o due, della frase cercata nel riquadro di filtro.

**Caso d’uso** : La ricerca di frasi parziali è utile quando non si è sicuri della combinazione esatta di parole che si verificano nella frase cercata.

Ad esempio, se il modulo di ricerca in Brand Portal utilizza il predicato Proprietà per la ricerca parziale sul titolo delle risorse, specificando il termine camp verranno restituite tutte le risorse con il word camp nella relativa frase del titolo.

#### Ricerca con caratteri jolly

Brand Portal consente di utilizzare l’asterisco (*) nella query di ricerca insieme a una parte della parola nella frase cercata.

**Caso d’uso** :Se non sei sicuro delle parole esatte che si verificano nella frase cercata, puoi utilizzare una ricerca con caratteri jolly per riempire gli spazi vuoti nella query di ricerca.

Ad esempio, specificando climb* vengono restituite tutte le risorse con parole che iniziano con i caratteri che salgono nella frase del titolo se il modulo di ricerca in Brand Portal utilizza Predicato proprietà per la ricerca parziale sul titolo delle risorse.

Analogamente, specificando:

* \*climb restituisce tutte le risorse con parole che terminano con caratteri che salgono nella frase del titolo.
* \*climb\* restituisce tutte le risorse con parole che comprendono i caratteri che salgono nella frase del titolo.

#### Abilita gerarchia cartelle

Gli amministratori possono ora configurare il modo in cui le cartelle vengono visualizzate agli utenti non amministratori (editor, visualizzatori e utenti ospiti) al momento dell’accesso.
[Abilita gerarchia cartelle](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) La configurazione viene aggiunta in Impostazioni generali, nel pannello Strumenti di amministrazione. Se la configurazione è:

* Attivata, la struttura delle cartelle che inizia dalla cartella principale è visibile agli utenti non amministratori. Pertanto, concedendo loro un’esperienza di navigazione simile agli amministratori.
* Disabilitata, nella pagina di destinazione vengono visualizzate solo le cartelle condivise.

[Abilita gerarchia cartelle](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) Questa funzionalità (se abilitata) consente di distinguere le cartelle con gli stessi nomi condivisi da gerarchie diverse. Al momento dell&#39;accesso, gli utenti non amministratori ora visualizzano le cartelle padre (e predecessori) virtuali delle cartelle condivise.

Le cartelle condivise sono organizzate all&#39;interno delle rispettive directory in cartelle virtuali. È possibile riconoscere queste cartelle virtuali con un&#39;icona di blocco.

La miniatura predefinita delle cartelle virtuali è l&#39;immagine in miniatura della prima cartella condivisa.

### Supporto delle rappresentazioni video Dynamic Media

Gli utenti la cui istanza AEM Author è in modalità ibrida di Dynamic Media possono visualizzare in anteprima e scaricare le rappresentazioni degli elementi multimediali dinamici, oltre ai file video originali.

Per consentire l’anteprima e il download di rappresentazioni di file multimediali dinamici su account tenant specifici, gli amministratori devono specificare la configurazione Dynamic Media (URL del servizio video (URL del gateway DM) e l’ID di registrazione per recuperare il video dinamico) nella configurazione video dal pannello strumenti di amministrazione.

I video Dynamic Media possono essere visualizzati in anteprima su:

* Pagina dei dettagli della risorsa
* Vista a schede della risorsa
* Pagina di anteprima della condivisione dei collegamenti

Le codifiche video Dynamic Media possono essere scaricate da:

* Brand Portal
* Collegamento condiviso

### Pubblicazione pianificata in Brand Portal

Flusso di lavoro di pubblicazione delle risorse (e delle cartelle) da [AEM (6.4.2.0)](https://helpx.adobe.com/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011) L’istanza di authoring in Brand Portal può essere pianificata per una data, un’ora successive.

Allo stesso modo, le risorse pubblicate possono essere rimosse dal portale in una data (ora) successiva, pianificando il flusso di lavoro Annulla pubblicazione da Brand Portal.

### Alias tenant configurabile nell&#39;URL

Le organizzazioni possono personalizzare l’URL del portale utilizzando un prefisso alternativo nell’URL. Per ottenere un alias per il nome del tenant nell’URL del portale esistente, le organizzazioni devono contattare il supporto Adobe.

Tieni presente che solo il prefisso dell’URL Brand Portal può essere personalizzato e non l’intero URL.
Ad esempio, un&#39;organizzazione con un dominio esistente `wknd.brand-portal.adobe.com` può ottenere `wkndinc.brand-portal.adobe.com` creato su richiesta.

Tuttavia, l’istanza di AEM Author può essere [configurato](https://helpx.adobe.com/it/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html) solo con l’URL dell’ID tenant e non con l’URL dell’alias del tenant (alternativo).

**Caso d’uso** : Le organizzazioni possono soddisfare le proprie esigenze di branding personalizzando l’URL del portale, anziché attenersi all’URL fornito dall’Adobe.

## Funzionalità e miglioramenti di Brand Portal Dicembre 2018{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### Accesso come ospite

AEM Brand Portal consente l’accesso al portale agli utenti ospiti. Un utente ospite non richiede le credenziali per accedere al portale e può accedere e scaricare tutte le cartelle e le raccolte pubbliche. Gli utenti ospiti possono aggiungere risorse al proprio light-box (raccolta privata) e scaricare lo stesso. Possono anche visualizzare i predicati di ricerca e ricerca per tag avanzati impostati dagli amministratori. La sessione guest non consente agli utenti di creare raccolte e ricerche salvate o condividerle ulteriormente, di accedere alle impostazioni delle cartelle e delle raccolte e di condividere le risorse come collegamenti.

### Download accelerato

Gli utenti Brand Portal possono sfruttare i download veloci basati su Aspera per ottenere velocità fino a 25 volte più veloci e godere di un&#39;esperienza di download senza soluzione di continuità indipendentemente dalla loro posizione in tutto il mondo. Per scaricare più rapidamente le risorse da Brand Portal o dal collegamento condiviso, gli utenti devono selezionare l’opzione Abilita accelerazione download nella finestra di dialogo di download, purché l’accelerazione di download sia abilitata nella propria organizzazione.

* [Guida per accelerare i download da Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Server di test di Aspera Connect](https://test-connect.asperasoft.com/)

### Report di accesso utente

È stato introdotto un nuovo rapporto per tenere traccia degli accessi utente. Il rapporto User Logins può essere utile per consentire alle organizzazioni di eseguire il controllo e mantenere un controllo sugli amministratori delegati e sugli altri utenti di Brand Portal.

I registri dei rapporti mostrano nomi, ID e-mail, utenti tipo (amministratore, visualizzatore, editor, ospite), gruppi, ultimo accesso, stato dell’attività e conteggio di accesso di ciascun utente.

### Accesso alle rappresentazioni originali

Gli amministratori possono limitare l&#39;accesso dell&#39;utente ai file immagine originali (jpeg, tiff, png, bmp, gif, pjpeg, x-portatile-anymap, x-portatile-bitmap, x-portatile-graymap, x-portatile-pixmap, x-rgb, x-xpixmap, x-xpixmap, x-icon, image/photoshop, image/x-photoshop, psd, image/vnd.adobe.photoshop) e consente di accedere alle rappresentazioni a bassa risoluzione scaricate da Brand Portal o da un collegamento condiviso. Questo accesso può essere controllato a livello di gruppo di utenti dalla scheda Gruppi della pagina Ruoli utente nel pannello Strumenti di amministrazione.

### Nuove configurazioni

Sono state aggiunte sei nuove configurazioni per consentire agli amministratori di abilitare/disabilitare le seguenti funzionalità per tenant specifici:

* Consenti accesso come ospite
* Consenti agli utenti di richiedere l&#39;accesso a Brand Portal
* Consenti agli amministratori di eliminare le risorse da Brand Portal
* Consentire la creazione di collezioni pubbliche
* Consenti creazione di raccolte avanzate pubbliche
* Consenti accelerazione download

### Altri miglioramenti

* *Percorso gerarchia cartelle nelle viste a schede e a elenco* — consente agli utenti di conoscere il percorso delle cartelle memorizzate in un&#39;istanza di Brand Portal. Consente agli utenti di differenziare le cartelle con lo stesso nome all’interno di una gerarchia di cartelle diversa.
* *Opzione Panoramica* — fornisce metadati relativi alla risorsa o alla cartella agli utenti non amministratori selezionando la risorsa o la cartella e quindi l’opzione di panoramica dalla barra degli strumenti. Attualmente, visualizza titolo, data creata e percorso

### Adobe I/O Hosts UI per configurare le integrazioni oAuth

Brand Portal utilizza Adobe I/O [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) Interfaccia per creare un’applicazione JWT, che consente di configurare integrazioni oAuth per consentire l’integrazione di AEM Assets con Brand Portal. In precedenza, l’interfaccia utente per la configurazione delle integrazioni OAuth era ospitata in `https://marketing.adobe.com/developer/`. Per ulteriori informazioni sull’integrazione di AEM Assets con Brand Portal per la pubblicazione di risorse e raccolte in Brand Portal, consulta [Configurare l’integrazione AEM Assets con Brand Portal](https://helpx.adobe.com/it/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html).

## Funzionalità e miglioramenti di Brand Portal febbraio 2018{#brand-portal-features-and-enhancements-632}

Nuove funzionalità avanzate orientate all&#39;allineamento di Brand Portal con AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### Miglioramenti alla navigazione

* Interfaccia utente aggiornata che si allinea al AEM e utilizza l’interfaccia utente Coral3.
* Accesso rapido e semplice agli strumenti amministrativi attraverso il nuovo logo Adobe.
* Navigazione del prodotto attraverso una sovrapposizione
* Navigazione rapida alle cartelle principali da una cartella figlio.
* Opzione Omnisearch per passare a strumenti e contenuti amministrativi.
* La vista a schede, la vista a elenco e la vista a colonne consentono di spostarsi facilmente tra le cartelle nidificate.
* L’ordinamento delle risorse in Vista a elenco non è più limitato al numero di risorse visualizzate sullo schermo. Tutte le risorse in una cartella vengono ordinate.

### Miglioramenti alla ricerca

* La funzionalità Omnisearch ti consente di eseguire una ricerca rapida per risorse e file all’interno di Brand Portal.
* Omnisearch fornisce anche un’opzione per cercare le risorse all’interno di una cartella o una posizione specifica
* Suggerimenti automatici per parole chiave per facilitare la ricerca
* Migliorate la ricerca Omnisearch con altri filtri. Opzione per salvare il risultato della ricerca in una Raccolta avanzata in modo da poter visitare nuovamente la ricerca in un secondo momento.
* Supporta la ricerca di risorse con tag avanzati
* AEM le risorse con tag avanzati possono essere condivise da AEM a Brand Portal e utilizzate i tag avanzati per la ricerca delle risorse in Brand Portal.

### Miglioramenti alla condivisione dei file

* L’utente può condividere una risorsa utilizzando l’opzione di condivisione del collegamento.
* Durante la condivisione delle risorse, l’utente deve impostare una data di scadenza per ciascuna risorsa. Offre agli utenti un maggiore controllo sulle risorse condivise.
* Un utente esterno con collegamento Condivisione risorse può scaricare l’immagine e visualizzarne le proprietà.
* La gerarchia delle cartelle nidificate originale viene mantenuta per le cartelle di risorse scaricate.

### Funzionalità di reporting e amministrazione

* È ora possibile pubblicare lo schema metadati da AEM Assets da AEM a Brand Portal.
* Gli amministratori possono creare e gestire tre tipi di rapporti: risorse scaricate, scadute e pubblicate
* Possibilità di configurare la colonna da includere nel rapporto.
* Creare predefiniti immagine per le risorse in Brand Portal.
* Possibilità di modificare Admin Search Rail Form o Search Forms per includere opzioni di filtro aggiuntive.
* Aggiorna e visualizza in anteprima lo sfondo personalizzato per il tuo marchio
* Rapporto sull’utilizzo per conoscere il numero di utenti, lo spazio di archiviazione utilizzato e le risorse totali.

## Risorse aggiuntive{#additional-resources}

* [Novità in Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [Agenti di replica di AEM Author](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [Guida al download accelerato](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Documentazione di AEM Assets Brand Portal Adobe](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html)
* [AEM Assets Dynamic Media Adobe Docs](https://experienceleague.adobe.com/docs/)
* [Scarica Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Server di test di Aspera Connect](https://test-connect.asperasoft.com/)
