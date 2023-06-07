---
title: Perché effettuare l'aggiornamento
description: Analisi delle funzioni chiave per i clienti che intendono effettuare l’aggiornamento all’ultima versione di Adobe Experience Manager 6.
version: 6.5
topic: Upgrade
role: Leader, Architect, Developer, Admin, User
level: Beginner
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
source-git-commit: 678ecb99b1e63b9db6c9668adee774f33b2eefab
workflow-type: tm+mt
source-wordcount: '2602'
ht-degree: 4%

---

# Perché effettuare l&#39;aggiornamento

Analisi delle funzioni chiave per i clienti che intendono effettuare l’aggiornamento all’ultima versione di Adobe Experience Manager 6.

## Funzioni chiave per l’aggiornamento a AEM 6.5

+ [Note sulla versione di Adobe Experience Manager 6.5](https://helpx.adobe.com/it/experience-manager/6-5/release-notes.html)

### Miglioramenti alla base

Adobe Experience Manager 6.5 continua a migliorare la stabilità, le prestazioni e la supportabilità del sistema tramite:

+ **Java 11** (mantenendo il supporto Java 8).

### Creazione e gestione di siti Web

AEM Sites presenta una serie di funzioni progettate per accelerare la creazione e l’espansione di siti web:

+ **Editor SPA** Il supporto di consente di creare completamente l’SPA (applicazioni a pagina singola) nell’AEM, offrendo un’esperienza di authoring ricca e intuitiva.
+_ **SDK JavaScript**, un kit di avvio per progetti SPA e strumenti di supporto per la creazione di, consentono agli sviluppatori front-end di sviluppare applicazioni a pagina singola compatibili con l’Editor SPA indipendentemente dall’AEM.
+ **Componenti core** aggiunge una moltitudine di nuovi componenti, **Libreria dei componenti** nonché una serie di miglioramenti ai Componenti core esistenti.
+ Ulteriori informazioni **Traduzioni** i miglioramenti semplificano la traduzione di AEM Sites.

### Esperienze fluide

L’AEM continua a utilizzare le esperienze fluide con strumenti nuovi e migliorati che facilitano l’utilizzo di contenuti al di fuori dell’AEM.

+ **Frammenti di contenuto** supporta il confronto delle versioni, le differenze di versione e le annotazioni.
+ **API HTTP di risorse AEM** supporta l&#39;esposizione **Frammenti di contenuto** direttamente in DAM come **JSON**.
   **Frammenti esperienza** supporto **Ricerca full-text** e **Annullamento della validità della cache del dispatcher AEM** per riferimento **Pagine**.

### Gestione risorse

AEM Assets continua a sfruttare la ricca serie di funzionalità per la gestione delle risorse per migliorare l’utilizzo, la gestione e la comprensione di DAM. AEM 6.5 continua a migliorare l’integrazione tra Adobe Creative Cloud e i flussi di lavoro creativi.

+ **Adobe collegamento risorsa** collega i contenuti creativi direttamente ad AEM Assets da Adobe Creative Cloud tools.
+ **Adobe Stock** L’integrazione di consente di accedere direttamente alle immagini di Adobe Stock dall’esperienza AEM Assets, creando un’esperienza di individuazione dei contenuti fluida.
+ **App desktop AEM** viene rilasciata la versione 2.0 di e viene rivisto, migliorando al contempo le prestazioni e la stabilità.
+ **Risorse collegate** supporta istanze AEM Sites discrete per accedere e utilizzare senza problemi le risorse di un’istanza AEM Assets diversa.
+ Supporto video aggiornato in **Dynamic Media**, tra cui **Video a 360°** e **Miniature video personalizzate**.

### Informazioni sui contenuti

L’AEM continua a costruire la sua integrazione con le tecnologie intelligenti, sfruttando l’apprendimento automatico e l’intelligenza artificiale per migliorare tutte le esperienze.

+ **Adobe collegamento risorsa** aggiunge **Ricerca somiglianza visiva**, consentendo la facile individuazione e utilizzo di immagini simili all&#39;interno di **Strumenti Adobe Creative Cloud**.

### Integrazioni

L’AEM aumenta la sua capacità di integrazione con altri servizi Adobe:

+ **Frammenti esperienza** approfondisce la loro integrazione con **Adobe Target** sostenendo **Esporta come JSON** ad Adobe Target e la possibilità di **eliminare le offerte basate su frammenti di esperienza** da **Adobe Target**.

### AMS Cloud Manager

[Cloud Manager](https://adobe.ly/2HODmsv), esclusivo per i clienti di Adobe Managed Services (AMS), offre le seguenti funzionalità:

+ Cloud Manager supporta l’estensione del supporto per la distribuzione dell’AEM da AEM Sites a **AEM Assets**, tra cui **test delle prestazioni automatizzati per l’elaborazione delle risorse**.
+ **Ridimensionamento automatico** del livello di pubblicazione di AEM a soglie predefinite, garantisce un’esperienza utente finale ottimale.
+ **Pipeline non di produzione** consente ai team di sviluppo di sfruttare Cloud Manager per verificare continuamente la qualità del codice e distribuirla in ambienti più bassi (Sviluppo e Controllo qualità).
+ **API della pipeline CI/CD** consente ai clienti di interagire in modo programmatico con Cloud Manager, approfondendo le possibilità di integrazione con l’infrastruttura di sviluppo on-premise.

## Funzioni di base

Di seguito è riportata una matrice delle principali funzionalità di base offerte dall’AEM. Alcune di queste funzioni sono state introdotte nelle versioni precedenti con miglioramenti incrementali aggiunti in ciascuna versione.

+ [Note sulla versione di AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> miglioramenti significativi alla funzione in questa versione.***

***✔<sup>SP</sup> indica che la funzione è disponibile tramite un Service Pack o un Feature Pack.***

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
                <strong>Supporto Java 11:</strong> L'AEM supporta Java 11 (oltre a Java 8).
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
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Archivio dei contenuti Oak</a>:</strong> Fornisce prestazioni e scalabilità di gran lunga superiori rispetto al predecessore Jackrabbit 2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">Supporto per l’indice oak-run.jar</a>:</strong> Sono state migliorate la reindicizzazione, la raccolta di statistiche e il controllo di coerenza degli indici Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Indici di ricerca personalizzati</a>: </strong>
                Possibilità di aggiungere definizioni di indice personalizzate per ottimizzare le prestazioni delle query e la rilevanza delle ricerche.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/it/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Pulizia revisioni online</a>:</strong>
                Manutenzione dell'archivio senza tempi di inattività del server.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">Archiviazione archivio TarMK o MongoMK</a>:</strong>
                <br> Opzioni per l'utilizzo di uno storage basato su file semplice e performante di TarMK (versione di nuova generazione di TarPM)
                <br> o scalare orizzontalmente con un archivio supportato da MongoDB con MongoMK.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">Prestazioni e stabilità MongoMK</a>:</strong>
            Sono stati apportati continui miglioramenti a MongoMK dalla sua introduzione con AEM 6.0.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Archivio dati Amazon S3</a>:</strong>
            Sfrutta la soluzione di archiviazione cloud espandibile per archiviare risorse binarie.</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Parità delle funzioni dell’interfaccia touch:</strong>
                Miglioramenti continui all’interfaccia utente di authoring per una maggiore velocità, una maggiore produttività e parità di funzioni con l’interfaccia classica.</td>
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
                Cerca e naviga rapidamente in AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Dashboard operazioni</a>:</strong>
 Eseguire interventi di manutenzione, monitorare lo stato del server e analizzare le prestazioni dall'interno di AEM.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Miglioramenti degli aggiornamenti</a>:</strong>
            I miglioramenti degli aggiornamenti consentono aggiornamenti in sede più semplici e rapidi dell’AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/it/experience-manager/htl/using/overview.html" target="_blank">Lingua del modello HTL</a>:</strong>
            Motore di modelli moderno che separa la presentazione dalla logica. Riduce notevolmente i tempi di sviluppo dei componenti. Sono state aggiunte funzioni incrementali a ogni versione.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Modelli Sling</a>:</strong>
            Un framework flessibile per modellare le risorse JCR in oggetti e logiche di business. Sono state aggiunte funzioni incrementali a ogni versione.
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
                Esclusivo per i clienti Adobe Managed Services (AMS), Cloud Manager accelera lo sviluppo e la distribuzione tramite una pipeline CI/CD allo stato dell’arte.</td>
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

## Funzioni di sicurezza

Di seguito è riportata una matrice delle principali funzioni di sicurezza offerte dall&#39;AEM. Alcune di queste funzioni sono state introdotte nelle versioni precedenti con miglioramenti incrementali aggiunti in ciascuna versione.

+ [Note sulla versione di sicurezza](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔ indica che sono stati apportati miglioramenti significativi alla funzione in questa versione.***

***✔<sup>+</sup> indica che la funzione è disponibile tramite un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Funzione di sicurezza</td>
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Utenti del servizio</a></strong>
            <br> Le autorizzazioni suddivise in compartimenti evitano l’uso superfluo dei privilegi di amministratore.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Gestione archivio chiavi</a></strong>
            <br> Archivio attendibilità globale, certificati e chiavi gestiti all'interno dell'archivio.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRF</strong> <strong>protezione</strong></a>
            <br> Protezione preconfigurata contro la falsificazione di richieste cross-site.</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong> <strong>supporto</strong></a>
            <br> Supporto di Cross-Origin Resource Sharing per una maggiore flessibilità delle applicazioni.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">Migliore supporto per l’autenticazione SAML</a><br>
 </strong>Sono stati risolti i problemi di reindirizzamento SAML migliorato, informazioni di gruppo ottimizzate e crittografia delle chiavi.
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">Configurazione LDAP as a OSGi</a><br>
 </strong>Gestione e aggiornamenti semplificati dell'autenticazione LDAP.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>Supporto della crittografia OSGi per le password in formato testo normale<br>
 </strong>Le password e gli altri valori sensibili possono essere salvati in forma crittografata e decrittografati automaticamente.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">Miglioramenti CUG</a><br>
 </strong>L’implementazione di Gruppo utenti chiuso è stata riscritta per risolvere i problemi di prestazioni e scalabilità.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL Wizard</a></strong>
            <br> Interfaccia utente per semplificare la configurazione e la gestione di SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Supporto di token incapsulati</a></strong>
            <br> Non è più necessario che le sessioni "permanenti" supportino l’autenticazione orizzontale tra le istanze di pubblicazione.</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Supporto per l’autenticazione Adobe IMS</a><br>
 </strong>Esclusivamente per Adobe Managed Services (AMS), gestisce centralmente l’accesso alle istanze di AEM Author tramite Adobe IMS (Identity Management System).</td>
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

## Funzioni di Sites

Di seguito è riportata una matrice delle funzioni principali di Sites offerte dall&#39;AEM. Alcune di queste funzioni sono state introdotte nelle versioni precedenti con miglioramenti incrementali aggiunti in ciascuna versione.

+ [Note sulla versione di AEM Sites](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup> miglioramenti significativi alla funzione in questa versione.***

***✔<sup>SP</sup> indica che la funzione è disponibile tramite un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td><strong>Funzione Sites</strong></td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Authoring delle pagine ottimizzate per il tocco</a>:</strong>
            Consente agli editor di utilizzare tablet e computer con schermi touch screen.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Authoring reattivo del sito</a>:</strong>
                La modalità layout consente agli editor di ridimensionare i componenti in base alle larghezze dei dispositivi per i siti reattivi.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Modelli modificabili</a>:</strong>
            Consente agli autori specializzati di creare e modificare modelli di pagina.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">Componenti core</a>:</strong>
            Accelerare lo sviluppo del sito. Disponibile su GitHub per pianificazione delle versioni frequenti e flessibilità.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">Editor SPA</a>:</strong>
            Creare esperienze web personalizzabili utilizzando framework per applicazioni a pagina singola (SPA) basati su React.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Sistema di stili:</strong>
            Aumenta il riutilizzo dei componenti AEM definendone l’aspetto visivo utilizzando il sistema di stili nel contesto.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Gestione multisito (MSM)</a>:</strong>
            Gestisci più siti web che condividono contenuti comuni (ad esempio, multilingue, marchi multipli).</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Traduzione dei contenuti</a>:</strong>
            Il framework Plug and Play si integra con i principali servizi di traduzione di terze parti del settore.</td>
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
            Framework di contesto client di nuova generazione per la personalizzazione dei contenuti.</td>
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
            Sviluppare contenuti per una versione futura senza interrompere l’authoring quotidiano.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Frammenti di contenuto:</strong>
            Creare e curare contenuti editoriali separati dalla presentazione per un facile riutilizzo.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Frammenti esperienza</a>:</strong>
            Crea esperienze e varianti riutilizzabili ottimizzate per canali desktop, mobili e social.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Content Services:</strong>
            Esporta contenuti dall’AEM come JSON per il consumo tra dispositivi e applicazioni.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Integrazione di Adobe Analytics e informazioni sui contenuti:</strong>
                Facile integrazione di Adobe Analytics e DTM. Visualizza le informazioni sulle prestazioni nell’ambiente di authoring.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Integrazione di Adobe Target</a>:</strong>
            Procedura guidata dettagliata per creare esperienze mirate e librerie di offerte riutilizzabili.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Integrazione di Adobe Campaign</a>:</strong>
            Semplice integrazione con la soluzione della campagna e-mail di nuova generazione.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=it" target="_blank">Integrazione di Adobe Launch</a>:</strong>
            Integrazione con il servizio cloud di gestione tag di nuova generazione di Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Schermi:</strong>
            Gestisci le esperienze per i chioschi e le soluzioni di digital signage.</td>
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
            Fornisci esperienze di acquisto personalizzate e con marchio su diversi punti di contatto web, mobili e social.
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
            Forum, commenti filettati, calendari degli eventi e molte altre funzioni consentono un coinvolgimento profondo con i visitatori del sito.</td>
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

## Funzioni di Assets

Di seguito è riportata una matrice delle funzioni principali di Assets offerte dall’AEM. Alcune di queste funzioni sono state introdotte nelle versioni precedenti con miglioramenti incrementali aggiunti in ciascuna versione.

+ [Note sulla versione di AEM Assets](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***✔ indica che sono stati apportati miglioramenti significativi alla funzione in questa versione.***

***✔<sup>+</sup> indica che la funzione è disponibile tramite un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Funzionalità risorse</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Interfaccia touch</a>:</strong>
            Gestisci le risorse su un computer desktop o su dispositivi touch.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">Gestione avanzata dei metadati</a>:</strong>
            Modelli di metadati, Editor schema metadati e modifica in blocco dei metadati.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">Attività</a> e <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank">Flusso di lavoro</a> Gestione:</strong>
            Flussi di lavoro e compiti predefiniti per la revisione e l’approvazione delle risorse digitali che sfruttano i progetti AEM.</td>
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
            Supporto migliorato per l’acquisizione, il caricamento e l’archiviazione su larga scala.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API HTTP di Assets</a>:</strong>
            Interagire a livello di programmazione con le risorse tramite HTTP e JSON.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Condivisione collegamenti</a>:</strong>
            Semplice condivisione ad hoc di risorse digitali senza dover effettuare l’accesso.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>:</strong>
            Soluzione SAAS di Cloud Service per la condivisione e la distribuzione senza soluzione di continuità delle risorse digitali.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Risorse collegate</a>:</strong>
            Le istanze di AEM Sites possono accedere e utilizzare senza problemi le risorse di un’istanza di AEM Assets diversa.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Informazioni sulla risorsa</a>:</strong>
            Sfrutta Adobe Analytics per acquisire l’interazione dei clienti con le risorse digitali e visualizzarle nell’AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Risorse multilingue</a>:</strong>
            Supporto della traduzione automatica dei metadati delle risorse con directory principali della lingua.</td>
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
            Sfrutta Adobe Sensei per assegnare automaticamente tag alle immagini con metadati utili.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Ricerca traduzione avanzata</a>:</strong>
            Traduci automaticamente i termini di ricerca quando cerchi AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Integrazione di Adobe InDesign Server</a>:</strong>
            Genera cataloghi di prodotti. Creazione di brochure, volantini e annunci di stampa in base ai modelli InDesign.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/using.html?lang=it" target="_blank">App desktop AEM</a>:</strong>
            Sincronizza le risorse con il desktop locale per la modifica con i prodotti Creative Suite.
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Libreria immagini Adobe</a>:</strong>
                <br> Librerie PDF di Photoshop e Acrobat utilizzate per la manipolazione di file di alta qualità.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/it/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe collegamento risorsa</a>:</strong>
            Accedi ad AEM Assets direttamente dalle applicazioni Adobe Create Cloud.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Integrazione di Adobe Stock</a>:</strong>
            Accedi e utilizza senza problemi le immagini Adobe Stock direttamente dall’AEM.</td>
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

### AEM Assets Dynamic Media

***✔<sup>+</sup> miglioramenti significativi alla funzione in questa versione.***

***✔<sup>SP</sup> indica che la funzione è disponibile tramite un Service Pack o un Feature Pack.***


<table>
    <thead>
        <tr>
            <td>Funzione Dynamic Media</td>
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
            Distribuzione dinamica di immagini in formati e dimensioni diversi, incluso Smart Crop.</td>
            <td> </td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">File multimediali interattivi</a>:</strong>
            Crea banner interattivi e video con contenuti cliccabili per mostrare le offerte chiave.
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
            <td><strong>Set (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Immagine</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Rotazione</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">File multimediali diversi</a>):</strong>
            Consente agli utenti di eseguire zoom, scorrimento, rotazione e simulazione di un'esperienza di visualizzazione a 360 gradi.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">Visualizzatori</a>:</strong>
            Lettori multimediali avanzati e predefiniti personalizzati con il supporto per schermi/dispositivi diversi.</td>
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
            Opzioni flessibili per il collegamento o l'incorporamento di contenuti Dynamic Media e la distribuzione tramite il protocollo HTTP/2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Aggiornamento da Scene7 a Dynamic Media:</strong>
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

## Funzioni di Forms

Di seguito è riportata una matrice delle principali funzioni del componente aggiuntivo AEM Forms offerte dall’AEM. Alcune di queste funzioni sono state introdotte nelle versioni precedenti con miglioramenti incrementali aggiunti in ciascuna versione.

***✔<sup>+</sup> miglioramenti significativi alla funzione in questa versione.***

***✔<sup>SP</sup> indica che la funzione è disponibile tramite un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Funzione Forms</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Editor Forms adattivo</a>:</strong>
            Crea moduli coinvolgenti, reattivi e adattivi basati sulle impostazioni del dispositivo e del browser.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Documento record</a>:</strong>
            Crea un documento per garantire l’archiviazione a lungo termine di un’esperienza di acquisizione dati o di una versione pronta per la stampa.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">Editor temi</a>:</strong>
            Creare temi riutilizzabili per assegnare stili ai componenti e ai pannelli di un modulo.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Editor modelli</a>:</strong>
            Standardizzare e implementare le best practice per i moduli adattivi.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Integrazione di Acrobat Sign</a>:</strong>
            Consenti la distribuzione di scenari di firma basati su moduli integrati di Acrobat Sign.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Gestione della corrispondenza</a>:</strong>
            Con AEM Forms puoi creare, gestire e distribuire corrispondenza personalizzata e interattiva con i clienti.
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Integrazione dei dati di terze parti</a>:</strong>
            Utilizzando l’integrazione dei dati, i dati vengono recuperati da diverse origini dati in base agli input dell’utente in un modulo. All’invio del modulo, i dati acquisiti vengono riscritti nelle origini dati.
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Flusso di lavoro (su OSGi) per elaborazione Forms</a>:</strong>
            Installazione semplificata dei processi di approvazione dei moduli.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Integrazione con Marketing Cloud</a>:</strong>
            Integrazione con Adobe Analytics e Adobe Target per migliorare e misurare le esperienze dei clienti.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Gestore moduli</a>:</strong>
            Posizione singola per gestire tutti i moduli/documenti/corrispondenza, ad esempio abilitare analisi, traduzione, test A/B, revisioni e pubblicazioni.
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">App AEM Forms</a>:</strong>
            Consente l’elaborazione di moduli online/offline all’interno di un’app su iOS, Android o Windows.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Comunicazioni interattive</a>:</strong>
            Creare comunicazioni avanzate, ad esempio istruzioni mirate con elementi interattivi quali grafici (precedentemente noti come documenti adattivi).</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Flusso di lavoro (J2EE) per l'elaborazione Forms:</strong>
            Crea flussi di lavoro complessi incentrati su moduli e documenti utilizzando un IDE intuitivo.</td>
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
            Accesso sicuro e autorizzazione dei documenti PDF e Office.
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Framework di prova</a>:</strong>
            Utilizza il framework Calvin e il plug-in Chrome per supportare ed eseguire il debug dei moduli adattivi.</td>
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

