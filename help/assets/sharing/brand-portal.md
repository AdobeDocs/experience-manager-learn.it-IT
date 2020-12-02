---
title: Utilizzo di Brand Portal
seo-title: Utilizzo di Brand Portal con  AEM Assets
description: Video introduttivi sull'integrazione di AEM Author e  AEM Assets Brand Portal.
seo-description: Video introduttivi sull'integrazione di AEM Author e  AEM Assets Brand Portal.
feature: brand-portal
topics: authoring, sharing, collaboration, search, integrations, publishing, metadata, images, renditions, administration
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
team: tm
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '1788'
ht-degree: 1%

---


# Utilizzo di Brand Portal con  AEM Assets{#using-brand-portal-with-aem-assets}

Guide video sull’integrazione di Adobe Experience Manager (AEM) Assets Brand Portal.

## Brand Portal settembre 2019 Funzionalità e miglioramenti

Il portale Brand, settembre 2019, presenta in particolare Asset Sourcing, che aumenta la velocità dei contenuti e consente uno scambio rapido e semplice di risorse tra autori di  Experienci Manager e creativi e collaboratori di terze parti.

### Origine risorsa Brand Portal{#asset-sourcing}

Asset Sourcing di Brand Portal viene utilizzato per raccogliere le risorse da agenzie e team di terze parti, sincronizzandole in modo semplice con  Experience Manager Author per la revisione e l&#39;utilizzo.

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*Experience Manager Per utilizzare Asset Sourcing è richiesto Author 6.5 SP2 (6.5.2) o versione successiva*

Per istruzioni su come configurare e impostare l&#39;origine delle risorse in  Experience Manager, consultate [Abilita autore  Experience Manager per l&#39;origine delle risorse](https://docs.adobe.com/content/help/en/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/configure-asset-sourcing-in-aem/brand-portal-enable-asset-sourcing.html).

## Brand Portal febbraio 2019 Feature e miglioramenti{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

La versione di febbraio 2019 del Brand Portal si concentra sui miglioramenti apportati alla ricerca di testo e alle richieste dei clienti principali.

### Miglioramenti della ricerca

Brand Portal ottimizza la ricerca con la ricerca parziale del testo sul predicato delle proprietà nel riquadro di filtraggio. Per consentire la ricerca parziale del testo, è necessario abilitare la ricerca parziale nel predicato delle proprietà nel modulo di ricerca.

Continua a leggere per saperne di più sulla ricerca parziale di testo e caratteri jolly.

#### Ricerca di frasi parziali

È ora possibile cercare le risorse specificando solo una parte, ovvero una parola o due, della frase ricercata nel riquadro di filtraggio.

**Caso**  di utilizzo: La ricerca di frasi parziali è utile quando non si è sicuri della combinazione esatta di parole che si verificano nella frase cercata.

Ad esempio, se il modulo di ricerca in Brand Portal utilizza Property Predicate per la ricerca parziale sul titolo delle risorse, specificando il termine camp vengono restituite tutte le risorse con la parola camp nella frase del titolo.

#### Ricerca con caratteri jolly

Il Portale marchio consente di utilizzare l&#39;asterisco (*) nella query di ricerca insieme a una parte della parola nella frase cercata.

**Caso**  di utilizzo:se non sei sicuro delle parole esatte che si trovano nella frase ricercata, puoi usare una ricerca con caratteri jolly per riempire gli spazi vuoti nella query di ricerca.

Ad esempio, se specificate climb*, tutte le risorse con parole che iniziano con i caratteri che salgono nella frase del titolo se nel modulo di ricerca in Brand Portal viene utilizzato Predicato proprietà per la ricerca parziale sul titolo delle risorse.

Analogamente, specificando:

* \*climb restituisce tutte le risorse con parole che terminano con caratteri che salgono nella frase del titolo.
* \*climb\* restituisce tutte le risorse con parole contenenti i caratteri che salgono nella frase del titolo.

