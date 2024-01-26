---
title: Utilizzo di Brand Portal
description: Video introduttivi sull’integrazione di AEM Author e AEM Assets Brand Portal.
feature: Brand Portal
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-15T00:00:00Z
doc-type: Feature Video
exl-id: 42f13a19-52bf-413d-a141-63f1f0910dce
duration: 2546
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1703'
ht-degree: 0%

---

# Utilizzo di Brand Portal con AEM Assets{#using-brand-portal-with-aem-assets}

Guide video dell’integrazione con Adobe Experience Manager (AEM) Assets Brand Portal.

## Funzionalità e miglioramenti di Brand Portal settembre 2019

In particolare, a settembre 2019 Brand Portal introduce Asset Sourcing, che velocizza la trasmissione dei contenuti e consente uno scambio rapido e semplice di risorse tra autori Experienci Manager e creativi e collaboratori di terze parti.

### Brand Portal Asset Sourcing{#asset-sourcing}

Asset Sourcing di Brand Portal viene utilizzato per raccogliere risorse da agenzie e team di terze parti, sincronizzandole facilmente con l’istanza di authoring Experience Manager per la revisione e l’utilizzo.

>[!VIDEO](https://video.tv.adobe.com/v/29365?quality=12&learn=on)

*Per utilizzare Asset Sourcing è richiesto un Experience Manager Author 6.5 SP2 (6.5.2) o versione successiva*

Revisione [Abilita Autore Experience Manager per Asset Sourcing](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/brand-portal-asset-sourcing.html?lang=it) per istruzioni su come configurare e impostare Asset Sourcing in Experience Manager Author.

## Funzionalità e miglioramenti di Brand Portal febbraio 2019{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

La versione di febbraio 2019 di Brand Portal si concentra sui miglioramenti apportati alla ricerca di testo e alle principali richieste dei clienti.

### Miglioramenti alla ricerca

Brand Portal migliora la ricerca con la ricerca di testo parziale nel predicato delle proprietà nel riquadro di filtro. Per consentire la ricerca di testo parziale, è necessario abilitare la ricerca parziale nel predicato Proprietà nel modulo di ricerca.

Ulteriori informazioni sulla ricerca parziale di testo e caratteri jolly.

#### Ricerca frase parziale

È ora possibile cercare le risorse specificando solo una parte, ovvero una parola o due, della frase cercata nel riquadro di filtraggio.

**Caso d’uso** : la ricerca per frase parziale è utile quando non si è sicuri della combinazione esatta di parole che si verificano nella frase cercata.

Ad esempio, se il modulo di ricerca in Brand Portal utilizza il predicato Proprietà per la ricerca parziale nel titolo delle risorse, specificando il termine camp vengono restituite tutte le risorse il cui titolo contiene il termine camp.

#### Ricerca con caratteri jolly

Brand Portal consente di utilizzare l’asterisco (*) nella query di ricerca insieme a una parte della parola nella frase cercata.

**Caso d’uso** :Se non si è certi delle parole esatte presenti nella frase cercata, è possibile utilizzare una ricerca con caratteri jolly per colmare le lacune nella query di ricerca.

Se ad esempio si specifica climb*, tutte le risorse che contengono parole che iniziano con i caratteri climb nella frase titolo vengono restituite se il modulo di ricerca di Brand Portal utilizza il predicato Proprietà per la ricerca parziale nel titolo delle risorse.

Analogamente, specificando:

* \*climb restituisce tutte le risorse il cui titolo contiene parole che terminano con i caratteri climb.
* \*climb\* restituisce tutte le risorse contenenti parole che comprendono i caratteri climb nella frase del titolo.

#### Abilita gerarchia cartelle

Ora gli amministratori possono configurare il modo in cui le cartelle vengono mostrate agli utenti non amministratori (editor, visualizzatori e utenti ospiti) al momento dell’accesso.
[Abilita gerarchia cartelle](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) La configurazione di viene aggiunta in Impostazioni generali, nel pannello Strumenti di amministrazione. Se la configurazione è:

* Attivata, la struttura di cartelle che inizia dalla cartella principale è visibile agli utenti non amministratori. In questo modo, puoi offrire loro un’esperienza di navigazione simile a quella degli amministratori.
* Disabilitata, nella pagina di destinazione vengono visualizzate solo le cartelle condivise.

[Abilita gerarchia cartelle](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) (se abilitata) consente di distinguere le cartelle con gli stessi nomi condivisi da gerarchie diverse. Al momento dell’accesso, gli utenti non amministratori ora visualizzano le cartelle principali virtuali (e precedenti) delle cartelle condivise.

Le cartelle condivise sono organizzate nelle rispettive directory in cartelle virtuali. È possibile riconoscere queste cartelle virtuali con un&#39;icona di blocco.

La miniatura predefinita delle cartelle virtuali è l&#39;immagine miniatura della prima cartella condivisa.

### Supporto delle rappresentazioni video di Dynamic Medie

Gli utenti la cui istanza di AEM Author è in modalità ibrida Dynamic Medie possono visualizzare in anteprima e scaricare le rappresentazioni Dynamic Media, oltre ai file video originali.

Per consentire l’anteprima e il download delle rappresentazioni di elementi multimediali dinamici su account tenant specifici, gli amministratori devono specificare Configurazione Dynamic Medie (URL servizio video (DM-Gateway URL) e ID registrazione per recuperare il video dinamico) nella configurazione video dal pannello degli strumenti di amministrazione.

I video Dynamic Medie possono essere visualizzati in anteprima su:

* Pagina dettagli risorsa
* Vista a schede della risorsa
* Pagina di anteprima condivisione collegamenti

Le codifiche video Dynamic Medie possono essere scaricate da:

* Brand Portal
* Collegamento condiviso

### Pubblicazione pianificata in Brand Portal

Flusso di lavoro di pubblicazione di risorse (e cartelle) da [AEM (6.4.2.0)](https://helpx.adobe.com/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011) L’istanza di authoring in Brand Portal può essere pianificata per una data e un’ora successive.

Allo stesso modo, le risorse pubblicate possono essere rimosse dal portale in una data (ora) successiva, pianificando il flusso di lavoro Annulla pubblicazione da Brand Portal.

### Alias tenant configurabile nell’URL

Le organizzazioni possono personalizzare l’URL del portale con un prefisso alternativo nell’URL. Per ottenere un alias per il nome del tenant nell’URL del portale esistente, le organizzazioni devono contattare il supporto Adobe.

Tieni presente che solo il prefisso dell’URL di Brand Portal può essere personalizzato e non l’intero URL.
Ad esempio, un’organizzazione con un dominio esistente `wknd.brand-portal.adobe.com` può ottenere `wkndinc.brand-portal.adobe.com` creato su richiesta.

Tuttavia, l’istanza di authoring dell’AEM può essere [configurato](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html) solo con l’URL dell’ID tenant e non con l’URL dell’alias tenant (alternativo).

**Caso d’uso** : le organizzazioni possono soddisfare le loro esigenze di branding personalizzando l’URL del portale, anziché attenersi all’URL fornito dall’Adobe.

## Funzionalità e miglioramenti di Brand Portal Dicembre 2018{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707?quality=12&learn=on)

### Accesso come ospite

Il Portale marchio AEM consente l’accesso come ospite al portale. Un utente guest non richiede credenziali per accedere al portale e può accedere e scaricare tutte le cartelle e le raccolte pubbliche. Gli utenti ospiti possono aggiungere risorse alla propria light box (raccolta privata) e scaricarle. Possono inoltre visualizzare i predicati di ricerca e ricerca di tag avanzati impostati dagli amministratori. La sessione guest non consente agli utenti di creare raccolte e ricerche salvate o di condividerle ulteriormente, di accedere alle impostazioni delle cartelle e delle raccolte e di condividere le risorse come collegamenti.

### Download accelerato

Gli utenti di Brand Portal possono sfruttare i download rapidi basati su Aspera per ottenere velocità fino a 25 volte più veloci e godere di un’esperienza di download fluida indipendentemente dalla loro posizione in tutto il mondo. Per scaricare le risorse più rapidamente da Brand Portal o da un collegamento condiviso, gli utenti devono selezionare l’opzione Abilita accelerazione di download nella finestra di dialogo di download, a condizione che l’accelerazione di download sia abilitata nella loro organizzazione.

* [Guida per accelerare i download da Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Server di prova Aspera Connect](https://test-connect.asperasoft.com/)

### Rapporto di accesso utente

È stato introdotto un nuovo rapporto, per tenere traccia degli accessi degli utenti. Il rapporto sugli accessi degli utenti può essere utile per consentire alle organizzazioni di effettuare controlli e tenere sotto controllo gli amministratori delegati e gli altri utenti di Brand Portal.

I registri del rapporto mostrano nomi, ID e-mail, utenti tipo (amministratore, visualizzatore, editor, ospite), gruppi, ultimo accesso, stato attività e conteggio di accesso di ciascun utente.

### Accesso alle rappresentazioni originali

Gli amministratori possono limitare l’accesso dell’utente ai file immagine originali (jpeg, tiff, png, bmp, gif, pjpeg, x-portable-anymap, x-portable-bitmap, x-portable-graymap, x-portable-pixmap, x-rgb, x-xbitmap, x-icon, image/photoshop, image/x-photoshop, psd, image/vnd.adobe.photoshop) e dare accesso alle rappresentazioni a bassa risoluzione che scaricano da Brand Portal o da un collegamento condiviso. Questo accesso può essere controllato a livello di gruppo di utenti dalla scheda Gruppi della pagina Ruoli utente nel pannello Strumenti di amministrazione.

### Nuove configurazioni

Sono state aggiunte sei nuove configurazioni che consentono agli amministratori di abilitare/disabilitare le seguenti funzionalità su tenant specifici:

* Consenti accesso come ospite
* Consenti agli utenti di richiedere l’accesso a Brand Portal
* Consenti agli amministratori di eliminare le risorse da Brand Portal
* Consenti creazione di raccolte pubbliche
* Consenti creazione di raccolte avanzate pubbliche
* Consenti accelerazione di download

### Altri miglioramenti

* *Percorso gerarchia cartelle nelle viste a schede e a elenco* — consente agli utenti di conoscere la posizione delle cartelle memorizzate in un&#39;istanza di Brand Portal. Consente agli utenti di distinguere le cartelle con lo stesso nome all’interno di una gerarchia di cartelle diversa.
* *Opzione Panoramica* — fornisce agli utenti non amministratori i metadati sulla risorsa o sulla cartella selezionando la risorsa o la cartella e selezionando l&#39;opzione panoramica nella barra degli strumenti. Attualmente, mostra titolo, data di creazione e percorso

### Adobe I/O di interfaccia utente di Hosts per configurare le integrazioni OAuth

Brand Portal utilizza Adobe I/O [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) per la creazione di un’applicazione JWT, che consente di configurare le integrazioni OAuth per consentire l’integrazione di AEM Assets con Brand Portal. In precedenza, l’interfaccia utente per la configurazione delle integrazioni OAuth era ospitata in `https://marketing.adobe.com/developer/`. Per ulteriori informazioni sull’integrazione di AEM Assets con Brand Portal per la pubblicazione di risorse e raccolte in Brand Portal, consulta [Configurare l’integrazione di AEM Assets con Brand Portal](https://helpx.adobe.com/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html).

## Funzionalità e miglioramenti di Brand Portal febbraio 2018{#brand-portal-features-and-enhancements-632}

Nuove funzionalità ottimizzate orientate all’allineamento di Brand Portal con AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

### Miglioramenti alla navigazione

* Interfaccia utente aggiornata che è allineata con l&#39;AEM e utilizza l&#39;interfaccia utente Coral3.
* Accesso rapido e semplice agli strumenti amministrativi tramite il nuovo logo Adobe.
* Navigazione del prodotto attraverso una sovrapposizione
* Spostamento rapido alle cartelle principali da una cartella secondaria.
* Opzione Omnisearch per passare agli strumenti e ai contenuti di amministrazione.
* La vista a schede, la vista a elenco e la vista a colonne consentono di esplorare facilmente le cartelle nidificate.
* L’ordinamento delle risorse nella vista a elenco non è più limitato al numero di risorse visualizzate sullo schermo. Tutte le risorse di una cartella sono ordinate.

### Miglioramenti alla ricerca

* La funzionalità Omnisearch consente di eseguire una ricerca rapida di risorse e file in Brand Portal.
* Omnisearch fornisce inoltre un’opzione per cercare le risorse all’interno di una cartella o posizione specifica
* Suggerimenti automatici di parole chiave per semplificare la ricerca
* Migliora Omnisearch con filtri aggiuntivi. Opzione per salvare i risultati della ricerca in una raccolta avanzata in modo da poter rivedere la ricerca in un secondo momento.
* Supporta la ricerca di risorse con tag avanzati
* Le risorse con tag avanzati AEM possono essere condivise dall’AEM a Brand Portal e utilizzate per la ricerca di risorse in Brand Portal.

### Miglioramenti alla condivisione dei file

* L’utente può condividere una risorsa utilizzando l’opzione di condivisione del collegamento.
* Durante la condivisione delle risorse, l’utente può impostare una data di scadenza per ogni risorsa. Offre agli utenti un maggiore controllo sulle risorse condivise.
* Un utente esterno con il collegamento di condivisione delle risorse può scaricare l’immagine e visualizzarne le proprietà.
* La gerarchia di cartelle nidificate originale viene mantenuta per le cartelle di risorse scaricate.

### Funzionalità di reporting e amministrazione

* Ora è possibile pubblicare lo schema metadati da AEM Assets dall’AEM a Brand Portal.
* Gli amministratori possono creare e gestire tre tipi di rapporti: risorse scaricate, scadute e pubblicate
* Possibilità di configurare la colonna da includere nel rapporto.
* Creare predefiniti immagine per le risorse in Brand Portal.
* Possibilità di modificare il modulo della barra di ricerca dell’amministratore o il Forms di ricerca per includere opzioni di filtro aggiuntive.
* Aggiornare e visualizzare in anteprima lo sfondo personalizzato per il tuo marchio
* Rapporto sull’utilizzo per conoscere il numero di utenti, lo spazio di archiviazione utilizzato e le risorse totali.

## Risorse aggiuntive{#additional-resources}

* [Novità di Brand Portal](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/introduction/whats-new.html?lang=it#introduction)
* [Agenti di replica di creazione AEM](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [Guida al download accelerato](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Documentazione di AEM Assets Brand Portal Adobe](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html)
* [Documentazione di AEM Assets Dynamic Medie Adobe](https://experienceleague.adobe.com/docs/)
* [Scarica Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Server di prova Aspera Connect](https://test-connect.asperasoft.com/)
