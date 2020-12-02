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

+ **Supporto per Java 11** (mantenendo al contempo il supporto per Java 8).

### Creazione e gestione di siti Web

 AEM Sites introduce una serie di funzionalità progettate per accelerare la creazione e la creazione di siti Web:

+ **SPA** Editorsupport consente di creare SPA (applicazioni a pagina singola) in AEM, supportando un&#39;esperienza di authoring ricca e intuitiva.
+_ **SDK JavaScript**, un kit di avvio SPA progetto e strumenti di generazione di supporto, consentono agli sviluppatori front-end di sviluppare applicazioni di pagina singola compatibili con l&#39;editor SPA indipendentemente da AEM.
+ **Componenti di base** aggiunge una serie di nuovi componenti, una  **libreria di** componenti e una serie di miglioramenti ai componenti core esistenti.
+ Ulteriori **Traduzioni** miglioramenti semplificano la traduzione di  AEM Sites.

### Esperienze fluide

AEM continua ad abbracciare esperienze fluide con strumenti nuovi e migliorati che facilitano l&#39;utilizzo di contenuti al di fuori dei AEM.

+ **I** frammenti di contenuto supportano il confronto delle versioni/differenze e le annotazioni.
+ **AEM Assets HTTP** API supporta l’esposizione di  **frammenti di** contenuto direttamente in DAM come  **JSON**.
   **I** frammenti esperienza supportano  **la** ricerca full-text e  **AEM** invalidazione della cache del dispatcher per fare riferimento alle  **pagine**.

### Gestione delle risorse

 AEM Assets continua a sfruttare la vasta gamma di funzionalità di gestione delle risorse per migliorare l’utilizzo, la gestione e la comprensione di DAM. AEM 6.5 continua a migliorare l&#39;integrazione tra Adobe Creative Cloud e i flussi di lavoro creativi.

+ **risorsa Adobe** collega i creativi direttamente a  AEM Assets dagli strumenti Adobe Creative Cloud.
+ **&#39;integrazione di Adobe** Stock consente l&#39;accesso diretto alle immagini  Adobe Stock direttamente dall&#39;esperienza AEM Assets , creando un&#39;esperienza di scoperta dei contenuti perfetta.
+ **AEM Desktop** Apprema la versione 2.0 e si ripropone migliorando le prestazioni e la stabilità.
+ **Le** risorse collegate supportano  istanze AEM Sites distinte per accedere e utilizzare in modo diretto le risorse da un’istanza AEM Assets  diversa.
+ Supporto video aggiornato in **Contenuti multimediali dinamici**, inclusi **360 Video** e **Miniature video personalizzate**.

### Content intelligence

AEM continua a costruire la sua integrazione con tecnologie intelligenti, sfruttando l&#39;apprendimento automatico e l&#39;intelligenza artificiale per migliorare tutte le esperienze.

+ **Adobe** Collegamento risorse consente di eseguire ricerche **per** similarità visiva, per individuare e utilizzare facilmente immagini simili negli strumenti **** Adobe Creative Cloud.

### Integrations (Integrazioni)

AEM la sua capacità di integrarsi con altri servizi  Adobe:

+ **I** frammenti esperienza approfondiscono la loro integrazione con  **Adobe** Target, supportando  **Esporta come** JSONper  Adobe Target e la possibilità di  **eliminare** offerte basate su frammenti esperienza da  **Adobe Target**.

### Gestione di AMS Cloud