#### Abilita gerarchia cartelle

Gli amministratori possono ora configurare il modo in cui le cartelle vengono visualizzate agli utenti non amministratori (editor, visualizzatori e utenti ospiti) al momento dell&#39;accesso.
[Abilita configurazione ](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) gerarchia cartelle viene aggiunta in Impostazioni generali, nel pannello degli strumenti di amministrazione. Se la configurazione è:

* Attivata, la struttura delle cartelle che inizia dalla cartella principale è visibile agli utenti non amministratori. Pertanto, gli viene concessa un&#39;esperienza di navigazione simile agli amministratori.
* Disattivato, solo le cartelle condivise vengono visualizzate sulla pagina di destinazione.

[Abilita la funzionalità ](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) Gerarchia cartelle (se abilitata) consente di distinguere le cartelle con gli stessi nomi condivisi da gerarchie diverse. Al momento dell&#39;accesso, gli utenti non amministratori ora visualizzano le cartelle padre (e antenati) virtuali delle cartelle condivise.

Le cartelle condivise sono organizzate nelle rispettive directory nelle cartelle virtuali. È possibile riconoscere queste cartelle virtuali con un&#39;icona a forma di lucchetto.

La miniatura predefinita delle cartelle virtuali è la miniatura della prima cartella condivisa.

### Supporto delle rappresentazioni video per contenuti multimediali dinamici

Gli utenti la cui istanza di AEM Author è attivata dalla modalità Dynamic Media ibrida possono visualizzare in anteprima e scaricare le rappresentazioni per contenuti multimediali dinamici, oltre ai file video originali.

Per consentire l’anteprima e il download delle rappresentazioni multimediali dinamiche su account tenant specifici, gli amministratori devono specificare la configurazione di elementi multimediali dinamici (URL del servizio video (URL DM-Gateway) e l’ID di registrazione per recuperare il video dinamico) nella configurazione video dal pannello degli strumenti di amministrazione.

È possibile visualizzare in anteprima i video per elementi multimediali dinamici su:

* Pagina dei dettagli della risorsa
* Vista a schede della risorsa
* Pagina di anteprima della condivisione dei collegamenti

Le codifiche video per contenuti multimediali dinamici possono essere scaricate da:

* Brand Portal
* Collegamento condiviso

### Pubblicazione pianificata su Brand Portal

