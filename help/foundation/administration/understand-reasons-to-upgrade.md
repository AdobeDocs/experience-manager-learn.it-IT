---
title: Perché effettuare l'aggiornamento
description: Analisi dettagliata delle funzioni chiave per i clienti che intendono effettuare l'aggiornamento all'ultima versione di Adobe Experience Manager 6.
version: Experience Manager 6.5
topic: Upgrade
feature: Release Information
role: Leader, Architect, Developer, Admin, User
level: Beginner
doc-type: Article
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
duration: 538
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '2576'
ht-degree: 1%

---

# Perché effettuare l&#39;aggiornamento

Analisi dettagliata delle funzioni chiave per i clienti che intendono effettuare l&#39;aggiornamento all&#39;ultima versione di Adobe Experience Manager 6.

## Funzioni chiave per l’aggiornamento ad AEM 6.5

+ [Note sulla versione di Adobe Experience Manager 6.5](https://helpx.adobe.com/it/experience-manager/6-5/release-notes.html)

### Miglioramenti alla base

Adobe Experience Manager 6.5 continua a migliorare la stabilità, le prestazioni e la supportabilità del sistema tramite:

+ Supporto di **Java 11** (mantenendo il supporto di Java 8).

### Creazione e gestione di siti Web

AEM Sites presenta una serie di funzioni progettate per accelerare la creazione e l’espansione di siti web:

+ Il supporto dell&#39;**Editor di applicazioni a pagina singola** consente di creare completamente applicazioni a pagina singola in AEM, supportando un&#39;esperienza di authoring avanzata e intuitiva per gli addetti al marketing.
+_ **JavaScript SDK**, un kit di avvio per progetti SPA e strumenti di supporto per la creazione, consente agli sviluppatori front-end di sviluppare applicazioni a pagina singola compatibili con SPA Editor indipendentemente da AEM.
+ **Componenti core** aggiunge una moltitudine di nuovi componenti, una **Libreria componenti** e una serie di miglioramenti ai Componenti core esistenti.
+ Ulteriori miglioramenti alle **traduzioni** consentono di semplificare la traduzione di AEM Sites.

### Esperienze fluide

AEM continua ad abbracciare le esperienze fluide con strumenti nuovi e migliorati che facilitano l’utilizzo di contenuti al di fuori di AEM.

+ **I frammenti di contenuto** supportano il confronto delle versioni/le differenze e le annotazioni.
+ **L&#39;API HTTP Assets di AEM** supporta l&#39;esposizione di **Frammenti di contenuto** direttamente in DAM come **JSON**.
  **I frammenti di esperienza** supportano **la ricerca full-text** e **l&#39;annullamento della validità della cache di AEM Dispatcher** per il riferimento a **pagine**.

### Gestione risorse

AEM Assets continua a sfruttare la ricca serie di funzionalità per la gestione delle risorse per migliorare l’utilizzo, la gestione e la comprensione di DAM. AEM 6.5 continua a migliorare l’integrazione tra Adobe Creative Cloud e i flussi di lavoro creativi.

+ **Adobe Asset Link** collega i contenuti creativi direttamente ad AEM Assets dagli strumenti di Adobe Creative Cloud.
+ L&#39;integrazione di **Adobe Stock** consente l&#39;accesso diretto alle immagini Adobe Stock direttamente dall&#39;esperienza AEM Assets, creando un&#39;esperienza di individuazione dei contenuti senza soluzione di continuità.
+ **L&#39;app desktop AEM** rilascia la versione 2.0 e si rivede migliorando al contempo le prestazioni e la stabilità.
+ **Assets** connesso supporta istanze AEM Sites discrete per accedere e utilizzare in modo semplice le risorse di un&#39;istanza AEM Assets diversa.
+ Supporto video aggiornato in **Dynamic Media**, inclusi **360 Video** e **Anteprime video personalizzate**.

### Informazioni sui contenuti

AEM continua a sviluppare la sua integrazione con le tecnologie intelligenti, sfruttando l’apprendimento automatico e l’intelligenza artificiale per migliorare tutte le esperienze.

+ **Adobe Asset Link** aggiunge **Visual Similarity Search**, consentendo di individuare e utilizzare immagini simili in modo semplice in **strumenti di Adobe Creative Cloud**.

### Integrazioni

AEM aumenta la capacità di integrarsi con altri servizi Adobe:

+ **Frammenti di esperienza** approfondisce l&#39;integrazione con **Adobe Target** supportando **Esporta come JSON** in Adobe Target e la possibilità di **eliminare le offerte basate su frammenti di esperienza** da **Adobe Target**.

### AMS CLOUD MANAGER

[Cloud Manager](https://adobe.ly/2HODmsv), esclusivo per i clienti Adobe Managed Services (AMS), offre le seguenti funzionalità:

+ Cloud Manager supporta l&#39;estensione del supporto per la distribuzione di AEM da AEM Sites a **AEM Assets**, incluso **test delle prestazioni automatizzati per l&#39;elaborazione delle risorse**.
+ **Ridimensionamento automatico** del livello di pubblicazione AEM a soglie predefinite, per garantire un&#39;esperienza utente ottimale.
+ **Pipeline non di produzione** consentono ai team di sviluppo di sfruttare Cloud Manager per verificare continuamente la qualità del codice e distribuirle in ambienti di livello inferiore (sviluppo e controllo qualità).
+ Le **API della pipeline CI/CD** consentono ai clienti di interagire in modo programmatico con Cloud Manager, approfondendo le possibilità di integrazione con l&#39;infrastruttura di sviluppo on-premise.

## Funzioni di base

Di seguito è riportata una matrice delle funzioni principali di base offerte da AEM. Alcune di queste funzioni sono state introdotte nelle versioni precedenti con miglioramenti incrementali aggiunti in ciascuna versione.

+ [Note sulla versione di AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> miglioramenti significativi alla funzionalità in questa versione.***

***✔<sup>SP</sup> indica che la funzionalità è disponibile tramite un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Funzione Foundation</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <strong>Supporto Java 11:</strong> AEM supporta Java 11 (oltre a Java 8).
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
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Archivio dei contenuti Oak</a>:</strong> Fornisce prestazioni e scalabilità notevolmente superiori rispetto al predecessore Jackrabbit 2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">Supporto indice oak-run.jar</a>:</strong> Miglioramento della reindicizzazione, della raccolta delle statistiche e della verifica di coerenza degli indici Oak.</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Pulizia revisioni in linea</a>:</strong>
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
                <br> Opzioni per l'utilizzo di un archivio basato su file semplice e performante di TarMK (versione di nuova generazione di TarPM)
                <br> o scalabilità orizzontale con un archivio supportato da MongoDB con MongoMK.</td>
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
            <td><strong>Parità funzionalità interfaccia utente touch:</strong>
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
 Esegui la manutenzione, monitora lo stato del server e analizza le prestazioni dall’interno di AEM.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Miglioramenti all'aggiornamento</a>:</strong>
            I miglioramenti degli aggiornamenti consentono aggiornamenti AEM più semplici e rapidi.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/it/experience-manager/htl/using/overview.html" target="_blank">Linguaggio del modello HTL</a>:</strong>
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
                Esclusivamente per i clienti Adobe Managed Services (AMS), Cloud Manager accelera lo sviluppo e la distribuzione tramite una pipeline CI/CD allo stato dell’arte.</td>
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

Di seguito è riportata una matrice delle funzioni di sicurezza principali offerte da AEM. Alcune di queste funzioni sono state introdotte nelle versioni precedenti con miglioramenti incrementali aggiunti in ciascuna versione.

+ [Note sulla versione di sicurezza](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔indica che sono stati apportati miglioramenti significativi alla funzionalità in questa versione.***

***✔<sup>+</sup> indica che la funzionalità è disponibile tramite un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Funzione di sicurezza</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Utenti del servizio</a></strong>
            <br> Le autorizzazioni per la suddivisione in compartimenti evitano l'uso non necessario dei privilegi di amministratore.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Gestione archivio chiavi</a></strong>
            <br> Archivio attendibilità globale, certificati e chiavi tutti gestiti all'interno dell'archivio.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRF</strong> <strong>protezione</strong></a>
            <br> protezione preconfigurata contro la falsificazione di richieste tra siti diversi.</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong> <strong>supporto</strong></a>
            <br> Supporto per la condivisione delle risorse tra le origini per una maggiore flessibilità dell'applicazione.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">Supporto dell'autenticazione SAML migliorato</a><br>
 </strong>Sono stati risolti problemi di reindirizzamento SAML migliorato, informazioni sul gruppo ottimizzato e crittografia delle chiavi.
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP come configurazione OSGi</a><br>
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
 </strong>L'implementazione del gruppo utenti chiuso è stata riscritta per risolvere i problemi di prestazioni e scalabilità.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">Procedura guidata SSL</a></strong>
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Supporto token incapsulato</a></strong>
            <br> Non è più necessario che le sessioni "permanenti" supportino l'autenticazione orizzontale tra le istanze di pubblicazione.</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Supporto per l'autenticazione Adobe IMS</a><br>
 </strong>Esclusivo per Adobe Managed Services (AMS), gestisce centralmente l’accesso alle istanze di AEM Author tramite Adobe IMS (Identity Management System).</td>
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

Di seguito è riportata una matrice delle funzioni chiave di Sites offerte da AEM. Alcune di queste funzioni sono state introdotte nelle versioni precedenti con miglioramenti incrementali aggiunti in ciascuna versione.

+ [Note sulla versione di AEM Sites](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup> miglioramenti significativi alla funzionalità in questa versione.***

***✔<sup>SP</sup> indica che la funzionalità è disponibile tramite un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td><strong>Funzione di Sites</strong></td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Authoring pagine ottimizzate per il tocco</a>:</strong>
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
            Crea esperienze web coinvolgenti e personalizzabili utilizzando framework per applicazioni a pagina singola (SPA) basati su React.</td>
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
            Aumenta il riutilizzo dei componenti di AEM definendone l’aspetto visivo mediante il sistema di stili nel contesto.</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Traduzione contenuto</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Avvii</a>:</strong>
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
            <td><strong>Servizi contenuto:</strong>
            Esporta contenuti da AEM come JSON per l’utilizzo su dispositivi e applicazioni.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Integrazione di Adobe Analytics e informazioni sul contenuto:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Integrazione Adobe Target</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Integrazione Adobe Campaign</a>:</strong>
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
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=it" target="_blank">Tag nell'integrazione di Adobe Experience Platform</a>:</strong>
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
            <td><strong>Screens:</strong>
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

Di seguito è riportata una matrice delle funzioni chiave di Assets offerte da AEM. Alcune di queste funzioni sono state introdotte nelle versioni precedenti con miglioramenti incrementali aggiunti in ciascuna versione.

+ [Note sulla versione di AEM Assets](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***✔indica che sono stati apportati miglioramenti significativi alla funzionalità in questa versione.***

***✔<sup>+</sup> indica che la funzionalità è disponibile tramite un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Funzione Assets</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Interfaccia utente ottimizzata per il tocco</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">Attività</a> e Gestione flussi di lavoro:</strong>
            Flussi di lavoro e attività predefiniti per la revisione e l’approvazione delle risorse digitali che sfruttano AEM Projects.</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API HTTP Assets</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Assets connesso</a>:</strong>
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
            Sfrutta Adobe Analytics per acquisire l’interazione dei clienti con le risorse digitali e visualizzarle in AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Assets multilingue</a>:</strong>
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
            Sfrutta l’intelligenza artificiale di Adobe per assegnare automaticamente alle immagini tag con metadati utili.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Ricerca Smart Translation</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Integrazione Adobe InDesign Server</a>:</strong>
            Genera cataloghi di prodotti. Creazione di brochure, volantini e annunci di stampa in base ai modelli di InDesign.</td>
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
            Sincronizza le risorse sul desktop locale per l’editing con i prodotti Creative Suite.
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Libreria di imaging Adobe</a>:</strong>
                <br> librerie Photoshop e Acrobat PDF utilizzate per manipolare file di alta qualità.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/it/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe Asset Link</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Integrazione Adobe Stock</a>:</strong>
            Accedi e utilizza facilmente le immagini Adobe Stock direttamente da AEM.</td>
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

### Dynamic Media di AEM Assets

***✔<sup>+</sup> miglioramenti significativi alla funzionalità in questa versione.***

***✔<sup>SP</sup> indica che la funzionalità è disponibile tramite un Service Pack o un Feature Pack.***


<table>
    <thead>
        <tr>
            <td>Funzione elemento multimediale dinamico</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6.2</td>
            <td>6.3 +FP</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Immagini</a>:</strong>
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
            <td><strong>Set (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Immagine</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Rotazione</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">File Multimediali Misti</a>):</strong>
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
            Opzioni flessibili per il collegamento o l’incorporamento di contenuti Dynamic Media e la distribuzione tramite il protocollo HTTP/2.</td>
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

Di seguito è riportata una matrice delle principali funzioni del componente aggiuntivo AEM Forms offerte da AEM. Alcune di queste funzioni sono state introdotte nelle versioni precedenti con miglioramenti incrementali aggiunti in ciascuna versione.

***✔<sup>+</sup> miglioramenti significativi alla funzionalità in questa versione.***

***✔<sup>SP</sup> indica che la funzionalità è disponibile tramite un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Funzione Forms</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Integrazione Acrobat Sign</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Gestione moduli</a>:</strong>
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
            <td><strong>Flusso di lavoro (J2EE) per elaborazione Forms:</strong>
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
            Accesso sicuro e autorizzazione dei documenti di PDF e Office.
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Framework di test</a>:</strong>
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

