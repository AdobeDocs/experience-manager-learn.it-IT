---
title: Motivi dell’aggiornamento
description: Analisi dettagliata delle funzioni chiave per i clienti che considerano l’aggiornamento all’ultima versione di Adobe Experience Manager 6.
version: 6.5
topic: Upgrade
role: Leader, Architect, Developer, Admin, User
level: Beginner
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
source-git-commit: 278433e7d9a2d524198efcebae336dca01a15259
workflow-type: tm+mt
source-wordcount: '3462'
ht-degree: 3%

---

# Perché effettuare l’aggiornamento

Analisi dettagliata delle funzioni chiave per i clienti che considerano l’aggiornamento all’ultima versione di Adobe Experience Manager 6.

## Funzioni principali per l&#39;aggiornamento a AEM 6.5

+ [Note sulla versione di Adobe Experience Manager 6.5](https://helpx.adobe.com/it/experience-manager/6-5/release-notes.html)

### Miglioramenti di Foundation

Adobe Experience Manager 6.5 continua a migliorare la stabilità, le prestazioni e la supportabilità del sistema tramite:

+ **Supporto Java 11**  (pur mantenendo il supporto Java 8).

### Creazione e gestione di siti web

AEM Sites introduce una serie di funzioni progettate per accelerare la creazione e la creazione di siti web:

+ **SPA** Editorsupport consente di creare SPA (applicazioni a pagina singola) completamente in AEM, supportando un’esperienza di authoring ricca e intuitiva.
+_ **SDK JavaScript**, un kit di avvio del progetto SPA e strumenti di creazione di supporto, consentono agli sviluppatori front-end di sviluppare applicazioni a pagina singola compatibili con l&#39;editor SPA indipendentemente da AEM.
+ **I** componenti core aggiungono una moltitudine di nuovi componenti, una  **libreria di** componenti e diversi miglioramenti ai componenti core esistenti.
+ Ulteriori miglioramenti **Translations** semplificano la traduzione di AEM Sites.

### Esperienze fluide

AEM continua ad abbracciare esperienze fluide con strumenti nuovi e migliorati che facilitano l’utilizzo di contenuti al di fuori di AEM.

+ **Frammenti** di contenuto supporta il confronto delle versioni/differenze e le annotazioni.
+ **AEM le** API HTTP di Assets supportano l’esposizione di  **frammenti di contenuto** direttamente nel DAM come  **JSON**.
   **Frammenti esperienza** supportano la funzione  **Fulltext** Searchand  **AEM** Invalidazione della cache del dispatcher per riferimenti alle  **pagine**.

### Gestione delle risorse

AEM Assets continua a sfruttare le sue numerose funzionalità di gestione delle risorse per migliorare l’utilizzo, la gestione e la comprensione di DAM. AEM 6.5 continua a migliorare l’integrazione tra Adobe Creative Cloud e i flussi di lavoro creativi.

+ **Adobe Asset** Linkconnette i creativi direttamente ad AEM Assets dagli strumenti Adobe Creative Cloud.
+ **L’integrazione di Adobe** Stock consente l’accesso diretto alle immagini Adobe Stock direttamente dall’esperienza AEM Assets, creando un’esperienza di individuazione dei contenuti perfetta.
+ **AEM Desktop** Appreviene la versione 2.0 e si riprevede migliorando le prestazioni e la stabilità.
+ **Risorse collegate** supporta istanze AEM Sites discrete per accedere e utilizzare in modo semplice le risorse di un’altra istanza AEM Assets.
+ È stato aggiornato il supporto video in **Dynamic Media**, inclusi **360 Video** e **Miniature video personalizzate**.

### Intelligenza dei contenuti

AEM continua a sviluppare la sua integrazione con le tecnologie intelligenti, sfruttando l’apprendimento automatico e l’intelligenza artificiale per migliorare tutte le esperienze.

+ **Adobe Asset** Linkadd Ricerca per similarità  **visiva**, per consentire di individuare e utilizzare facilmente immagini simili all’interno degli strumenti **** Adobe Creative Cloud.

### Integrazioni

AEM aumenta la sua capacità di integrarsi con altri servizi Adobe:

+ **I** frammenti esperienza approfondiscono l’integrazione con  **Adobe** Target grazie al supporto di  **Export as** JSONto Adobe Target (Esporta come  **JSON) e la possibilità di** eliminare  **offerte basate su frammenti esperienza da** Adobe Target.

### Gestione cloud AMS

[Cloud Manager](https://adobe.ly/2HODmsv), esclusivo dei clienti di Adobe Managed Services (AMS), offre le seguenti funzionalità:

+ Cloud Manager supporta l’estensione AEM supporto per la distribuzione da AEM Sites a **AEM Assets**, incluso **test delle prestazioni automatizzate dell’elaborazione delle risorse**.
+ **Il** ridimensionamento automatico del livello di pubblicazione AEM a soglie predefinite assicura un’esperienza utente finale ottimale.
+ **Le** pipeline non di produzione consentono ai team di sviluppo di sfruttare Cloud Manager per controllare continuamente la qualità del codice e implementarle in ambienti più bassi (sviluppo e controllo qualità).
+ **API pipeline CI/CD:** consente ai clienti di interagire in modo programmatico con Cloud Manager, ampliando le possibilità di integrazione con l’infrastruttura di sviluppo on-premise.

## Funzioni di base

Di seguito è riportata una matrice delle caratteristiche principali offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti con miglioramenti incrementali aggiunti in ogni versione.

+ [Note sulla versione di AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***Miglioramenti <sup>+</sup> significativi alla funzione in questa versione.***

***✔ <sup></sup> SP indica che la funzione è disponibile tramite un Service Pack o un Feature Pack.***

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
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak Content Repository</a>: </strong> fornisce prestazioni e scalabilità notevolmente superiori rispetto al predecessore Jackrabbit 2.</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">supporto dell'indice oak-run.jar</a>: </strong> miglioramento della reindicizzazione, della raccolta delle statistiche e del controllo di coerenza degli indici Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Indici</a> di ricerca personalizzati:  </strong>
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
                eseguire la manutenzione del repository senza tempi di inattività del server.</td>
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
                <br> opzioni per utilizzare un archivio semplice e performante basato su file di TarMK (versione di nuova generazione di TarPM) 
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3 DataStore</a>:</strong>
            sfrutta una soluzione di archiviazione cloud espandibile per archiviare risorse binarie.</td>
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
                 miglioramenti continui all’interfaccia utente di authoring per velocità con maggiore produttività e parità di funzioni con l’interfaccia classica.</td>
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
                consente di cercare e navigare rapidamente AEM.</td>
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
 esegui la manutenzione, monitora lo stato del server e analizza le prestazioni dall'interno di AEM.</td>
            <td></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Miglioramenti all'aggiornamento</a>:</strong>
            i miglioramenti all'aggiornamento consentono aggiornamenti sul posto più semplici e rapidi di AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/it/experience-manager/htl/using/overview.html" target="_blank">HTL Template Language</a> (Linguaggio modello HTL):</strong>
            un moderno motore di modelli che separa la presentazione dalla logica. Riduce notevolmente i tempi di sviluppo dei componenti. Funzionalità incrementali aggiunte a ogni versione.</td>
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
            framework flessibile per modellare le risorse JCR in oggetti e logiche aziendali. Funzionalità incrementali aggiunte a ogni versione.
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
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>:  </strong>
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

***✔ <sup>+</sup> indica che la funzione è disponibile tramite un Service Pack o un Feature Pack.***

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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Service </a></strong>
            <br> UsersCompartmentalizza le autorizzazioni per evitare un uso non necessario dei privilegi di amministratore.</td>
        <td></td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Archivio chiavi </a></strong>
            <br> GestioneArchivio globale attendibile, certificati e chiavi tutti gestiti all'interno dell'archivio.</td>
        <td></td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> Protezione CSRFprotectionProtezione cross-site Request Forgery preconfigurata.</td>
        <td></td>
        <td></td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> CORSsupportSupporto per condivisione risorse tra origini per una maggiore flessibilità delle applicazioni.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">Miglioramento del </a><br>
 </strong>supporto dell'autenticazione SAMLSono stati migliorati il reindirizzamento SAML, le informazioni sui gruppi ottimizzate e i problemi di crittografia delle chiavi. 
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP come </a><br>
 </strong>configurazione OSGiSemplifica la gestione e gli aggiornamenti dell'autenticazione LDAP.</td>
        <td></td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
    </tr>
    <tr>
        <td><strong>Il supporto della crittografia OSGi per <br>
 </strong>password in formato testo normaleLe password e altri valori sensibili possono essere salvati in forma crittografata e automaticamente decrittografati.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">Miglioramenti </a><br>
 </strong>a CUGl’implementazione di Gruppo utenti chiuso è stata riscritta per risolvere problemi di prestazioni e scalabilità.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>↓</td>
        <td>↓<sup>+</sup></td>
        <td>↓</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">Interfaccia </a></strong>
            <br> guidata SSL per semplificare la configurazione e la gestione di SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Supporto per token </a></strong>
            <br> incapsulatiNon più necessario per sessioni "permanenti" per supportare l’autenticazione orizzontale nelle istanze di pubblicazione.</td>
        <td> </td>
        <td> </td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
        <td>↓</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Adobe IMS Authentication </a><br>
 </strong>SupportEsclusivo per Adobe Managed Services (AMS), gestisci centralmente l’accesso alle istanze di authoring AEM tramite Adobe IMS (Identity Management System).</td>
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

***Miglioramenti <sup>+</sup> significativi alla funzione in questa versione.***

***✔ <sup></sup> SP indica che la funzione è disponibile tramite un Service Pack o un Feature Pack.***

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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Touch Optimized Page Authoring</a> (Creazione pagina ottimizzata):</strong>
            consente agli editor di sfruttare tablet e computer con schermi touch.</td>
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
                la modalità di layout consente agli editor di ridimensionare i componenti in base alla larghezza del dispositivo per i siti reattivi.</td>
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
            consente agli autori specializzati di creare e modificare modelli di pagina.</td>
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
            velocizza lo sviluppo del sito. Disponibile su GitHub per una pianificazione e flessibilità frequenti delle versioni.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPA Editor</a>:</strong>
            crea esperienze web authoring e coinvolgenti utilizzando i framework delle applicazioni a pagina singola (SPA) basati su React o Angular.</td>
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
            aumenta AEM componente riutilizzandolo definendone l’aspetto visivo tramite il sistema di stili nel contesto.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>●<sup>SP</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Multi-Site Manager (MSM)</a>:</strong>
            consente di gestire più siti web che condividono contenuti comuni (ad esempio più lingue, più marchi).</td>
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
            il framework di plug and play si integra con i principali servizi di traduzione di terze parti del settore.</td>
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
            framework client di nuova generazione per la personalizzazione dei contenuti.</td>
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
            sviluppa i contenuti per una versione futura senza interrompere la creazione quotidiana.</td>
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
            crea e cura contenuti editoriali scollegati dalla presentazione per un facile riutilizzo.</td>
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
            crea esperienze e varianti riutilizzabili ottimizzate per i canali desktop, mobili e social.</td>
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
            esporta contenuti da AEM come JSON per utilizzarli su dispositivi e applicazioni.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>●<sup>SP</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong>Integrazione di Adobe Analytics e informazioni sui contenuti:</strong>
                facile integrazione di Adobe Analytics e DTM. Visualizza informazioni sulle prestazioni nell’ambiente Authoring.</td>
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
            procedura guidata dettagliata per creare esperienze mirate e creare librerie di offerte riutilizzabili.</td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Integrazione di Adobe Campaign</a>:</strong>
            facile integrazione con la soluzione e-mail di nuova generazione per campagne.</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Integrazione Adobe Launch</a>: </strong>
            integrazione con il servizio cloud di gestione tag di nuova generazione di Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong>Screens:</strong>
            gestire le esperienze per il digital signage e i chioschi.</td>
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
            puoi fornire esperienze di acquisto personalizzate attraverso siti web, dispositivi mobili e punti di contatto social.
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
            forum, commenti thread, calendari di eventi e molte altre funzionalità consentono un coinvolgimento profondo dei visitatori del sito.</td>
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

***✔ <sup>+</sup> indica che la funzione è disponibile tramite un Service Pack o un Feature Pack.***

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
            gestire le risorse su un computer desktop o su dispositivi touch.</td>
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
            modelli di metadati, editor di schemi di metadati e modifica di metadati in blocco.</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank"></a> Task e  <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank"></a> WorkflowManagement:</strong>
            flussi di lavoro e attività pregenerati per la revisione e l’approvazione delle risorse digitali che sfruttano i progetti AEM.</td>
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
            supporto migliorato per l’acquisizione, il caricamento e lo storage su larga scala.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API HTTP delle risorse</a>:</strong>
            interagisce in modo programmatico con le risorse tramite HTTP e JSON.</td>
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
            condivisione ad hoc semplice delle risorse digitali senza necessità di accedere.</td>
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
            soluzione SAAS di Cloud Service per la condivisione e la distribuzione senza soluzione di continuità delle risorse digitali.</td>
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
            le istanze AEM Sites possono accedere e utilizzare facilmente le risorse di un’altra istanza di AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Asset Insights</a>:</strong>
            sfrutta Adobe Analytics per acquisire l’interazione del cliente sulle risorse digitali e visualizzarla in AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Risorse multilingue</a>: </strong>
            supporto automatico della traduzione dei metadati delle risorse con radici della lingua.</td>
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
            sfrutta Adobe Sensei per assegnare automaticamente tag alle immagini con metadati utili.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Ricerca di traduzione avanzata</a>:</strong>
            traduce automaticamente i termini di ricerca durante la ricerca di AEM Assets.</td>
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
            generazione di cataloghi di prodotti. Crea opuscoli, volantini e annunci per la stampa in base ai modelli di InDesign.</td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/using.html?lang=it" target="_blank">AEM Desktop App</a>:</strong>
            sincronizza le risorse sul desktop locale per la modifica con i prodotti Creative Suite.
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Libreria di imaging di Adobe</a>: </strong>
                <br> librerie Photoshop e Acrobat PDF utilizzate per la manipolazione di file di alta qualità.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe Asset Link</a>:</strong>
            accedere ad AEM Assets direttamente dalle applicazioni Adobe Create Cloud.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Integrazione di Adobe Stock</a>:</strong>
            è possibile accedere e utilizzare facilmente le immagini Adobe Stock direttamente da AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>●<sup>SP</sup></td>
            <td>↓</td>
        </tr>
    </tbody>
</table>

### AEM Assets Dynamic Media

***Miglioramenti <sup>+</sup> significativi alla funzione in questa versione.***

***✔ <sup></sup> SP indica che la funzione è disponibile tramite un Service Pack o un Feature Pack.***


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
            distribuisce dinamicamente immagini di diverse dimensioni e formati, tra cui Smart Crop.</td>
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
            codifica video avanzata e streaming video adattivo</td>
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
            crea banner interattivi e video con contenuti cliccabili per mostrare le offerte chiave.
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
            <td><strong>Set (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Immagine</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Centrifuga</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">File multimediali diversi</a>):</strong>
            Consenti agli utenti di ingrandire, scorrere, ruotare e simulare un’esperienza di visualizzazione a 360 gradi.</td>
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
            lettori e predefiniti per contenuti multimediali personalizzati con marchio personalizzato, con supporto per diversi schermi/dispositivi.</td>
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
            opzioni flessibili per il collegamento o l’incorporazione di contenuti Dynamic Media e la consegna tramite protocollo HTTP/2.</td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong>Aggiornamento da Scene7 a Dynamic Media: </strong>
            possibilità di migrare le risorse master e continuare a utilizzare gli URL S7 esistenti.</td>
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

***Miglioramenti <sup>+</sup> significativi alla funzione in questa versione.***

***✔ <sup></sup> SP indica che la funzione è disponibile tramite un Service Pack o un Feature Pack.***

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
            crea moduli coinvolgenti, reattivi e adattivi in base alle impostazioni del dispositivo e del browser.</td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Documento di record</a>:</strong>
            crea un documento per garantire l’archiviazione a lungo termine di un’esperienza di acquisizione dei dati o di una versione pronta per la stampa.</td>
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
            consente di creare temi riutilizzabili per assegnare uno stile ai componenti e ai pannelli di un modulo.</td>
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
            standardizzare e implementare le best practice per i moduli adattivi.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Integrazione di Adobe Sign</a>:</strong>
            consente la distribuzione di scenari di firma basati su moduli integrati di Adobe Sign.</td>
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
            con AEM Forms puoi creare, gestire e distribuire corrispondenze personalizzate e interattive per i clienti.
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
            utilizzando l’integrazione dei dati, i dati vengono recuperati da origini dati diverse in base agli input degli utenti in un modulo. All’invio del modulo, i dati acquisiti vengono riscritti nelle origini dati.
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Flusso di lavoro (su OSGi) per Forms Processing</a>:</strong>
            distribuzione semplificata dei processi di approvazione dei moduli.</td>
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
            integrazione con Adobe Analytics e Adobe Target per migliorare e misurare le esperienze dei clienti.</td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Form Manager</a>:</strong>
            una singola posizione per gestire tutti i moduli/documenti/corrispondenza, ad esempio per abilitare analisi, traduzione, test A/B, revisioni e pubblicazione.
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
            consente l’elaborazione di moduli online/offline all’interno di un’app su iOS, Android o Windows.</td>
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
            crea comunicazioni avanzate, ad esempio dichiarazioni mirate, con elementi interattivi quali grafici (precedentemente noti come Documenti adattivi).</td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Flusso di lavoro (J2EE) per Forms Processing:</strong>
            creazione di moduli complessi/flussi di lavoro incentrati sui documenti utilizzando un IDE intuitivo.</td>
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
            accesso e autorizzazione sicuri dei documenti PDF e Office.
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
            utilizza il framework Calvin e il plug-in Chrome per supportare ed eseguire il debug dei moduli adattivi.</td>
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

***Miglioramenti <sup>+</sup> significativi alla funzione in questa versione.***

***✔ <sup></sup> SP indica che la funzione è disponibile tramite un Service Pack o un Feature Pack.***

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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">Forum</a>:</strong>  (Social Component Framework) crea nuovi argomenti o visualizza, segue, cerca e sposta gli argomenti esistenti.</td>
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
                poni, visualizza e rispondi alle domande.</p>
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
                crea articoli e commenti di blog sul lato della pubblicazione.
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
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">Ideazione</a>:</strong>
                crea e condividi idee con la comunità, o visualizza, segui e commenta idee esistenti.
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
                 (Social Component Framework) fornisce informazioni sugli eventi della community ai visitatori del sito.
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
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">Libreria di file</a>:</strong>
                carica, gestisci e scarica file all’interno del sito della community.</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">Gruppi</a> di utenti: 
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
            crea e assegna risorse di apprendimento ai membri della community.</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td rowspan="5">Attivazione</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank"></a> Gestione  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">risorse e cataloghi</a>: </strong>
            accedere alle risorse di abilitazione dal catalogo.</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">Gestione dei percorsi di apprendimento</a>: </strong>
            gestire corsi o gruppi di risorse di abilitazione.</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">Generazione di rapporti di abilitazione</a>: </strong>
            generazione di rapporti sulle risorse di abilitazione e sui percorsi di apprendimento.</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">Coinvolgimento in Abilitazione</a>:</strong>
            aggiungi commenti sulle risorse di abilitazione.</td>
            <td> </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analisi di abilitazione</a>: </strong>
            analisi dei video, generazione di rapporti sull'avanzamento e rapporti sulle assegnazioni</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank"></a> Commenti e allegati:</strong>
             (Social Component Framework) In qualità di membro della community condividi opinioni e conoscenze sui contenuti sul sito Community.</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong>Conversione dei frammenti di contenuto:</strong>
            consente di convertire i contributi UGC in frammenti di contenuto.</td>
            <td> </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">Recensioni</a>:</strong>
                 (Social Component Framework) In qualità di membro della community, rivedi un contenuto utilizzando una combinazione di commenti e funzioni di valutazione.</td>
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
            allega tag (parole chiave o etichette) con contenuto per individuare rapidamente il contenuto.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">Ricerca</a>:</strong>
            ricerche predittive e suggestive.</td>
            <td> </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">Traduzione</a>:</strong>
            traduzione automatica dei contenuti generati dall’utente.</td>
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
            creazione di siti con funzioni di community.</td>
            <td> </td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Modelli</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank"></a> modelli di sito e  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank"></a> di gruppo per la creazione guidata di siti community completamente funzionanti.</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong>Modelli modificabili:</strong>
            consente agli amministratori della community di creare esperienze avanzate utilizzando AEM modelli modificabili.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">Gruppi o sottocomunità</a>:</strong>
            crea in modo dinamico sottocomunità all’interno dei siti delle community.
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
            moderazione del contenuto generato dall’utente.
            </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">Moderazione di gruppo</a>:</strong>
            console di moderazione per gestire in massa il contenuto generato dall’utente.</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">Filtri di rivelazione e Profanity</a>: </strong>
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
            consente di gestire profili utente e gruppi dall’area di gestione membri.</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">Progettazione reattiva</a>:</strong>
            i siti AEM Communities sono reattivi.
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
            effettua l’integrazione con Adobe Analytics per ottenere informazioni chiave sull’utilizzo dei siti di Communities.</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td rowspan="4">Membri</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">Punteggio e Badging</a>:</strong>
             (Punteggio avanzato fornito da Adobe Sensei) identifica i membri della community come esperti e li ricompensa.</td>
            <td> </td>
            <td> </td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank"></a> Attività e  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">notifiche</a>:</strong>
            visualizza il flusso di attività recenti e riceve notifiche su eventi di interesse.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">Messaggi</a>:</strong>
            messaggistica diretta a utenti e gruppi.</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">Accesso a Social</a>:</strong>
            accedi con il loro account Facebook o Twitter.</td>
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
            il contenuto generato dall’utente (UGC) viene mantenuto direttamente in un’istanza MongoDB locale</td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP (Database Storage)</a>:</strong>
            il contenuto generato dall'utente (UGC) viene mantenuto direttamente in un'istanza di database locale MySQL.</td>
            <td> </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP (Cloud Storage)</a>:</strong>
                il contenuto generato dall’utente (UGC) viene mantenuto in remoto in un servizio cloud ospitato e gestito da Adobe.</td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                il contenuto della community è memorizzato in JCR e UGC è accessibile dall’istanza di authoring (o pubblicazione) a cui è stato pubblicato.</td>
            <td> </td>
            <td> </td>
            <td>↓<sup>+</sup></td>
            <td>↓</td>
            <td>↓</td>
            <td>↓</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">Sincronizzazione di utenti e gruppi</a>:</strong>
            sincronizza utenti e gruppi tra le istanze di pubblicazione quando utilizzi una topologia di Publish farm.</td>
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

+ **@** mentionsupport nei contenuti generati dagli utenti.
+ Miglioramenti all&#39;accessibilità tramite **Navigazione tastiera** nei componenti **Abilitazione**.
+ Miglioramento della **moderazione di gruppo** utilizzando **Filtri personalizzati**.
+ **Modelli** modificabili per consentire agli amministratori della community di creare esperienze di community avanzate in AEM.
+ Gli utenti ora possono inviare **messaggi diretti in blocco** a tutti i membri di un gruppo.