Il flusso di lavoro di pubblicazione delle risorse (e delle cartelle) da [AEM (6.4.2.0)](https://helpx.adobe.com/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011) Istanza autore a Brand Portal può essere pianificato per una data e un&#39;ora successive.

Allo stesso modo, le risorse pubblicate possono essere rimosse dal portale in una data (ora) successiva, pianificando il flusso di lavoro Annulla pubblicazione da Brand Portal.

### Alias tenant configurabile nell&#39;URL

Le organizzazioni possono personalizzare l’URL del proprio portale utilizzando un prefisso alternativo nell’URL. Per ottenere un alias per il nome del tenant nell&#39;URL del portale esistente, le organizzazioni devono contattare  supporto del Adobe.

Tenete presente che è possibile personalizzare solo il prefisso dell’URL del Portale marchio e non l’intero URL.
Ad esempio, un&#39;organizzazione con un dominio esistente `wknd.brand-portal.adobe.com` può ottenere `wkndinc.brand-portal.adobe.com` creato su richiesta.

Tuttavia, l&#39;istanza di AEM Author può essere [configurata](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html) solo con l&#39;URL tenant e non con l&#39;URL tenant (alternativo).

**Caso**  di utilizzo: Le organizzazioni possono soddisfare le proprie esigenze di branding personalizzando l&#39;URL del portale, invece di attingere all&#39;URL fornito dal  Adobe.

## Brand Portal dicembre 2018 Feature e miglioramenti{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### Accesso come ospite

AEM Portale marchio consente agli ospiti di accedere al portale. Un utente ospite non richiede le credenziali per entrare nel portale e può accedere e scaricare tutte le cartelle e le raccolte pubbliche. Gli utenti ospiti possono aggiungere risorse alla propria light-box (raccolta privata) e scaricare la stessa. Inoltre, possono visualizzare i predicati di ricerca e smart tag impostati dagli amministratori. La sessione guest non consente agli utenti di creare raccolte e ricerche salvate né di condividerle ulteriormente, accedere alle impostazioni delle cartelle e delle raccolte e condividere le risorse come collegamenti.

### Download accelerato

Gli utenti di Brand Portal possono sfruttare i download veloci basati su Aspera per ottenere velocità fino a 25 volte più veloci e godere di un&#39;esperienza di download senza soluzione di continuità indipendentemente dalla loro posizione in tutto il mondo. Per scaricare più rapidamente le risorse dal Brand Portal o dal collegamento condiviso, gli utenti devono selezionare l’opzione Abilita accelerazione download nella finestra di dialogo di download, a condizione che l’accelerazione del download sia abilitata nell’organizzazione.

* [Guida per accelerare i download da Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera Connect Test Server](https://test-connect.asperasoft.com/)

### Rapporto login utente

È stato introdotto un nuovo rapporto per tenere traccia degli accessi utente. Il rapporto Login utente può essere utile per consentire alle organizzazioni di eseguire il controllo e controllare gli amministratori delegati e gli altri utenti di Brand Portal.

I registri dei rapporti contengono nomi visualizzati, ID e-mail, personalità (amministratore, visualizzatore, editor, ospite), gruppi, ultimo login, stato dell&#39;attività e conteggio di login di ciascun utente.

### Accesso alle rappresentazioni originali

Gli amministratori possono limitare l&#39;accesso degli utenti ai file di immagine originali (jpeg, tiff, png, bmp, gif, pjpeg, x-portable-anymap, x-portatile-bitmap, x-portabile-grigio, x-portatile-pixmap, x-rgb, x-xbitmap, x-xpixmap, x-icon, image/photoshop, image/x-photoshop, psd, image/vnd.adobe.photoshop) e consente di accedere alle rappresentazioni a bassa risoluzione scaricate dal Portale marchio o dal collegamento condiviso. Questo accesso può essere controllato a livello di gruppo di utenti dalla scheda Gruppi della pagina Ruoli utente nel pannello degli strumenti di amministrazione.

### Nuove configurazioni

Sono state aggiunte sei nuove configurazioni per consentire agli amministratori di abilitare/disabilitare le seguenti funzionalità per tenant specifici:

* Consenti accesso come ospite
* Consentire agli utenti di richiedere l&#39;accesso a Brand Portal
* Consenti agli amministratori di eliminare le risorse dal portale dei marchi
* Consenti creazione di raccolte pubbliche
* Consenti creazione di raccolte pubbliche intelligenti
* Consenti accelerazione download

### Altri miglioramenti

* *Percorso della gerarchia delle cartelle nelle viste*  scheda ed elenco consente agli utenti di conoscere il percorso delle cartelle memorizzate in un’istanza di Brand Portal. Consente agli utenti di distinguere le cartelle con lo stesso nome all’interno di una gerarchia di cartelle diversa.
* *Opzione*  Panoramica: fornisce metadati agli utenti non amministratori sulla risorsa o sulla cartella selezionando la risorsa o la cartella, quindi selezionando l’opzione della panoramica dalla barra degli strumenti. Attualmente, visualizza titolo, data di creazione e percorso

###  interfaccia utente host Adobe I/O per configurare le integrazioni di autenticazione

Brand Portal utilizza &#39;interfaccia Adobe I/O [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) per creare un&#39;applicazione JWT, che consente di configurare integrazioni Auth per consentire &#39;integrazione AEM Assets con Brand Portal. In precedenza, l&#39;interfaccia utente per la configurazione delle integrazioni OAuth era ospitata in `https://marketing.adobe.com/developer/`. Per ulteriori informazioni sull&#39;integrazione  AEM Assets con Brand Portal per la pubblicazione di risorse e raccolte in Brand Portal, consultate [Configurare &#39;integrazione AEM Assets con Brand Portal](https://helpx.adobe.com/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html).

## Brand Portal febbraio 2018 Feature e miglioramenti{#brand-portal-features-and-enhancements-632}

Nuove funzionalità migliorate orientate all&#39;allineamento del Portale del marchio con AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### Miglioramenti della navigazione

* Interfaccia utente aggiornata che si allinea al AEM e utilizza l’interfaccia utente Coral3.
* Accesso rapido e semplice agli strumenti amministrativi tramite il nuovo logo del Adobe .
* Navigazione del prodotto attraverso una sovrapposizione
* Navigazione rapida alle cartelle principali da una cartella secondaria.
* Opzione di ricerca per passare a strumenti e contenuti amministrativi.
* Le visualizzazioni a schede, a elenco e a colonne consentono di sfogliare facilmente le cartelle nidificate.
* L’ordinamento delle risorse in visualizzazione Elenco non è più limitato al numero di risorse visualizzate sullo schermo. Vengono ordinate tutte le risorse presenti in una cartella.

### Miglioramenti nella ricerca

* La funzionalità di ricerca universale consente di effettuare una ricerca rapida di risorse e file all’interno del Brand Portal.
* Omnisearch offre anche un’opzione per cercare risorse all’interno di una cartella o un percorso specifico
* Suggerimenti di parole chiave automatici per facilitare la ricerca
* Migliorate la vostra ricerca Omnisearch con altri filtri. Opzione per salvare il risultato della ricerca in una raccolta avanzata per consentire di visitare nuovamente la ricerca in un secondo momento.
* Supporta la ricerca di risorse con tag avanzati
* AEM risorse con tag avanzati possono essere condivise da AEM al Portale del marchio e utilizzate gli smart tag per la ricerca di risorse all’interno del Portale del marchio.

### Miglioramenti della condivisione dei file

* L&#39;utente può condividere una risorsa utilizzando l&#39;opzione di condivisione dei collegamenti.
* Durante la condivisione delle risorse, l’utente può impostare una data di scadenza per ciascuna risorsa. Consente agli utenti un maggiore controllo sulle risorse condivise.
* Un utente esterno con collegamento Condivisione risorse può scaricare l’immagine e visualizzarne le proprietà.
* La gerarchia di cartelle nidificate originale viene mantenuta per le cartelle di risorse scaricate.

### Funzionalità di reporting e amministrazione

* È ora possibile pubblicare lo schema di metadati  AEM Assets da AEM a Brand Portal.
* Gli amministratori possono creare e gestire tre tipi di rapporti: risorse scaricate, scadute e pubblicate
* Possibilità di configurare la colonna da includere nel rapporto.
* Creare predefiniti per immagini per le risorse in Brand Portal.
* Possibilità di modificare Admin Search Rail Form o Search Forms per includere opzioni di filtro aggiuntive.
* Aggiornare e visualizzare in anteprima lo sfondo personalizzato per il vostro marchio
* Rapporto sull’utilizzo per conoscere il numero di utenti, lo spazio di archiviazione utilizzato e le risorse totali.

## Risorse aggiuntive{#additional-resources}

* [Novità in Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [Agenti di replica di AEM Author](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [Guida al download accelerato](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [ AEM Assets Brand Portal  Adobi Docs](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html)
* [ AEM Assets Dynamic Media  Adobe Docs](https://docs.adobe.com/docs/it/aem/6-3/author/assets/dynamic-media.html)
* [Download di Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Aspera Connect Test Server](https://test-connect.asperasoft.com/)