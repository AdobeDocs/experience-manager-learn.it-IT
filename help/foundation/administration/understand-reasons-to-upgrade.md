---
title: Motivi dell'aggiornamento
description: Una suddivisione di alto livello delle funzioni chiave per i clienti che considerano l'aggiornamento all'ultima versione di Adobe Experience Manager.
version: 6.5
sub-product: risorse, gestione cloud, commercio, servizi di contenuto, contenuti multimediali dinamici, moduli, fondazioni, schermate, siti
topics: best-practices, upgrade
audience: all
activity: understand
doc-type: article
translation-type: tm+mt
source-git-commit: 1519856731758ece2860615c06fc0d64edb104a5
workflow-type: tm+mt
source-wordcount: '3540'
ht-degree: 2%

---


# Motivi dell&#39;aggiornamento

Una suddivisione di alto livello delle funzioni chiave per i clienti che considerano l&#39;aggiornamento all&#39;ultima versione di Adobe Experience Manager.

## Funzioni principali per l&#39;aggiornamento a AEM 6.5

+ [Note sulla versione di Adobe Experience Manager 6.5](https://helpx.adobe.com/it/experience-manager/6-5/release-notes.html)

### Miglioramenti di base

Adobe Experience Manager 6.5 continua a migliorare la stabilità, le prestazioni e la capacità di supporto del sistema tramite:

+ **Supporto per Java 11** (pur mantenendo il supporto per Java 8).

### Creazione e gestione di siti Web

 AEM Sites introduce una serie di funzionalità progettate per accelerare la creazione e la creazione di siti Web:

+ **Il supporto dell&#39;editor** SPA consente di creare completamente l&#39;app SPA (applicazioni a pagina singola) in AEM, per un&#39;esperienza di authoring estremamente semplice e intuitiva.
+_ SDK **JavaScript**, un kit SPA per l&#39;avvio del progetto e strumenti di generazione di supporto, consentono agli sviluppatori front-end di sviluppare applicazioni a pagina singola compatibili con SPA Editor indipendentemente da AEM.
+ **Componenti** di base aggiunge una moltitudine di nuovi componenti, una libreria **di** componenti e una serie di miglioramenti ai componenti di base esistenti.
+ Ulteriori **traduzioni** migliorano la traduzione di  AEM Sites.

### Esperienze fluide

AEM continua ad abbracciare esperienze fluide con strumenti nuovi e migliorati che facilitano l&#39;utilizzo di contenuti al di fuori dei AEM.

+ **I frammenti** di contenuto supportano il confronto delle versioni/differenze e le annotazioni.
+ **AEM Assets HTTP API** supporta l’esposizione di frammenti **di** contenuto direttamente in DAM come **JSON**.
   **I frammenti** esperienza supportano la ricerca **** full-text e l&#39;annullamento della validità della cache del dispatcher **AEM** per fare riferimento a **pagine**.

### Gestione delle risorse

 AEM Assets continua a sfruttare la vasta gamma di funzionalità di gestione delle risorse per migliorare l’utilizzo, la gestione e la comprensione di DAM. AEM 6.5 continua a migliorare l&#39;integrazione tra Adobe Creative Cloud e i flussi di lavoro creativi.

+ **Adobe Asset Link** collega i creativi direttamente a  AEM Assets dagli strumenti Adobe Creative Cloud.
+ **&#39;integrazione con Adobe Stock** consente l&#39;accesso diretto alle immagini  Adobe Stock direttamente dall&#39;esperienza  AEM Assets, creando un&#39;esperienza di scoperta dei contenuti perfetta.
+ **AEM Desktop App** release versione 2.0 e riprevede se stesso migliorando le prestazioni e la stabilità.
+ **Risorse** collegate supporta  istanze AEM Sites distinte per accedere e utilizzare in modo diretto le risorse da un’istanza AEM Assets  diversa.
+ Supporto video aggiornato in **Dynamic Media**, inclusi **360 Miniature video** e **video** personalizzate.

### Content intelligence

AEM continua a costruire la sua integrazione con tecnologie intelligenti, sfruttando l&#39;apprendimento automatico e l&#39;intelligenza artificiale per migliorare tutte le esperienze.

+ **Collegamento** risorse Adobe aggiunge la ricerca per similarità **visiva**, che consente di individuare e utilizzare facilmente immagini simili negli strumenti **** Adobe Creative Cloud.

### Integrations (Integrazioni)

AEM la sua capacità di integrarsi con altri servizi  Adobe:

+ **I frammenti** esperienza approfondiscono la loro integrazione con **Adobe Target** sostenendo **Esporta come JSON** per  Adobe Target e la possibilità di **eliminare offerte** basate su frammenti esperienza da **Adobe Target**.

### Gestione di AMS Cloud

[Cloud Manager](https://adobe.ly/2HODmsv), esclusivo per i clienti di Adobe Managed Services (AMS), offre le seguenti funzionalità:

+ Cloud Manager supporta AEM supporto di distribuzione da  AEM Sites a **AEM Assets**, compreso il test **automatico delle prestazioni dell&#39;elaborazione** delle risorse.
+ **Ridimensionamento** automatico del livello AEM Publish a soglie predefinite, per garantire un&#39;esperienza utente finale ottimale.
+ **I oleodotti** non di produzione consentono ai team di sviluppo di utilizzare Cloud Manager per controllare continuamente la qualità del codice e distribuirli in ambienti più bassi (sviluppo e QA).
+ **Le API** di pipeline CI/CD consentono ai clienti di interagire in modo programmatico con Cloud Manager, ampliando le possibilità di integrazione con l&#39;infrastruttura di sviluppo locale.

## Funzioni di base

Di seguito è riportata una matrice di caratteristiche fondamentali offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti miglioramenti incrementali aggiunti in ciascuna release.

+ [Note sulla versione di AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup>miglioramenti significativi alla funzione in questa versione.***

***✔<sup>SP</sup>indica che la funzione è disponibile tramite Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Funzione Foundation</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <strong>Supporto per Java 11:</strong> AEM supporta Java 11 (così come Java 8).
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Archivio</a>contenuto Oak:</strong> Fornisce prestazioni e scalabilità molto maggiori rispetto al predecessore Jackrabbit 2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">Supporto</a>indice oak-run.jar:</strong> È stato migliorato l’indicizzazione, la raccolta di statistiche e il controllo di coerenza degli indici Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Indici</a>di ricerca personalizzati: </strong>
                Possibilità di aggiungere definizioni di indice personalizzate per ottimizzare le prestazioni della query e la rilevanza della ricerca.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Pulizia</a>revisioni online:</strong>
                Eseguire la manutenzione dell'archivio senza tempi di inattività del server.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">Archivio</a>archivio TarMK o MongoMK:</strong>
                <br> Opzioni per utilizzare uno storage semplice e performante basato su file di TarMK (versione di nuova generazione di TarPM) <br> o scalare orizzontalmente con un repository di MongoDB con MongoMK.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">Prestazioni e stabilità</a>MongoMK:</strong>
            Sono stati apportati continui miglioramenti a MongoMK da quando è stato introdotto con AEM 6.0.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3 DataStore</a>:</strong>
            Sfruttate la soluzione di archiviazione cloud espandibile per memorizzare risorse binarie.</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Parità delle funzioni dell’interfaccia utente touch:</strong>
                Miglioramenti continui all’interfaccia utente di authoring per velocità, maggiore produttività e maggiore parità di funzionalità con l’interfaccia classica.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Omnisearch:</strong>
                Ricerca e navigazione rapide AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Pannello</a>operazioni:</strong>
 Eseguire interventi di manutenzione, monitorare lo stato del server e analizzare le prestazioni dall'interno AEM.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Miglioramenti</a>dell'aggiornamento:</strong>
            I miglioramenti dell'aggiornamento consentono di effettuare aggiornamenti interni più semplici e rapidi dei AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/htl/using/overview.html" target="_blank">HTML Template Language</a>:</strong>
            Un moderno motore di modellazione che separa la presentazione dalla logica. Riduzione significativa dei tempi di sviluppo dei componenti. Funzioni incrementali aggiunte a ciascuna versione.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Modelli</a>Sling:</strong>
            Un framework flessibile per la modellazione delle risorse JCR in oggetti aziendali e logica. Funzioni incrementali aggiunte a ciascuna versione.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>: </strong>
                Esclusivo per i clienti di Adobe Managed Services (AMS), Cloud Manager accelera lo sviluppo e la distribuzione tramite una pipeline CI/CD all'avanguardia.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Caratteristiche di sicurezza

Di seguito è riportata una matrice di funzioni di sicurezza chiave offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti miglioramenti incrementali aggiunti in ciascuna release.

+ [Note sulla versione di sicurezza](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***③ indica che sono stati apportati miglioramenti significativi alla funzionalità in questa versione.***

***✔<sup>+</sup>indica che la funzione è disponibile tramite Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Funzionalità di protezione</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Utenti</a></strong><br> del servizio Confronta le autorizzazioni per evitare l'uso non necessario dei privilegi di amministratore.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Key Store Management</a></strong><br> Global Trust Store, certificati e chiavi gestiti all'interno dell'archivio.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>Protezione CSRF</strong><strong>per la protezione</strong></a>dei documenti <br> Cross-Site Request Forgery</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong><strong>supporta</strong></a>il supporto per la condivisione delle risorse tra <br> origini per una maggiore flessibilità dell'applicazione.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/security/saml-2-0-authenticationhandler.html" target="_blank">È stato migliorato il supporto</a><br>dell'autenticazione SAML. Sono stati </strong>migliorati il reindirizzamento SAML, le informazioni sui gruppi ottimizzate e i problemi di crittografia delle chiavi.
            <br>
        </td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP come configurazione</a><br>OSGi </strong>semplifica la gestione e gli aggiornamenti dell’autenticazione LDAP.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>Il supporto per la crittografia OSGi per password<br>in testo normale </strong>Le password e altri valori sensibili può essere salvato in forma crittografata e automaticamente decrittografato.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">Miglioramenti</a><br>CUG implementazione del gruppo utenti </strong>chiusi riscritti per risolvere problemi di prestazioni e scalabilità.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">Interfaccia utente della procedura guidata</a></strong><br> SSL per semplificare la configurazione e la gestione di SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Supporto</a></strong><br> token codificati Non è più necessario per le sessioni "appiccicose" per supportare l'autenticazione orizzontale nelle istanze di pubblicazione.</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Adobe Supporto</a><br>per l'autenticazione IMS </strong>Esclusivo ai servizi gestiti Adobe (AMS), gestisci centralmente l'accesso alle istanze di AEM Author tramite  Adobe IMS ( Identity Management System).</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
    </tr>
</tbody>
</table>

## Caratteristiche dei siti

Di seguito è riportata una matrice di funzioni principali di Siti offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti miglioramenti incrementali aggiunti in ciascuna release.

+ [note sulla versione di AEM Sites](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup>miglioramenti significativi alla funzione in questa versione.***

***✔<sup>SP</sup>indica che la funzione è disponibile tramite Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td><strong>Funzionalità Siti</strong></td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Touch Optimized Page Authoring</a>:</strong>
            Consente agli editor di utilizzare tablet e computer con schermi touch.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Authoring</a>reattivo del sito:</strong>
                La modalità di layout consente agli editor di ridimensionare i componenti in base alla larghezza del dispositivo per i siti reattivi.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Modelli</a>modificabili:</strong>
            Consente agli autori specializzati di creare e modificare i modelli di pagina.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">Componenti</a>di base:</strong>
            Accelerare lo sviluppo del sito. Disponibile su GitHub per una pianificazione e flessibilità frequenti delle versioni.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">Editor</a>SPA:</strong>
            Create esperienze Web coinvolgenti e personalizzabili utilizzando i framework SPA (Single Page Application) basati su React o Angular.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/style-system-fp.html" target="_blank">Sistema</a>di stile:</strong>
            Per aumentare AEM componente, definirne l’aspetto visivo mediante il sistema di stile in-context.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Multi-Site Manager (MSM)</a>:</strong>
            Gestire più siti Web che condividono contenuti comuni (ad esempio, più lingue, più marchi).</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Traduzione</a>contenuto:</strong>
            Il framework Plug and Play si integra con i servizi di traduzione di terze parti leader del settore.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:</strong>
            Framework di ClientContext di nuova generazione per la personalizzazione dei contenuti.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Lanci</a>:</strong>
            Sviluppare contenuti per una release futura senza interrompere l'authoring quotidiano.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-understand.html" target="_blank">Frammenti</a>di contenuto:</strong>
            Create e curate contenuti editoriali scollegati dalla presentazione per un facile riutilizzo.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Frammenti</a>esperienza:</strong>
            Crea esperienze e varianti riutilizzabili ottimizzate per i canali desktop, mobili e social.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/content-services-fragments-featurepack.html" target="_blank">Content Services</a>:</strong>
            Esportate i contenuti da AEM come JSON da utilizzare per dispositivi e applicazioni.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Integrazione  Adobe Analytics e approfondimenti sui contenuti:</strong>
                Integrazione semplice di  Adobe Analytics e DTM. Visualizzare le informazioni sulle prestazioni nell'ambiente Authoring.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/it/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Integrazione</a>Adobe Target :</strong>
            Procedura guidata dettagliata per creare esperienze con targeting e creare librerie di offerte riutilizzabili.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Integrazione</a>Adobe Campaign :</strong>
            Semplice integrazione con la soluzione di campagne e-mail di nuova generazione.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Integrazione</a>lancio Adobe :</strong>
            Integrazione con  Adobe  servizio cloud di gestione tag di nuova generazione.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-screens-introduction.html" target="_blank">Schermi</a>:</strong>
            Gestione delle esperienze per il digital signage e i chioschi.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">eCommerce</a>:</strong>
            Esperienze di acquisto personalizzate e personalizzate su siti Web, dispositivi mobili e social.
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">Community</a>:</strong>
            I forum, i commenti concatenati, i calendari degli eventi e molte altre funzioni consentono di coinvolgere in modo approfondito i visitatori del sito.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Funzioni delle risorse

Di seguito è riportata una matrice di funzioni chiave di Risorse offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti miglioramenti incrementali aggiunti in ciascuna release.

+ [note sulla versione di AEM Assets](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***③ indica che sono stati apportati miglioramenti significativi alla funzionalità in questa versione.***

***✔<sup>+</sup>indica che la funzione è disponibile tramite Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Funzionalità Risorse</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Interfaccia</a>touch:</strong>
            Consente di gestire le risorse su un computer desktop o su dispositivi touch.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">Gestione</a>avanzata dei metadati:</strong>
            Modelli di metadati, Editor schema metadati e Modifica di metadati in blocco.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">Gestione attività</a> e <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank">flussi di lavoro</a> :</strong>
            Flussi di lavoro e attività pregenerati per la revisione e l’approvazione di risorse digitali che sfruttano AEM progetti.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Scalabilità e prestazioni:</strong>
            Supporto migliorato per l’assimilazione, il caricamento e l’archiviazione su scala.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API</a>HTTP Assets:</strong>
            Interagisci in modo programmatico con le risorse tramite HTTP e JSON.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Condivisione</a>collegamenti:</strong>
            Condivisione ad hoc semplice delle risorse digitali senza necessità di effettuare l’accesso.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Portale</a>marchio:</strong>
            Soluzione SAAS per servizi cloud per la condivisione e la distribuzione senza soluzione di continuità delle risorse digitali.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Risorse</a>connesse:</strong>
             istanze AEM Sites possono accedere e utilizzare facilmente le risorse da un’altra istanza  AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Approfondimenti</a>risorse:</strong>
            Sfruttate  Adobe Analytics per acquisire l'interazione con i clienti delle risorse digitali e visualizzarle in AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Risorse</a>multilingue:</strong>
            Il supporto per la traduzione automatica dei metadati delle risorse con radici della lingua.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Tag avanzati e moderazione</a>:</strong>
            Sfruttate  Adobe Sensei per assegnare automaticamente tag alle immagini con utili metadati.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Ricerca</a>traduzione intelligente:</strong>
            Traduci automaticamente i termini di ricerca durante la ricerca di  AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Integrazione</a>Adobe InDesign Server :</strong>
            Genera cataloghi di prodotti. Crea brochure, volantini e annunci per la stampa basati sui modelli  InDesign.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">App</a>desktop AEM:</strong>
            Sincronizzate le risorse sul desktop locale per modificarle con i prodotti di Creative Suite.
            </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Libreria</a>immagini Adobe:</strong>
                <br> Librerie PDF Photoshop e  Acrobat utilizzate per la manipolazione di file di alta qualità.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">collegamento</a>risorsa Adobe:</strong>
            Accesso  AEM Assets direttamente dalle applicazioni  Adobe Create Cloud.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Integrazione</a>Adobe Stock :</strong>
            Accesso e utilizzo  immagini Adobe Stock direttamente da AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

###  AEM Assets Dynamic Media

***✔<sup>+</sup>miglioramenti significativi alla funzione in questa versione.***

***✔<sup>SP</sup>indica che la funzione è disponibile tramite Service Pack o Feature Pack.***


<table>
    <thead>
        <tr>
            <td>Elemento multimediale dinamico</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3 +FP</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Imaging</a>:</strong>
            Distribuire in modo dinamico immagini di dimensioni e formati diversi, incluso Smart Crop.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Video</a>:</strong>
            Codifica video avanzata e streaming video adattivo</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">Contenuti multimediali</a>interattivi:</strong>
            Create banner interattivi e video con contenuti su cui sia possibile fare clic per mostrare le offerte chiave.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Set (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Immagine</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Rotazione</a>, File <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">Multimediali</a>Diversi):</strong>
            Consente agli utenti di ingrandire, scorrere, ruotare e simulare un’esperienza di visualizzazione di 360 gradi.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/content/dynamic-media/viewer-presets.html" target="_blank">Visualizzatori</a>:</strong>
            Lettori e predefiniti per contenuti multimediali avanzati con marchio personalizzato con supporto per schermi/dispositivi diversi.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Consegna</a>:</strong>
            Opzioni flessibili per il collegamento o l'incorporazione di contenuti multimediali dinamici e la distribuzione tramite il protocollo HTTP/2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Aggiornamento da Scene7 a Contenuti multimediali dinamici:</strong>
            Possibilità di migrare le risorse principali e continuare a utilizzare gli URL S7 esistenti.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## Funzioni Forms

Di seguito è riportata una matrice di funzioni chiave  AEM Forms Add-on offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti miglioramenti incrementali aggiunti in ciascuna release.

+ [note sulla versione di AEM Forms](https://helpx.adobe.com/experience-manager/6-5/release-notes/forms.html)

***✔<sup>+</sup>miglioramenti significativi alla funzione in questa versione.***

***✔<sup>SP</sup>indica che la funzione è disponibile tramite Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Funzionalità Forms</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Editor</a>Forms adattivo:</strong>
            Creazione di moduli coinvolgenti, reattivi e adattivi in base alle impostazioni del dispositivo e del browser.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Documento di registrazione</a>:</strong>
            Creare un documento per garantire l'archiviazione a lungo termine di un'esperienza di acquisizione dei dati o di una versione pronta per la stampa.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">Editor</a>tema:</strong>
            Creare temi riutilizzabili per lo stile di componenti e pannelli di un modulo.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Editor</a>modello:</strong>
            Standardizzazione e implementazione di procedure ottimali per i moduli adattivi.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Integrazione</a>Adobe Sign :</strong>
            Consentire la distribuzione di scenari di firma basati su moduli integrati  Adobe Sign.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Gestione</a>corrispondenza:</strong>
            Con  AEM Forms, puoi creare, gestire e distribuire corrispondenze personalizzate e interattive per i clienti.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Integrazione</a>dei dati di terze parti:</strong>
            Utilizzando l'integrazione dei dati, i dati vengono recuperati da origini dati diverse in base agli input dell'utente in un modulo. Durante l'invio del modulo, i dati acquisiti vengono riscritti alle origini dati.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Flusso di lavoro (su OSGi) per l’elaborazione</a>Forms:</strong>
            Implementazione semplificata dei processi di approvazione dei moduli.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Integrazione con il Marketing Cloud</a>:</strong>
            Integrazione con  Adobe Analytics e  Adobe Target per migliorare e misurare le esperienze dei clienti.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Form Manager</a>:</strong>
            Un'unica posizione per gestire tutti i moduli/documenti/corrispondenza, ad esempio abilitando analisi, traduzioni, test A/B, revisioni e pubblicazioni.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">app</a>AEM Forms:</strong>
            Consente l'elaborazione dei moduli online/offline all'interno di un'app su iOS, Android o Windows.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Comunicazioni</a>interattive:</strong>
            Create comunicazioni avanzate, ad esempio istruzioni con targeting, con elementi interattivi quali grafici (precedentemente denominati Documenti adattivi).</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/pdf/aem-forms/6-5/WorkbenchHelp.pdf" target="_blank">Flusso di lavoro (J2EE) per elaborazione</a>Forms:</strong>
            Creazione di moduli complessi/flussi di lavoro basati su documenti mediante un IDE intuitivo.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms Document Security</a>:</strong>
            Accesso sicuro e autorizzazione di documenti PDF e Office.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Test dei framework</a>:</strong>
            Utilizzate il framework Calvin e il plug-in Chrome per supportare ed eseguire il debug dei moduli adattivi.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## Funzioni di Communities

Di seguito è riportata una matrice di funzioni chiave  AEM Communities Add-on offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti miglioramenti incrementali aggiunti in ciascuna release.

+ [riepilogo delle nuove funzioni di AEM Communities](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html#main-pars_text)

***✔<sup>+</sup>miglioramenti significativi alla funzione in questa versione.***

***✔<sup>SP</sup>indica che la funzione è disponibile tramite Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>Funzioni di Communities</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="7">Funzioni di Communities</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">Forum</a>:</strong> (Social Component Framework) Create nuovi argomenti oppure visualizzate, seguite, cercate e spostate gli argomenti esistenti.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">QnA</a>:</strong>
                Porre, visualizzare e rispondere alle domande.</p>
            </td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">Blog</a>:</strong>
                Creare articoli e commenti di blog sul lato della pubblicazione.
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">Ideazione</a>:</strong>
                Crea e condividi idee con la community, oppure visualizza, segui e commenta le idee esistenti.
            </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">Calendario</a>:</strong>
                (Social Component Framework) Fornisce informazioni sugli eventi della community ai visitatori del sito.
            </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">Libreria</a>file:</strong>
                Caricate, gestite e scaricate i file all'interno del sito della community.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">Gruppi</a>di utenti:
            </strong>Un set di utenti può appartenere a gruppi di membri e può essere assegnato a ruoli collettivamente.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Assegnazione</a>:</strong>
            Creazione e assegnazione di risorse di apprendimento ai membri della community.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Attivazione</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank">Gestione</a> catalogo e <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">risorse</a>:</strong>
            Accedere alle risorse di abilitazione dal catalogo.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">Gestione</a>dei percorsi di apprendimento:</strong>
            Gestione di corsi o gruppi di risorse di abilitazione.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">Generazione di rapporti</a>di abilitazione:</strong>
            Generazione di rapporti sulle risorse di abilitazione e sui percorsi di apprendimento.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">Partecipazione all'abilitazione</a>:</strong>
            Aggiungere commenti sulle risorse di abilitazione.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analisi</a>abilitazione:</strong>
            Analisi video, generazione di rapporti sull'avanzamento e sulle assegnazioni</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank">Commenti</a> e allegati:</strong>
            (Quadro della componente sociale) In qualità di membro della comunità, condividete opinioni e conoscenze sui contenuti presenti sul sito Community.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Conversione dei frammenti di contenuto:</strong>
            Convertire i contributi UGC in frammenti di contenuto.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">Recensioni</a>:</strong>
                (Social Component Framework) In qualità di membro della community, potete esaminare un contenuto utilizzando una combinazione di commenti e funzioni di valutazione.</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">Valutazioni</a>:/strong&gt; (Social Component Framework) Come membro della community, valuta un contenuto.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">Voti</a>:</strong>
                (Quadro dei componenti sociali) In qualità di membro della comunità, vota o deseleziona un contenuto.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">Tag</a>:</strong>
            Associate i tag (parole chiave o etichette) al contenuto per individuare rapidamente il contenuto.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">Ricerca</a>:</strong>
            Ricerche predittive e suggestive.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">Traduzione</a>:</strong>
            Traduzione automatica del contenuto generato dall’utente.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="10">Amministrazione</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">Gestione</a>del sito:</strong>
            Creazione di siti con funzioni per community.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Modelli</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Modelli per siti</a> e <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank">gruppi</a> per la creazione guidata di siti community completamente funzionanti.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Modelli modificabili:</strong>
            Consentite agli amministratori della community di creare esperienze avanzate utilizzando AEM Modelli modificabili.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">Gruppi o sottocomunità</a>:</strong>
            Creazione dinamica di sub-community all'interno dei siti delle community.
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">Moderazione</a>:</strong>
            Moderazione del contenuto generato dall’utente.
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">Moderazione</a>di massa:</strong>
            Console di moderazione per gestire in massa il contenuto generato dall’utente.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">Filtri</a>Spam Detection e Profanity:</strong>
            Rilevamento automatico dello spam.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">Gestione</a>membri:</strong>
            Gestisci profili utente e gruppi dall'area di gestione membri.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">Design</a>reattivo:</strong>
             siti AEM Communities sono reattivi.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analytics</a>:</strong>
            Effettuate l'integrazione con  Adobe Analytics per ottenere informazioni chiave sull'utilizzo dei siti community.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="4">Membri</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">Punteggio e Badging</a>:</strong>
            (punteggio avanzato fornito da  Adobe Sensei) Identificate i membri della community come esperti e premiateli.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank">Attività</a> e <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">notifiche</a>:</strong>
            Visualizzare il flusso di attività recenti e ricevere notifiche sugli eventi di interesse.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">Messaggi</a>:</strong>
            Messaggistica diretta a utenti e gruppi.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">Login</a>social:</strong>
            Effettuate l'accesso con il proprio account Facebook o Twitter.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Platform</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP (Mongo Storage)</a>:</strong>
            Il contenuto generato dall'utente (UGC) viene mantenuto direttamente in un'istanza MongoDB locale</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP (archivio database)</a>:</strong>
            Il contenuto generato dall'utente (UGC) viene mantenuto direttamente in un'istanza di database locale MySQL.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP (archiviazione cloud)</a>:</strong>
                Il contenuto generato dall'utente (UGC) è persistente in remoto in un servizio cloud ospitato e gestito da  Adobe.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                Il contenuto della community è memorizzato in JCR e UGC è accessibile dall’istanza di creazione (o pubblicazione) alla quale è stato pubblicato.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">Sincronizzazione</a>utente e gruppo:</strong>
            Sincronizzare utenti e gruppi tra le istanze di pubblicazione quando si utilizza una topologia della farm di pubblicazione.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

 AEM Communities aggiunge [miglioramenti](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html) attraverso le release per consentire alle organizzazioni di coinvolgere e abilitare i propri utenti, tramite:

+ **Supporto di @reference** nel contenuto generato dall&#39;utente.
+ Miglioramenti all’accessibilità tramite la navigazione **** tramite tastiera nei componenti **Abilitazione** .
+ Moderazione **** in blocco migliorata con filtri **** personalizzati.
+ **Modelli** modificabili per consentire agli amministratori della community di creare esperienze di community avanzate in AEM.
+ Gli utenti ora possono inviare messaggi **diretti in blocco** a tutti i membri di un gruppo.