[Cloud Manager](https://adobe.ly/2HODmsv), esclusivo per i clienti di Adobe Managed Services (AMS), offre le seguenti funzionalità:

+ Cloud Manager supporta l&#39;estensione AEM supporto di distribuzione da  AEM Sites a **AEM Assets**, incluso il **test automatico delle prestazioni dell&#39;elaborazione delle risorse**.
+ **Ridimensionamento automatico** del livello AEM Publish a soglie predefinite, per garantire un&#39;esperienza utente finale ottimale.
+ **I** oleodotti non di produzione consentono ai team di sviluppo di utilizzare Cloud Manager per controllare continuamente la qualità del codice e distribuirli in ambienti più bassi (sviluppo e QA).
+ **API** di pipeline CI/CDconsente ai clienti di interagire in modo programmatico con Cloud Manager, ampliando le possibilità di integrazione con l&#39;infrastruttura di sviluppo locale.

## Funzioni di base

Di seguito è riportata una matrice di caratteristiche fondamentali offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti miglioramenti incrementali aggiunti in ciascuna release.

+ [Note sulla versione di AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔ <sup>+miglioramenti </sup> significativi alla funzione in questa versione.***

***✔ <sup></sup> SPindica che la funzione è disponibile tramite Service Pack o Feature Pack.***

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
            <td>③</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak Content Repository</a>:</strong> Fornisce prestazioni e scalabilità molto maggiori rispetto al predecessore Jackrabbit 2.</td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">supporto</a> dell'indice oak-run.jar: indicizzazione </strong> migliorata, raccolta di statistiche e controllo di coerenza degli indici Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Indici</a> di ricerca personalizzati:  </strong>
                Possibilità di aggiungere definizioni di indice personalizzate per ottimizzare le prestazioni della query e la rilevanza della ricerca.</td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Pulizia</a> revisioni online:</strong>
                eseguire la manutenzione dell'archivio senza tempi di inattività del server.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">Archivio</a> archivio TarMK o MongoMK:</strong>
                <br> Opzioni per utilizzare un archivio semplice e performante basato su file di TarMK (versione di nuova generazione di TarPM) 
                <br> o scalare orizzontalmente con un repository con MongoDB con MongoMK.</td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">Prestazioni e stabilità</a> MongoMK:</strong>
            Sono stati apportati continui miglioramenti a MongoMK da quando è stato introdotto con AEM 6.0.</td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore"> Amazon S3 DataStore</a>:</strong>
            Sfruttare la soluzione di archiviazione cloud espandibile per memorizzare risorse binarie.</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong>Parità delle funzioni dell’interfaccia utente touch:</strong>
                miglioramenti continui all’interfaccia utente di authoring per velocità, con maggiore produttività e funzionalità uniformi nell’interfaccia classica.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong>Omnisearch:</strong>
                Ricerca e navigazione rapide AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Pannello</a> delle operazioni:</strong>
 eseguire la manutenzione, monitorare lo stato del server e analizzare le prestazioni dall'interno AEM.</td>
            <td></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Miglioramenti</a> all'aggiornamento:miglioramenti </strong>
            all'aggiornamento semplificano e velocizzano gli aggiornamenti interni di AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/htl/using/overview.html" target="_blank">HTL Template Language</a>:</strong>
            Un moderno motore di modellazione che separa la presentazione dalla logica. Riduzione significativa dei tempi di sviluppo dei componenti. Funzioni incrementali aggiunte a ciascuna versione.</td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Modelli</a> Sling:</strong>
            Un framework flessibile per la modellazione delle risorse JCR in oggetti aziendali e logica. Funzioni incrementali aggiunte a ciascuna versione.
            </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>:  </strong>
                Esclusivo per i clienti di Adobe Managed Services (AMS), Cloud Manager accelera lo sviluppo e la distribuzione tramite una pipeline CI/CD all'avanguardia.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Caratteristiche di sicurezza

Di seguito è riportata una matrice di funzioni di sicurezza chiave offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti miglioramenti incrementali aggiunti in ciascuna release.

+ [Note sulla versione di sicurezza](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***③ indica che sono stati apportati miglioramenti significativi alla funzionalità in questa versione.***

***✔ <sup>+</sup> indica che la funzione è disponibile tramite Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Funzionalità di protezione</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Service </a></strong>
            <br> UsersCompartmentalizza le autorizzazioni evitando l'uso non necessario dei privilegi di amministratore.</td>
        <td></td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Key Store </a></strong>
            <br> ManagementArchivio trust globale, certificati e chiavi gestiti all'interno dell'archivio.</td>
        <td></td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>Protezione </strong> <strong></strong></a>
            <br> CSRFprotectedProtezione della contraffazione richiesta all'esterno della confezione.</td>
        <td></td>
        <td></td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>Supporto per </strong> <strong></strong></a>
            <br> CORSsupportSupporto per la condivisione delle risorse tra origini per una maggiore flessibilità dell'applicazione.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
    </tr>
    <tr>
        <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/security/saml-2-0-authenticationhandler.html" target="_blank">Miglioramento del </a><br>
 </strong>supporto dell'autenticazione SAMLrocedimento SAML migliorato, informazioni di gruppo ottimizzate e problemi di crittografia delle chiavi risolti. 
            <br>
        </td>
        <td></td>
        <td></td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP come </a><br>
 </strong>configurazione OSGiSemplifica la gestione e gli aggiornamenti dell’autenticazione LDAP.</td>
        <td></td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
    </tr>
    <tr>
        <td><strong>Supporto della crittografia OSGi per <br>
 </strong>password in formato sempliceLe password e altri valori sensibili possono essere salvate in forma crittografata e decrittografate automaticamente.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">Miglioramenti CUG</a><br>
 </strong>Implementazione del gruppo utenti chiuso riscritta per risolvere problemi di prestazioni e scalabilità.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>③</td>
        <td>③<sup>+</sup></td>
        <td>③</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL </a></strong>
            <br> WizardUI per semplificare la configurazione e la gestione di SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Supporto per token </a></strong>
            <br> incapsulatiNon è più necessario per sessioni "appiccicose" per supportare l'autenticazione orizzontale nelle istanze di pubblicazione.</td>
        <td> </td>
        <td> </td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
        <td>③</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank"> Adobe </a><br>
 </strong>Supporto autenticazione IMSEsclusivo ai servizi gestiti Adobe (AMS), gestisci centralmente l'accesso alle istanze di AEM Author tramite  Adobe IMS ( Identity Management System).</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>③</td>
        <td>③</td>
    </tr>
</tbody>
</table>

## Caratteristiche dei siti

Di seguito è riportata una matrice di funzioni principali di Siti offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti miglioramenti incrementali aggiunti in ciascuna release.

+ [ note sulla versione di AEM Sites](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔ <sup>+miglioramenti </sup> significativi alla funzione in questa versione.***

***✔ <sup></sup> SPindica che la funzione è disponibile tramite Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td><strong>Funzionalità Siti</strong></td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Touch Optimized Page Authoring</a>:</strong>
            Consente agli editor di utilizzare tablet e computer con schermi touch screen.</td>
            <td></td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Creazione</a> siti reattiva:</strong>
                la modalità di layout consente agli editor di ridimensionare i componenti in base alla larghezza del dispositivo per i siti reattivi.</td>
            <td></td>
            <td></td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Modelli</a> modificabili:</strong>
            consentono agli autori specializzati di creare e modificare modelli di pagina.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">Componenti</a> di base:</strong>
            accelerare lo sviluppo del sito. Disponibile su GitHub per una pianificazione e flessibilità frequenti delle versioni.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPA Editor</a>:</strong>
            creare esperienze Web creabili e coinvolgenti utilizzando i framework di applicazioni (SPA) a pagina singola basati su React o Angular.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/style-system-fp.html" target="_blank">Sistema</a> di stile:</strong>
            Aumenta AEM componente riutilizzandolo definendone l’aspetto visivo mediante il sistema di stile in-context.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③<sup>SP</sup></td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Multi-Site Manager (MSM)</a>:</strong>
            gestire più siti Web che condividono contenuti comuni (ad esempio, più lingue, più marchi).</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Traduzione</a> dei contenuti:</strong>
            il framework Plug and Play si integra con i servizi di traduzione di terze parti leader del settore.</td>
            <td></td>
            <td></td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:framework cliente di </strong>
            nuova generazione per la personalizzazione dei contenuti.</td>
            <td></td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Lanci</a>:</strong>
            sviluppare contenuti per rilasci futuri senza interrompere l'authoring quotidiano.</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-understand.html" target="_blank">Frammenti</a> di contenuto:</strong>
            creare e curare contenuti editoriali scollegati dalla presentazione per facilitarne il riutilizzo.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Frammenti</a> esperienza:</strong>
            creare esperienze e varianti riutilizzabili ottimizzate per i canali desktop, mobili e social.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/content-services-fragments-featurepack.html" target="_blank">Content Services</a>:</strong>
            Esportare contenuti da AEM come JSON da utilizzare tra dispositivi e applicazioni.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③<sup>SP</sup></td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong> Adobe Analytics Integration and Content Insights:</strong>
                Facile integrazione di  Adobe Analytics e DTM. Visualizzare le informazioni sulle prestazioni nell'ambiente Authoring.</td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank"> Adobe Target Integration</a>:procedura guidata </strong>
            dettagliata per creare esperienze mirate e creare librerie di offerte riutilizzabili.</td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Integrazione</a> Adobe Campaign : </strong>
            facile integrazione con la soluzione di campagne e-mail di nuova generazione.</td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank"> Adobe Launch Integration</a>:</strong>
            Integrazione con  Adobe  servizio cloud di gestione tag di nuova generazione.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-screens-introduction.html" target="_blank">Schermi</a>:</strong>
            gestire le esperienze per il digital signage e i chioschi.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">eCommerce</a>: </strong>
            distribuzione di esperienze di acquisto personalizzate e personalizzate attraverso i punti di interazione Web, mobile e social.
            </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">Community</a>:</strong>
            forum, commenti concatenati, calendari evento e molte altre funzioni consentono un coinvolgimento diretto con i visitatori del sito.</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Funzioni delle risorse

Di seguito è riportata una matrice di funzioni chiave di Risorse offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti miglioramenti incrementali aggiunti in ciascuna release.

+ [ note sulla versione di AEM Assets](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***③ indica che sono stati apportati miglioramenti significativi alla funzionalità in questa versione.***

***✔ <sup>+</sup> indica che la funzione è disponibile tramite Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Funzionalità Risorse</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Interfaccia</a> touch:</strong>
            gestire le risorse su un computer desktop o su dispositivi touch.</td>
            <td> </td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">Gestione</a> avanzata dei metadati: modelli di </strong>
            metadati, Editor schema metadati e Modifica in blocco dei metadati.</td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">Gestione </a> delle attività e dei  <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank"></a> flussi di lavoro:flussi di lavoro e attività </strong>
            pregenerati per la revisione e l'approvazione di risorse digitali che sfruttano AEM progetti.</td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong>Scalabilità e prestazioni: supporto </strong>
            migliorato per l’assimilazione, il caricamento e l’archiviazione su scala.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API</a> HTTP Assets:interazione </strong>
            programmatica con le risorse tramite HTTP e JSON.</td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Condivisione</a> dei collegamenti:condivisione ad hoc </strong>
            semplice delle risorse digitali senza necessità di accedere.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>:soluzione SAAS per servizi </strong>
            Cloud per la condivisione e la distribuzione senza soluzione di continuità delle risorse digitali.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Risorse</a> connesse:</strong>
             istanze AEM Sites possono accedere e utilizzare direttamente le risorse da un’altra istanza  AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Approfondimenti</a> delle risorse:</strong>
            sfruttare  Adobe Analytics per acquisire l'interazione con i clienti delle risorse digitali e visualizzarle in AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Risorse</a> multilingue:supporto </strong>
            della traduzione automatica dei metadati delle risorse con radici della lingua.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Tag avanzati e moderazione</a>:</strong>
            utilizzate  Adobe Sensei per assegnare automaticamente tag alle immagini con metadati utili.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Ricerca</a> di traduzione intelligente:</strong>
            tradurre automaticamente i termini di ricerca durante la ricerca di  AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank"> Integrazione</a> Adobe InDesign Server:</strong>
            generazione di cataloghi di prodotti. Crea brochure, volantini e annunci per la stampa basati sui modelli  InDesign.</td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">AEM Desktop App</a>: </strong>
            sincronizzate le risorse sul desktop locale per la modifica con i prodotti di Creative Suite.
            </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank"> Libreria</a> immagini di Adobe:librerie PDF </strong>
                <br> Photoshop e  Acrobat utilizzate per la manipolazione di file di alta qualità.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank"> collegamento</a> risorsa Adobe:</strong>
            accedere  AEM Assets direttamente da  Adobe applicazioni Creative Cloud.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank"> Integrazione</a> Adobe Stock:accesso </strong>
            semplice e utilizzo  immagini Adobe Stock direttamente da AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③<sup>SP</sup></td>
            <td>③</td>
        </tr>
    </tbody>
</table>

###  AEM Assets Dynamic Media

***✔ <sup>+miglioramenti </sup> significativi alla funzione in questa versione.***

***✔ <sup></sup> SPindica che la funzione è disponibile tramite Service Pack o Feature Pack.***


<table>
    <thead>
        <tr>
            <td>Elemento multimediale dinamico</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Immagini</a>:</strong>
            Distribuzione dinamica di immagini in diverse dimensioni e formati, incluso Smart Crop.</td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Video</a>:codifica video </strong>
            avanzata e streaming video adattivo</td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">Contenuti multimediali</a> interattivi:</strong>
            create banner interattivi, video con contenuti selezionabili per presentare le offerte chiave.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong>Set (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Immagine</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Rotazione</a>, File multimediali <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank"> </a>diversi):</strong>
            Consentite agli utenti di ingrandire, scorrere, ruotare e simulare un’esperienza di visualizzazione di 360 gradi.</td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/content/dynamic-media/viewer-presets.html" target="_blank">Visualizzatori</a>:lettori e predefiniti per contenuti multimediali </strong>
            personalizzati con il logo aziendale, con supporto per schermi/dispositivi diversi.</td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Distribuzione</a>:opzioni </strong>
            flessibili per il collegamento o l'incorporamento di contenuti multimediali dinamici e la distribuzione tramite protocollo HTTP/2.</td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong>Aggiornamento da Scene7 a Dynamic Media:</strong>
            possibilità di migrare le risorse principali e continuare a utilizzare gli URL S7 esistenti.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
    </tbody>
</table>

## Funzioni Forms

Di seguito è riportata una matrice di funzioni chiave  AEM Forms Add-on offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti miglioramenti incrementali aggiunti in ciascuna release.

+ [ note sulla versione di AEM Forms](https://helpx.adobe.com/experience-manager/6-5/release-notes/forms.html)

***✔ <sup>+miglioramenti </sup> significativi alla funzione in questa versione.***

***✔ <sup></sup> SPindica che la funzione è disponibile tramite Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Funzionalità Forms</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Editor</a> Forms adattivo:</strong>
            creazione di moduli coinvolgenti, reattivi e adattativi in base alle impostazioni del dispositivo e del browser.</td>
            <td> </td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Documento di registrazione</a>:</strong>
            creare un documento per garantire l'archiviazione a lungo termine di un'esperienza di acquisizione dei dati o la versione pronta per la stampa.</td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">Editor</a> tema:</strong>
            creare temi riutilizzabili per lo stile di componenti e pannelli di un modulo.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Editor</a> modelli:</strong>
            standardizzazione e implementazione delle procedure ottimali per i moduli adattivi.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank"> integrazione</a> Adobe Sign:</strong>
            consentire la distribuzione  scenari di firma basati su moduli integrati Adobe Sign.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Gestione</a> della corrispondenza:</strong>
            con  AEM Forms, puoi creare, gestire e distribuire corrispondenze personalizzate e interattive per i clienti.
            </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Integrazione</a> dei dati di terze parti:</strong>
            utilizzando l'integrazione dei dati, i dati vengono recuperati da origini dati diverse in base agli input dell'utente in un modulo. Durante l'invio del modulo, i dati acquisiti vengono riscritti alle origini dati.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Flusso di lavoro (su OSGi) per l’elaborazione</a> Forms:distribuzione </strong>
            semplificata dei processi di approvazione dei moduli.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Integrazione con Marketing Cloud</a>:</strong>
            Integrazione con  Adobe Analytics e  Adobe Target per migliorare e misurare le esperienze dei clienti.</td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Form Manager</a>:posizione </strong>
            singola per gestire tutti i moduli/documenti/corrispondenza, ad esempio l'abilitazione di analisi, conversione, test A/B, revisioni e pubblicazione.
            </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank"> app</a> AEM Forms:</strong>
            consente l'elaborazione online/offline di moduli all'interno di un'app su iOS, Android o Windows.</td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Comunicazioni</a> interattive:</strong>
            creazione di comunicazioni avanzate, ad esempio dichiarazioni mirate, con elementi interattivi quali grafici (precedentemente denominati Documenti adattivi).</td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/pdf/aem-forms/6-5/WorkbenchHelp.pdf" target="_blank">Flusso di lavoro (J2EE) per Forms Processing</a>:</strong>
            creazione di moduli complessi/flussi di lavoro basati su documenti mediante un IDE intuitivo.</td>
            <td></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank"> AEM Forms Document Security</a>:accesso </strong>
            sicuro e autorizzazione di documenti PDF e Office.
            </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Framework</a> di test:</strong>
            Utilizzare il framework Calvin e il plug-in Chrome per supportare e debug i moduli adattivi.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
    </tbody>
</table>

## Funzioni di Communities

Di seguito è riportata una matrice di funzioni chiave  AEM Communities Add-on offerte da AEM. Alcune di queste funzionalità sono state introdotte nelle versioni precedenti miglioramenti incrementali aggiunti in ciascuna release.

+ [ riepilogo delle nuove funzioni di AEM Communities](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html#main-pars_text)

***✔ <sup>+miglioramenti </sup> significativi alla funzione in questa versione.***

***✔ <sup></sup> SPindica che la funzione è disponibile tramite Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>Funzioni di Communities</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="7">Funzioni di Communities</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">Forum</a>:</strong> (Social Component Framework) Creazione di nuovi argomenti o visualizzazione, follow, ricerca e spostamento di argomenti esistenti.</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">QnA</a>:</strong>
                porre domande, visualizzare e rispondere.</p>
            </td>
            <td></td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">Blog</a>:</strong>
                creare articoli e commenti di blog sul lato della pubblicazione.
            </td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">Ideazione</a>:</strong>
                creare e condividere idee con la comunità, oppure visualizzare, seguire e commentare le idee esistenti.
            </td>
            <td> </td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">Calendario</a>:</strong>
                (Social Component Framework) Fornisce informazioni sugli eventi della community ai visitatori del sito.
            </td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">Libreria</a> file:</strong>
                caricare, gestire e scaricare i file all'interno del sito della community.</td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">Gruppi</a> di utenti: 
            </strong>Un set di utenti può appartenere a gruppi di membri e può essere assegnato a ruoli collettivamente.</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Assegnazione</a>:</strong>
            creare e assegnare risorse di apprendimento ai membri della community.</td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td rowspan="5">Attivazione</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank"></a> Catalogo e  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Gestione</a> risorse:</strong>
            accedere alle risorse di abilitazione dal catalogo.</td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">Gestione</a> dei percorsi di apprendimento:</strong>
            gestire corsi o gruppi di risorse di abilitazione.</td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">Generazione di rapporti</a> di abilitazione:</strong>
            generazione di rapporti sulle risorse di abilitazione e sui percorsi di apprendimento.</td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">Partecipazione a attivazione</a>:</strong>
            aggiungere commenti sulle risorse di abilitazione.</td>
            <td> </td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analytics</a> abilitazione:analisi </strong>
            video, generazione di rapporti sull'avanzamento e assegnazione</td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank">Commenti e </a> allegati:</strong>
            (Quadro dei componenti sociali) I membri della comunità condividono opinioni e conoscenze sui contenuti del sito Community.</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong>Conversione dei frammenti di contenuto:</strong>
            conversione dei contributi UGC in frammenti di contenuto.</td>
            <td> </td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">Recensioni</a>:</strong>
                (Social Component Framework) In qualità di membro della community, potete esaminare un contenuto utilizzando una combinazione di commenti e funzioni di valutazione.</td>
            <td>③<sup>+</sup></td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">Valutazioni</a>:/strong&gt; (Social Component Framework) Come membro della community, valuta un contenuto.</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">Voti</a>:</strong>
                (Social Component Framework) Come membro della comunità vota o deseleziona un contenuto.</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">Tag</a>:</strong>
            allegare tag (parole chiave o etichette) al contenuto per individuare rapidamente il contenuto.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">Ricerca</a>:ricerche </strong>
            predittive e suggestive.</td>
            <td> </td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">Traduzione</a>:traduzione </strong>
            automatica del contenuto generato dall'utente.</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td rowspan="10">Amministrazione</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">Gestione</a> del sito:</strong>
            creazione di siti con funzioni per community.</td>
            <td> </td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Modelli</a>:modelli </strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank"></a> di siti e  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank"></a> raggruppamenti per la creazione guidata di siti community completamente funzionanti.</td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong>Modelli modificabili:</strong>
            consente agli amministratori della community di creare esperienze avanzate utilizzando AEM Modelli modificabili.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">Gruppi o sub-community</a>:creazione </strong>
            dinamica di sottocommunity all’interno dei siti delle community.
            </td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">Moderazione</a>:</strong>
            Moderazione del contenuto generato dall'utente.
            </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">Moderazione</a> di massa:console </strong>
            Moderazione per gestire in massa il contenuto generato dall'utente.</td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">Spam detection and Profanity Filters</a>:rilevamento </strong>
            automatico dello spam.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">Gestione</a> membri:</strong>
            gestire profili utente e gruppi dall'area di gestione membri.</td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">Responsive Design</a>:</strong>
             siti AEM Communities sono reattivi.
            </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analytics</a>:</strong>
            Integrazione con  Adobe Analytics per informazioni chiave sull'utilizzo dei siti community.</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td rowspan="4">Membri</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">Punteggio e Badging</a>:</strong>
            (punteggio avanzato fornito da  Adobe Sensei) Identifica i membri della community come esperti e li ricompensa.</td>
            <td> </td>
            <td> </td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank"></a> Attività e  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">Notifiche</a>:</strong>
            visualizzare il flusso delle attività recenti e ricevere notifiche sugli eventi di interesse.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">Messaggi</a>:Messaggistica </strong>
            diretta a utenti e gruppi.</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">Login</a> social:</strong>
            accedere con il proprio account Facebook o Twitter.</td>
            <td> </td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td rowspan="5">Platform</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP (Mongo Storage)</a>:contenuto generato dall'</strong>
            utente (UGC) viene mantenuto direttamente in un'istanza MongoDB locale</td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP (Database Storage)</a>:contenuto generato dall'</strong>
            utente (UGC) viene mantenuto direttamente in un'istanza di database MySQL locale.</td>
            <td> </td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP (Cloud Storage)</a>:contenuto generato dall'</strong>
                utente (UGC) è persistente in remoto in un servizio cloud ospitato e gestito da  Adobe.</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:il contenuto </strong>
                della community è memorizzato in JCR e UGC è accessibile dall’istanza di creazione (o pubblicazione) alla quale è stato pubblicato.</td>
            <td> </td>
            <td> </td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">Sincronizzazione</a> di utenti e gruppi:</strong>
            sincronizzate utenti e gruppi tra le istanze di pubblicazione quando utilizzate una topologia della farm di pubblicazione.</td>
            <td>③<sup>+</sup></td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
            <td>③</td>
        </tr>
    </tbody>
</table>

 AEM Communities aggiunge [miglioramenti](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html) attraverso le release per consentire alle organizzazioni di coinvolgere e abilitare i propri utenti, tramite:

+ **@** mentionsupporto nel contenuto generato dall&#39;utente.
+ Miglioramenti all&#39;accessibilità tramite **Navigazione da tastiera** nei componenti **Abilitazione**.
+ Miglioramento della moderazione **Bulk** mediante i **filtri personalizzati**.
+ **Modelli** modificabili per consentire agli amministratori della community di creare esperienze di community avanzate in AEM.
+ Gli utenti ora possono inviare **messaggi diretti in blocco** a tutti i membri di un gruppo.
