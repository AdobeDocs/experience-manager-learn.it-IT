---
title: Motivi dell’aggiornamento
description: Analisi dettagliata delle funzioni chiave per i clienti che considerano l’aggiornamento all’ultima versione di Adobe Experience Manager 6.
version: 6.5
topic: Upgrade
role: Leader, Architect, Developer, Admin, User
level: Beginner
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
source-git-commit: 34fbb22916cf8a8df0e3240835c71e0979fd11bd
workflow-type: tm+mt
source-wordcount: '3460'
ht-degree: 3%

---

# Perché effettuare l’aggiornamento

Analisi dettagliata delle funzioni chiave per i clienti che considerano l’aggiornamento all’ultima versione di Adobe Experience Manager 6.

## Funzioni principali per l&#39;aggiornamento a AEM 6.5

+ [Note sulla versione di Adobe Experience Manager 6.5](https://helpx.adobe.com/it/experience-manager/6-5/release-notes.html)

### Miglioramenti di Foundation

Adobe Experience Manager 6.5 continua a migliorare la stabilità, le prestazioni e la supportabilità del sistema tramite:

+ **Java 11** (mantenendo il supporto Java 8).

### Creazione e gestione di siti web

AEM Sites introduce una serie di funzioni progettate per accelerare la creazione e la creazione di siti web:

+ **Editor SPA** il supporto consente di creare SPA (applicazioni a pagina singola) completamente in AEM, per supportare un’esperienza di authoring ricca e intuitiva.
+_ **SDK JavaScript**, un kit di avvio del progetto SPA e il supporto di strumenti di creazione, consentono agli sviluppatori front-end di sviluppare applicazioni a pagina singola compatibili con l’editor SPA indipendentemente da AEM.
+ **Componenti core** aggiunge una moltitudine di nuovi componenti, un **Libreria dei componenti** e una serie di miglioramenti ai componenti core esistenti.
+ Ulteriori informazioni **Traduzioni** i miglioramenti semplificano la traduzione di AEM Sites.

### Esperienze fluide

AEM continua ad abbracciare esperienze fluide con strumenti nuovi e migliorati che facilitano l’utilizzo di contenuti al di fuori di AEM.

+ **Frammenti di contenuto** supporto di confronto delle versioni/differenze e annotazioni.
+ **API HTTP di AEM Assets** supporti che espongono **Frammenti di contenuto** direttamente nel DAM come **JSON**.
   **Frammenti esperienza** supporto **Ricerca full-text** e **Annullamento della validità della cache del dispatcher AEM** di riferimento **Pagine**.

### Gestione delle risorse

AEM Assets continua a sfruttare le sue numerose funzionalità di gestione delle risorse per migliorare l’utilizzo, la gestione e la comprensione di DAM. AEM 6.5 continua a migliorare l’integrazione tra Adobe Creative Cloud e i flussi di lavoro creativi.

+ **Adobe Asset Link** collega i creativi direttamente ad AEM Assets dagli strumenti Adobe Creative Cloud.
+ **Adobe Stock** L’integrazione consente l’accesso diretto alle immagini Adobe Stock direttamente dall’esperienza AEM Assets, creando un’esperienza di individuazione dei contenuti perfetta.
+ **App desktop AEM** rilascia la versione 2.0 e riprevede se stesso migliorando le prestazioni e la stabilità.
+ **Risorse collegate** supporta istanze AEM Sites discrete per accedere e utilizzare in modo semplice le risorse di una diversa istanza di AEM Assets.
+ Supporto video aggiornato in **Dynamic Media**, tra cui **Video a 360°** e **Miniature video personalizzate**.

### Intelligenza dei contenuti

AEM continua a sviluppare la sua integrazione con le tecnologie intelligenti, sfruttando l’apprendimento automatico e l’intelligenza artificiale per migliorare tutte le esperienze.

+ **Adobe Asset Link** add **Ricerca per similarità visiva**, che consente di individuare e utilizzare immagini simili in **Strumenti Adobe Creative Cloud**.

### Integrazioni

AEM aumenta la sua capacità di integrarsi con altri servizi Adobe:

+ **Frammenti esperienza** approfondisce la loro integrazione con **Adobe Target** sostenendo **Esporta come JSON** ad Adobe Target e la capacità di **eliminare le offerte basate su frammenti esperienza** da **Adobe Target**.

### Gestione cloud AMS

[Cloud Manager](https://adobe.ly/2HODmsv), un’esclusiva per i clienti di Adobe Managed Services (AMS), offre le seguenti funzionalità:

+ Cloud Manager supporta l’estensione del supporto AEM distribuzione da AEM Sites a **AEM Assets**, tra cui **test automatizzato delle prestazioni dell’elaborazione delle risorse**.
+ **Ridimensionamento automatico** del livello di pubblicazione AEM a soglie predefinite, assicurati un’esperienza utente finale ottimale.
+ **gasdotti non di produzione** consente ai team di sviluppo di sfruttare Cloud Manager per controllare continuamente la qualità del codice e implementarli in ambienti più bassi (sviluppo e controllo qualità).
+ **API pipeline CI/CD** consenti ai clienti di interagire in modo programmatico con Cloud Manager, ampliando le possibilità di integrazione con l’infrastruttura di sviluppo on-premise.

## Funzioni di base

Di seguito è riportata una matrice delle caratteristiche principali offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti con miglioramenti incrementali aggiunti in ogni versione.

+ [Note sulla versione di AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***↓<sup>+</sup> miglioramenti significativi alla funzione in questa versione.***

***↓<sup>SP</sup> indica che la funzione è disponibile tramite un Service Pack o Feature Pack.***

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
                <strong>Supporto Java 11:</strong> AEM supporta Java 11 (nonché Java 8).
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Archivio dei contenuti Oak</a>:</strong> Offre prestazioni e scalabilità molto più elevate rispetto al predecessore Jackrabbit 2.</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">Supporto indice oak-run.jar</a>:</strong> È stata migliorata la re/indicizzazione, la raccolta di statistiche e il controllo di coerenza degli indici Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Indici di ricerca personalizzati</a>: </strong>
                Possibilità di aggiungere definizioni di indice personalizzate per ottimizzare le prestazioni delle query e la rilevanza della ricerca.</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/it/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Pulizia revisioni online</a>:</strong>
                Esegui la manutenzione del repository senza tempi di inattività del server.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">Archiviazione archivio TarMK o MongoMK</a>:</strong>
                <br> Opzioni per l'utilizzo di archiviazione semplice e performante basata su file di TarMK (versione di nuova generazione di TarPM)
                <br> o scalare orizzontalmente con un archivio basato su MongoDB con MongoMK.</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">Prestazioni e stabilità MongoMK</a>:</strong>
            Sono stati apportati continui miglioramenti a MongoMK da quando è stato introdotto con AEM 6.0.</td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">DataStore Amazon S3</a>:</strong>
            Utilizza una soluzione di archiviazione cloud espandibile per memorizzare risorse binarie.</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong>Parità delle funzioni dell’interfaccia touch:</strong>
                Miglioramenti continui all’interfaccia utente di authoring per velocità con maggiore produttività e parità di funzioni con l’interfaccia classica.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong>Omnisearch:</strong>
                Ricerca e navigazione rapide AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Dashboard delle operazioni</a>:</strong>
 Esegui la manutenzione, monitora lo stato del server e analizza le prestazioni dall'interno di AEM.</td>
            <td></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Miglioramenti all’aggiornamento</a>:</strong>
            I miglioramenti dell'aggiornamento consentono aggiornamenti in locale più semplici e rapidi delle AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/it/experience-manager/htl/using/overview.html" target="_blank">Linguaggio dei modelli HTL</a>:</strong>
            Un moderno motore di modellazione che separa la presentazione dalla logica. Riduce notevolmente i tempi di sviluppo dei componenti. Funzionalità incrementali aggiunte a ogni versione.</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Modelli Sling</a>:</strong>
            Un framework flessibile per modellare le risorse JCR in oggetti e logiche aziendali. Funzionalità incrementali aggiunte a ogni versione.
            </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>: </strong>
                Escluso ai clienti di Adobe Managed Services (AMS), Cloud Manager accelera lo sviluppo e la distribuzione tramite una pipeline CI/CD allo stato dell’arte.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Funzioni di sicurezza

Di seguito è riportata una matrice delle principali funzioni di sicurezza offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti con miglioramenti incrementali aggiunti in ogni versione.

+ [Note sulla versione di sicurezza](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***● indica che miglioramenti significativi alla funzione in questa versione.***

***↓<sup>+</sup> indica che la funzione è disponibile tramite un Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Funzione di sicurezza</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3.</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Utenti del servizio</a></strong>
            <br> Il partizionamento delle autorizzazioni evita l'uso non necessario dei privilegi di amministratore.</td>
        <td></td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Gestione degli archivi chiave</a></strong>
            <br> Archivio trust globale, certificati e chiavi tutti gestiti all'interno dell'archivio.</td>
        <td></td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRF</strong> <strong>protezione</strong></a>
            <br> Protezione multisito per la falsificazione delle richieste</td>
        <td></td>
        <td></td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong> <strong>supporto</strong></a>
            <br> Supporto della condivisione risorse tra origini per una maggiore flessibilità delle applicazioni.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">Supporto migliorato per l'autenticazione SAML</a><br>
 </strong>Sono stati risolti i problemi di reindirizzamento SAML migliorati, informazioni di gruppo ottimizzate e crittografia delle chiavi.
            <br>
        </td>
        <td></td>
        <td></td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP come configurazione OSGi</a><br>
 </strong>Semplifica la gestione e gli aggiornamenti dell'autenticazione LDAP.</td>
        <td></td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
    </tr>
    <tr>
        <td><strong>Supporto della crittografia OSGi per password in testo normale<br>
 </strong>Le password e altri valori sensibili possono essere salvati in forma crittografata e automaticamente decrittografati.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">Miglioramenti al CUG</a><br>
 </strong>L’implementazione del gruppo utenti chiuso è stata riscritta per risolvere problemi di prestazioni e scalabilità.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>↓</td>
        <td>↓<sup>+</sup></td>
        <td>↓</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">Creazione guidata SSL</a></strong>
            <br> Interfaccia utente per semplificare la configurazione e la gestione di SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Supporto token incapsulati</a></strong>
            <br> Non è più necessario per le sessioni "permanenti" per supportare l’autenticazione orizzontale nelle istanze di pubblicazione.</td>
        <td> </td>
        <td> </td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Supporto per l’autenticazione Adobe IMS</a><br>
 </strong>Esclusivo per Adobe Managed Services (AMS), gestisci centralmente l’accesso alle istanze di authoring AEM tramite Adobe IMS (Identity Management System).</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>↓</td>
        <td>↓</td>
    </tr>
</tbody>
</table>

## Funzioni di Sites

Di seguito è riportata una matrice delle funzioni principali di Sites offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti con miglioramenti incrementali aggiunti in ogni versione.

+ [Note sulla versione di AEM Sites](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***↓<sup>+</sup> miglioramenti significativi alla funzione in questa versione.***

***↓<sup>SP</sup> indica che la funzione è disponibile tramite un Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td><strong>Funzione Sites</strong></td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3.</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Authoring delle pagine ottimizzato per il tocco</a>:</strong>
            Consente agli editor di utilizzare tablet e computer con touch screen.</td>
            <td></td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Authoring reattivo del sito</a>:</strong>
                La modalità di layout consente agli editor di ridimensionare i componenti in base alla larghezza del dispositivo per i siti reattivi.</td>
            <td></td>
            <td></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Modelli modificabili</a>:</strong>
            Consente agli autori specializzati di creare e modificare modelli di pagina.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">Componenti core</a>:</strong>
            Accelerare lo sviluppo del sito. Disponibile su GitHub per una pianificazione e flessibilità frequenti delle versioni.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">Editor SPA</a>:</strong>
            Crea esperienze web coinvolgenti e modificabili utilizzando i framework delle applicazioni a pagina singola (SPA) basati su React.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Sistema di stili:</strong>
            Aumenta AEM componente riutilizzandolo definendone l’aspetto visivo mediante il sistema di stili nel contesto.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓<sup>SP</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Multi-Site Manager (MSM)</a>:</strong>
            Consente di gestire più siti web che condividono contenuti comuni (ad esempio, multilingue, con più marchi).</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Traduzione dei contenuti</a>:</strong>
            Il framework Plug and Play si integra con i servizi di traduzione di terze parti leader del settore.</td>
            <td></td>
            <td></td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:</strong>
            Framework di ClientContext di nuova generazione per la personalizzazione dei contenuti.</td>
            <td></td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Lanci</a>:</strong>
            Sviluppa contenuti per una versione futura senza interrompere l’authoring quotidiano.</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong>Frammenti di contenuto:</strong>
            Crea e cura contenuti editoriali scollegati dalla presentazione per un facile riutilizzo.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Frammenti esperienza</a>:</strong>
            Crea esperienze e varianti riutilizzabili ottimizzate per desktop, dispositivi mobili e canali social.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Content Services:</strong>
            Esporta i contenuti da AEM come JSON da utilizzare tra dispositivi e applicazioni.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓<sup>SP</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong>Integrazione di Adobe Analytics e approfondimenti sui contenuti:</strong>
                Facile integrazione di Adobe Analytics e DTM. Visualizza informazioni sulle prestazioni nell’ambiente Authoring.</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Integrazione Adobe Target</a>:</strong>
            Procedura guidata dettagliata per creare esperienze con targeting e creare librerie di offerte riutilizzabili.</td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Integrazione Adobe Campaign</a>:</strong>
            Semplice integrazione con la soluzione e-mail di nuova generazione.</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=it" target="_blank">Integrazione di Adobe Launch</a>:</strong>
            Effettua l’integrazione con il servizio cloud di gestione tag di nuova generazione di Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong>Schermi:</strong>
            Gestisci esperienze per digital signage e chioschi.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">eCommerce</a>:</strong>
            Offri esperienze di acquisto personalizzate e con marchio in diversi punti di contatto web, mobili e social.
            </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">Community</a>:</strong>
            Forum, commenti thread, calendari di eventi e molte altre funzioni consentono un coinvolgimento profondo dei visitatori del sito.</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Funzioni di Assets

Di seguito è riportata una matrice delle funzionalità principali di Assets offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti con miglioramenti incrementali aggiunti in ogni versione.

+ [Note sulla versione di AEM Assets](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***● indica che miglioramenti significativi alla funzione in questa versione.***

***↓<sup>+</sup> indica che la funzione è disponibile tramite un Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Funzionalità di Assets</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3.</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Interfaccia touch</a>:</strong>
            Consente di gestire le risorse su un computer desktop o su dispositivi abilitati per touch.</td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">Gestione avanzata dei metadati</a>:</strong>
            Modelli di metadati, Editor schema metadati e Modifica di metadati in blocco.</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">Attività</a> e <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank">Flusso di lavoro</a> Gestione:</strong>
            Flussi di lavoro e attività predefiniti per la revisione e l’approvazione delle risorse digitali che sfruttano i progetti AEM.</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong>Scalabilità e prestazioni:</strong>
            Supporto migliorato per l’acquisizione, il caricamento e lo storage su larga scala.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API HTTP di Assets</a>:</strong>
            Interagisce in modo programmatico con le risorse tramite HTTP e JSON.</td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Condivisione collegamenti</a>:</strong>
            Condivisione semplice e ad hoc delle risorse digitali senza necessità di accedere.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>:</strong>
            Soluzione Cloud Service SAAS per la condivisione e la distribuzione senza soluzione di continuità delle risorse digitali.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Risorse collegate</a>:</strong>
            Le istanze AEM Sites possono accedere e utilizzare facilmente le risorse di un’altra istanza AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Informazioni sulle risorse</a>:</strong>
            Sfrutta Adobe Analytics per acquisire l’interazione dei clienti sulle risorse digitali e visualizzarle in AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Risorse multilingue</a>:</strong>
            Il supporto per la traduzione automatica dei metadati delle risorse con radici della lingua.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Tag avanzati e moderazione</a>:</strong>
            Utilizza Adobe Sensei per assegnare automaticamente tag alle immagini con metadati utili.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Ricerca di traduzione intelligente</a>:</strong>
            Tradurre automaticamente i termini di ricerca durante la ricerca di AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Integrazione Adobe InDesign Server</a>:</strong>
            Genera cataloghi di prodotti. Crea opuscoli, volantini e annunci per la stampa in base ai modelli di InDesign.</td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/using.html?lang=it" target="_blank">App desktop AEM</a>:</strong>
            Sincronizza le risorse sul desktop locale per la modifica con i prodotti Creative Suite.
            </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Libreria di immagini di Adobe</a>:</strong>
                <br> Librerie Photoshop e Acrobat PDF utilizzate per la manipolazione di file di alta qualità.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/it/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe Asset Link</a>:</strong>
            Accedi ad AEM Assets direttamente dalle applicazioni Adobe Create Cloud.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Integrazione Adobe Stock</a>:</strong>
            Accedi e utilizza le immagini Adobe Stock direttamente da AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓<sup>SP</sup></td>
            <td>↓</td>
        </tr>
    </tbody>
</table>

### AEM Assets Dynamic Media

***↓<sup>+</sup> miglioramenti significativi alla funzione in questa versione.***

***↓<sup>SP</sup> indica che la funzione è disponibile tramite un Service Pack o Feature Pack.***


<table>
    <thead>
        <tr>
            <td>Funzione Dynamic Media</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3 +FP</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Imaging</a>:</strong>
            Distribuzione dinamica di immagini in formati e dimensioni diversi, tra cui Smart Crop.</td>
            <td> </td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Video</a>:</strong>
            Codifica video avanzata e streaming video adattivo</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">File multimediali interattivi</a>:</strong>
            Crea banner interattivi e video con contenuti cliccabili per mostrare le offerte chiave.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong>Imposta (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Immagine</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Centrifuga</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">File multimediali diversi</a>):</strong>
            Consente agli utenti di effettuare lo zoom, la panoramica, la rotazione e la simulazione di un'esperienza di visualizzazione a 360 gradi.</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">Visualizzatori</a>:</strong>
            Lettori e predefiniti per contenuti multimediali personalizzati con marchio personalizzato con supporto per diversi schermi/dispositivi.</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Consegna</a>:</strong>
            Opzioni flessibili per il collegamento o l’incorporazione di contenuti Dynamic Media e la distribuzione tramite protocollo HTTP/2.</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong>Aggiornamento da Scene7 a Dynamic Media:</strong>
            Possibilità di migrare le risorse master e continuare a utilizzare gli URL S7 esistenti.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
    </tbody>
</table>

## Funzioni di Forms

Di seguito è riportata una matrice delle funzioni principali del componente aggiuntivo AEM Forms offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti con miglioramenti incrementali aggiunti in ogni versione.

***↓<sup>+</sup> miglioramenti significativi alla funzione in questa versione.***

***↓<sup>SP</sup> indica che la funzione è disponibile tramite un Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Funzione Forms</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3.</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Editor Forms adattivo</a>:</strong>
            Crea moduli coinvolgenti, reattivi e adattivi in base alle impostazioni del dispositivo e del browser.</td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Documento di registrazione</a>:</strong>
            Crea un documento per garantire l’archiviazione a lungo termine di un’esperienza di acquisizione dei dati o la versione pronta per la stampa.</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">Editor tema</a>:</strong>
            Creare temi riutilizzabili per assegnare uno stile ai componenti e ai pannelli di un modulo.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Editor modelli</a>:</strong>
            Standardizzazione e implementazione di best practice per i moduli adattivi.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Integrazione Adobe Sign</a>:</strong>
            Consenti la distribuzione di scenari di firma basati su moduli integrati di Adobe Sign.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Gestione della corrispondenza</a>:</strong>
            Con AEM Forms puoi creare, gestire e distribuire corrispondenze personalizzate e interattive per i clienti.
            </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Integrazione dei dati di terze parti</a>:</strong>
            Utilizzando l’integrazione dei dati, i dati vengono recuperati da origini dati diverse in base agli input degli utenti in un modulo. All’invio del modulo, i dati acquisiti vengono riscritti nelle origini dati.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Flusso di lavoro (su OSGi) per elaborazione Forms</a>:</strong>
            Distribuzione semplificata dei processi di approvazione dei moduli.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Integrazione con il Marketing Cloud</a>:</strong>
            Integrazione con Adobe Analytics e Adobe Target per migliorare e misurare le esperienze dei clienti.</td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Gestore dei moduli</a>:</strong>
            Posizione singola per gestire tutti i moduli/documenti/corrispondenza, ad esempio per l’abilitazione di analisi, traduzione, test A/B, revisioni e pubblicazione.
            </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">App AEM Forms</a>:</strong>
            Consente l’elaborazione di moduli online/offline all’interno di un’app su iOS, Android o Windows.</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Comunicazioni interattive</a>:</strong>
            Crea comunicazioni avanzate, ad esempio istruzioni mirate, con elementi interattivi quali grafici (precedentemente noti come Documenti adattivi).</td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Flusso di lavoro (J2EE) per elaborazione Forms:</strong>
            Creazione di moduli complessi/flussi di lavoro incentrati sui documenti utilizzando un IDE intuitivo.</td>
            <td></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms Document Security</a>:</strong>
            Accesso e autorizzazione sicuri dei documenti PDF e Office.
            </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Framework di test</a>:</strong>
            Utilizza il framework Calvin e il plug-in Chrome per supportare ed eseguire il debug dei moduli adattivi.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
    </tbody>
</table>

## Funzioni di Communities

Di seguito è riportata una matrice delle funzioni principali del componente aggiuntivo AEM Communities offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti con miglioramenti incrementali aggiunti in ogni versione.

***↓<sup>+</sup> miglioramenti significativi alla funzione in questa versione.***

***↓<sup>SP</sup> indica che la funzione è disponibile tramite un Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>Funzionalità di Communities</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3.</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="7">Funzioni di Communities</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">Forum</a>:</strong> (Social Component Framework) Crea nuovi argomenti oppure visualizza, segue, cerca e sposta gli argomenti esistenti.</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">QnA</a>:</strong>
                Poni, visualizza e rispondi alle domande.</p>
            </td>
            <td></td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">Blog</a>:</strong>
                Crea articoli e commenti di blog sul lato della pubblicazione.
            </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">Idea</a>:</strong>
                Crea e condividi idee con la community, oppure visualizza, segui e commenta le idee esistenti.
            </td>
            <td> </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">Calendario</a>:</strong>
                (Social Component Framework) Fornisci informazioni sugli eventi della community ai visitatori del sito.
            </td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">Libreria file</a>:</strong>
                Carica, gestisci e scarica file all'interno del sito della community.</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">Gruppi di utenti</a>:
            </strong>Un insieme di utenti può appartenere a gruppi di membri e può essere assegnato a ruoli collettivamente.</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Assegnazione</a>:</strong>
            Crea e assegna risorse di apprendimento ai membri della community.</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td rowspan="5">Attivazione</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank">Catalogo</a> e <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Gestione risorse</a>:</strong>
            Accedi alle risorse di abilitazione dal catalogo.</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">Gestione dei percorsi di apprendimento</a>:</strong>
            Gestisci corsi o gruppi di risorse di abilitazione.</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">Generazione di rapporti di abilitazione</a>:</strong>
            Generazione di rapporti sulle risorse di abilitazione e sui percorsi di apprendimento.</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">Coinvolgimento sull'abilitazione</a>:</strong>
            Aggiungi commenti sulle risorse di abilitazione.</td>
            <td> </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analisi di abilitazione</a>:</strong>
            Analisi video, generazione di rapporti sull'avanzamento e sulle assegnazioni</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank">Commenti</a> e allegati:</strong>
            (Social Component Framework) In qualità di membro della comunità condividi opinioni e conoscenze sui contenuti sul sito Community.</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong>Conversione dei frammenti di contenuto:</strong>
            Convertire i contributi UGC in frammenti di contenuto.</td>
            <td> </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">Recensioni</a>:</strong>
                (Social Component Framework) In qualità di membro della comunità, rivedi un contenuto utilizzando una combinazione di commenti e funzioni di valutazione.</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">Valutazioni</a>:/strong&gt; (Social Component Framework) In qualità di membro della community, denota un contenuto.</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">Voti</a>:</strong>
                (Social Component Framework) In qualità di membro della comunità vota o devota un contenuto.</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">Tag</a>:</strong>
            Allega tag (parole chiave o etichette) con contenuto per individuare rapidamente il contenuto.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">Ricerca</a>:</strong>
            Ricerche predittive e suggestive.</td>
            <td> </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">Traduzione</a>:</strong>
            Traduzione automatica dei contenuti generati dall’utente.</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td rowspan="10">Amministrazione</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">Gestione del sito</a>:</strong>
            Creazione di siti con funzioni di community .</td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Modelli</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Sito</a> e <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank">gruppo</a> modelli per la creazione guidata di siti community completamente funzionanti.</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong>Modelli modificabili:</strong>
            Consentono agli amministratori della community di creare esperienze avanzate utilizzando AEM modelli modificabili.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">Gruppi o sottocomunità</a>:</strong>
            Creare in modo dinamico sottocomunità all’interno dei siti delle community.
            </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">Moderazione</a>:</strong>
            Moderazione del contenuto generato dall’utente.
            </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">Moderazione in blocco</a>:</strong>
            Console di moderazione per gestire in massa il contenuto generato dall’utente.</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">Filtri per il rilevamento dello spam e della profanità</a>:</strong>
            Rilevamento automatico dello spam.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">Gestione membri</a>:</strong>
            Gestisci profili utente e gruppi dall'area di gestione membri.</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">Design reattivo</a>:</strong>
            I siti AEM Communities sono reattivi.
            </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analytics</a>:</strong>
            Effettua l’integrazione con Adobe Analytics per ottenere informazioni chiave sull’utilizzo dei siti di Communities.</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td rowspan="4">Membri</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">Punteggio e badging</a>:</strong>
            (Punteggio avanzato fornito da Adobe Sensei) Identifica i membri della community come esperti e li ricompensa.</td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank">Attività</a> e <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">Notifiche</a>:</strong>
            Visualizza il flusso di attività recenti e riceve notifiche su eventi di interesse.</td>
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
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">Accesso a Social</a>:</strong>
            Accedi con il loro account Facebook o Twitter.</td>
            <td> </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td rowspan="5">Platform</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP (Mongo Storage)</a>:</strong>
            Il contenuto generato dall’utente (UGC) viene mantenuto direttamente in un’istanza MongoDB locale</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP (Database Storage)</a>:</strong>
            Il contenuto generato dall'utente (UGC) viene mantenuto direttamente in un'istanza di database locale MySQL.</td>
            <td> </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP (archiviazione cloud)</a>:</strong>
                Il contenuto generato dall’utente (UGC) viene mantenuto in remoto in un servizio cloud ospitato e gestito da Adobe.</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                Il contenuto della community è memorizzato in JCR e UGC è accessibile dall’istanza di authoring (o pubblicazione) a cui è stato pubblicato.</td>
            <td> </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">Sincronizzazione utenti e gruppi</a>:</strong>
            Sincronizza utenti e gruppi tra le istanze di pubblicazione quando utilizzi una topologia di Publish farm.</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
    </tbody>
</table>

AEM Communities aggiunge miglioramenti tramite le versioni per consentire alle organizzazioni di coinvolgere e abilitare i propri utenti, tramite:

+ **@mention** nel contenuto generato dall’utente.
+ Miglioramenti all’accessibilità tramite **Navigazione tramite tastiera** in **Abilitazione** componenti.
+ Migliorati **Moderazione in blocco** utilizzo **Filtri personalizzati**.
+ **Modelli modificabili** per consentire agli amministratori della community di creare esperienze di community avanzate in AEM.
+ Gli utenti possono ora inviare **messaggi diretti in blocco** a tutti i membri di un gruppo.
