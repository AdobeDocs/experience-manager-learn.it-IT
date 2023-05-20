---
title: Estensioni della console Frammenti di contenuto AEM
description: Scopri come generare e distribuire le estensioni della console Frammenti di contenuto as a Cloud Service AEM
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
kt: 11603
thumbnail: KT-11603.png
last-substantial-update: 2022-12-09T00:00:00Z
exl-id: 093c8d83-2402-4feb-8a56-267243d229dd
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 4%

---

# Estensione della console Frammenti di contenuto AEM

[Console Frammenti di contenuto AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=it) le estensioni possono essere aggiunte tramite due punti di estensione: un pulsante nella [Console Frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=it) menu intestazione o barra delle azioni. Le estensioni sono scritte in JavaScript e vengono eseguite come app di App Builder; possono inoltre implementare un’interfaccia utente web personalizzata e azioni Adobe I/O Runtime senza server per eseguire operazioni più intensive e di lunga durata.

![Estensione della console Frammenti di contenuto AEM](./assets/overview/example.png){align="center"}

| Tipo di estensione | Descrizione | Parametro/i |
| :--- | :--- | :--- |
| Menu intestazione | Aggiunge un pulsante all’intestazione che viene visualizzato quando __zero__ I frammenti di contenuto sono selezionati. | Nessuno. |
| Barra delle azioni | Aggiunge un pulsante alla barra delle azioni che viene visualizzato quando __uno o più__ I frammenti di contenuto sono selezionati. | Array dei percorsi dei frammenti di contenuto selezionati. |

Una singola estensione della Console Frammenti di contenuto AEM può includere zero o un menu di intestazione e zero o un tipo di estensione della barra delle azioni. Se sono necessari più tipi di estensione dello stesso tipo, è necessario creare più estensioni della console Frammenti di contenuto AEM.

Le estensioni della console Frammenti di contenuto AEM richiedono un [Progetto console Adobe Developer](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console) e un [App Builder](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) utilizzando `@adobe/aem-cf-admin-ui-ext-tpl` modello, associato al progetto della console Adobe Developer.

Seleziona una delle seguenti funzionalità per generare l&#39;app di App Builder, in base alle funzioni dell&#39;estensione. È possibile utilizzare qualsiasi combinazione di opzioni in un’estensione.

|  | Aggiungi pulsante a [Menu intestazione](./header-menu.md) | Aggiungi pulsante a [Barra delle azioni](./action-bar.md) | Spettacolo [Modale](./modal.md) | Aggiungi [gestore lato server](./runtime-action.md) |
| ------------------------------------------ | :-----------------------: | :----------------------: | :--------: | :--------------------:  |
| Disponibile quando i frammenti di contenuto non sono selezionati | ✔ |  |  |  |
| Disponibile quando sono selezionati uno o più frammenti di contenuto |  | ✔ |  |  |
| Raccoglie input personalizzati dall&#39;utente |  |  | ✔️ |  |
| Visualizza feedback personalizzato per l&#39;utente |  |  | ✔️ |  |
| Richiama le richieste HTTP all’AEM |  |  |  | ✔ |
| Richiama richieste HTTP ai servizi di Adobe/terze parti |  |  |  | ✔ |


## Documentazione di Adobe Developer

Adobe Developer contiene informazioni per gli sviluppatori sulle estensioni della Console Frammenti di contenuto AEM. Rivedi il [Contenuto Adobe Developer per ulteriori dettagli tecnici](https://developer.adobe.com/uix/docs/).

## Sviluppare un’estensione

Segui i passaggi descritti di seguito per scoprire come generare, sviluppare e distribuire un’estensione della console Frammenti di contenuto AEM per AEM as a Cloud Service.

<div class="columns is-multiline">
    <!-- Create Adobe Developer Project -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create Adobe Developer Project">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./adobe-developer-console-project.md" title="Crea progetto Adobe Developer" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/project/card.png" alt="Crea progetto Adobe Developer">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">1. Creare un progetto</p>
                    <p class="is-size-6">Crea un progetto Adobe Developer Console che ne definisce l’accesso ad altri servizi Adobe e ne gestisce le implementazioni.</p>
                    <a href="./adobe-developer-console-project.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Creazione di un progetto Adobe Developer</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Generate an Extension app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Generate an Extension app">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./app-initialization.md" title="Generare un’app di estensione" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/initialize-app/card.png" alt="Inizializzare un’app di estensione">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">2. Inizializzare un’app di estensione</p>
                    <p class="is-size-6">Inizializza un’app App Builder per l’estensione della Console Frammenti di contenuto AEM che definisce dove viene visualizzata l’estensione e il lavoro che esegue.</p>
                    <a href="./app-initialization.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Inizializzare un’app di estensione</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension registration -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension registration">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./extension-registration.md" title="Registrazione dell’estensione" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/extension-registration/card.png" alt="Registrazione dell’estensione">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">3. Registrazione dell’estensione</p>
                    <p class="is-size-6">Registra l’estensione nella console Frammenti di contenuto AEM come tipo di estensione per il menu dell’intestazione o la barra delle azioni.</p>
                    <a href="./extension-registration.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Registrare l'estensione</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Header Menu -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./header-menu.md" title="Menu intestazione" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/header-menu/card.png" alt="Menu intestazione">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4a. Menu intestazione</p>
                    <p class="is-size-6">Scopri come creare un’estensione del menu di intestazione della Console Frammenti di contenuto AEM.</p>
                    <a href="./header-menu.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Estendere il menu dell’intestazione</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Action Bar -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action Bar">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./action-bar.md" title="Barra delle azioni" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/action-bar/card.png" alt="Barra delle azioni">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4b. Barra delle azioni</p>
                    <p class="is-size-6">Scopri come creare un’estensione della barra delle azioni della Console Frammenti di contenuto AEM.</p>
                    <a href="./action-bar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Estendere la barra delle azioni</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Modal -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Modal">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./modal.md" title="Finestra modale" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/modal/card.png" alt="Finestra modale">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">5. Modale</p>
                    <p class="is-size-6">Aggiungi all’estensione una finestra modale personalizzata che può essere utilizzata per creare esperienze personalizzate per gli utenti. I moduli spesso raccolgono input dagli utenti e visualizzano i risultati di un'operazione.</p>
                    <a href="./modal.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aggiungi un modale</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Adobe I/O Runtime action -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adobe I/O Runtime action">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./runtime-action.md" title="Azione Adobe I/O Runtime" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/runtime-action/card.png" alt="Azione Adobe I/O Runtime">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">6. Azione Adobe I/O Runtime</p>
                    <p class="is-size-6">Aggiungi un’azione Adobe I/O Runtime senza server che l’estensione può richiamare per interagire con Frammenti di contenuto e AEM per eseguire operazioni aziendali personalizzate.</p>
                    <a href="./runtime-action.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aggiungere un’azione Adobe I/O Runtime</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Test -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Test">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./test.md" title="Prova" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/test/card.png" alt="Prova">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">7. Prova</p>
                    <p class="is-size-6">Verifica le estensioni durante lo sviluppo e condividi le estensioni completate ai tester QA o UAT utilizzando un URL speciale.</p>
                    <a href="./test.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Testare l’estensione</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension deployment -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension deployment">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./deploy.md" title="Distribuzione dell’estensione" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/deploy/card.png" alt="Distribuzione dell’estensione">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">8. Distribuzione nell’ambiente di produzione</p>
                    <p class="is-size-6">Distribuisci l’estensione in Adobe I/O, rendendola disponibile agli utenti AEM. È possibile aggiornare e rimuovere anche le estensioni.</p>
                    <a href="./deploy.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Implementare in produzione</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
</div>

## Estensioni di esempio

Esempio di estensioni della console Frammenti di contenuto AEM.

<div class="columns is-multiline">
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/bulk-property-update.md" title="Estensione di aggiornamento proprietà in blocco" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/bulk-property-update/card.png" alt="Estensione di aggiornamento proprietà in blocco">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Estensione di aggiornamento proprietà in blocco</p>
                    <p class="is-size-6">Esplora un esempio di estensione della barra delle azioni che aggiorna in blocco una proprietà nei frammenti di contenuto selezionati.</p>
                    <a href="./example-extensions/bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Esplora l’estensione di esempio</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Image Generartion update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="OpenAI-based image generation and upload to AEM extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/image-generation-and-image-upload.md" title="Generazione di immagini basate su OpenAI e caricamento nell’estensione AEM" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/digital-image-generation/screenshot.png" alt="Generazione di immagini basate su OpenAI e caricamento nell’estensione AEM">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Generazione di immagini basate su OpenAI e caricamento nell’estensione AEM</p>
                    <p class="is-size-6">Esplora un esempio di estensione della barra delle azioni che genera un’immagine utilizzando OpenAI, la carica sull’AEM e aggiorna la proprietà dell’immagine nel frammento di contenuto selezionato.</p>
                    <a href="./example-extensions/image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Esplora l’estensione di esempio</span>
                    </a>
                </div>
            </div>
        </div>
    </div>



</div>
