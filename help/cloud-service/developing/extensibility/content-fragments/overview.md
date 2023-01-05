---
title: Estensioni AEM console Frammenti di contenuto
description: Scopri come creare e distribuire AEM estensioni as a Cloud Service della console Frammenti di contenuto
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
kt: 11603
thumbnail: KT-11603.png
last-substantial-update: 2022-12-09T00:00:00Z
source-git-commit: fbc8c11841f5b5e04a99ba74fac6f01dc3e3a2da
workflow-type: tm+mt
source-wordcount: '742'
ht-degree: 4%

---


# Estensione della console Frammenti di contenuto AEM

[Console Frammenti di contenuto AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=it) Le estensioni possono essere aggiunte tramite due punti di estensione: un pulsante [della console Frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=it) menu intestazione o barra delle azioni. Le estensioni sono scritte in JavaScript che vengono eseguite come app per App Builder e possono implementare un’interfaccia utente web personalizzata e azioni Adobe I/O Runtime senza server per eseguire lavori più intensivi e a lungo termine.

![Estensione della console Frammenti di contenuto AEM](./assets/overview/example.png){align="center"}

| Tipo di estensione | Descrizione | Parametri |
| :--- | :--- | :--- |
| Menu Intestazione | Aggiunge un pulsante all’intestazione che viene visualizzata quando __zero__ I frammenti di contenuto sono selezionati. | Nessuno. |
| Barra delle azioni | Aggiunge un pulsante alla barra delle azioni visualizzata quando __uno o più__ I frammenti di contenuto sono selezionati. | Matrice dei percorsi dei frammenti di contenuto selezionati. |

Una singola estensione della console Frammenti di contenuto AEM può includere zero o un menu di intestazione e zero o un tipo di estensione della barra delle azioni. Se sono necessari più tipi di estensione dello stesso tipo, è necessario creare più estensioni AEM console Frammenti di contenuto .

AEM estensioni della console Frammenti di contenuto, richiedono un [Progetto Adobe Developer Console](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console) e [App Builder](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) utilizzando `@adobe/aem-cf-admin-ui-ext-tpl` , associato al progetto Adobe Developer Console .

Scegli una delle funzionalità seguenti durante la generazione dell&#39;app Generatore app, in base alle prestazioni dell&#39;estensione. Qualsiasi combinazione di opzioni può essere utilizzata in un&#39;estensione.

|  | Aggiungi pulsante a [Menu Intestazione](./header-menu.md) | Aggiungi pulsante a [Barra delle azioni](./action-bar.md) | Mostra [modale](./modal.md) | Aggiungi [gestore lato server](./runtime-action.md) |
| ------------------------------------------ | :-----------------------: | :----------------------: | :--------: | :--------------------:  |
| Disponibile quando i frammenti di contenuto non sono selezionati | ↓ |  |  |  |
| Disponibile quando sono selezionati uno o più frammenti di contenuto |  | ✔ |  |  |
| Raccoglie input personalizzati dall&#39;utente |  |  | ✔️ |  |
| Visualizza il feedback personalizzato per l&#39;utente |  |  | ✔️ |  |
| Richiama le richieste HTTP a AEM |  |  |  | ✔ |
| Richiama le richieste HTTP ai servizi di Adobe/di terze parti |  |  |  | ✔ |


## Documentazione di Adobe Developer

Adobe Developer contiene informazioni per sviluppatori sulle estensioni della console AEM frammenti di contenuto . Controlla la [Contenuto Adobe Developer per ulteriori dettagli tecnici](https://developer.adobe.com/uix/docs/).

## Sviluppa un&#39;estensione

Segui i passaggi descritti di seguito per scoprire come generare, sviluppare e distribuire un’estensione della console AEM frammenti di contenuto per AEM as a Cloud Service.

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
                    <p class="is-size-6">Crea un progetto della console Adobe Developer che definisce l’accesso ad altri servizi Adobe e ne gestisce le implementazioni.</p>
                    <a href="./adobe-developer-console-project.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Creare un progetto Adobe Developer</span>
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
                        <img class="is-bordered-r-small" src="./assets/initialize-app/card.png" alt="Inizializzare un'app di estensione">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">2. Inizializzare un'app di estensione</p>
                    <p class="is-size-6">Inizializzare un’app AEM estensione della console Frammenti di contenuto App Builder che definisce dove appare l’estensione e il lavoro che esegue.</p>
                    <a href="./app-initialization.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Inizializzare un'app di estensione</span>
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
                    <a href="./extension-registration.md" title="Registrazione delle estensioni" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/extension-registration/card.png" alt="Registrazione delle estensioni">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">3. Registrazione dell'estensione</p>
                    <p class="is-size-6">Registra l'estensione nella AEM console Frammenti di contenuto come menu di intestazione o tipo di estensione della barra delle azioni.</p>
                    <a href="./extension-registration.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Registra l'estensione</span>
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
                    <a href="./header-menu.md" title="Menu Intestazione" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/header-menu/card.png" alt="Menu Intestazione">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4a. Menu Intestazione</p>
                    <p class="is-size-6">Scopri come creare un’estensione del menu intestazione della console frammenti di contenuto AEM.</p>
                    <a href="./header-menu.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Estende il menu intestazione</span>
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
                    <p class="is-size-6">Scopri come creare un’estensione della barra delle azioni della Console frammenti di contenuto AEM.</p>
                    <a href="./action-bar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Estende la barra delle azioni</span>
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
                    <p class="is-size-6">Aggiungi un modale personalizzato all’estensione che può essere utilizzato per creare esperienze personalizzate per gli utenti. I moduli spesso raccolgono l’input dagli utenti e visualizzano i risultati di un’operazione.</p>
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
                    <p class="is-size-6">Aggiungi un’azione Adobe I/O Runtime senza server richiamabile dall’estensione per interagire con i frammenti di contenuto e AEM per eseguire operazioni aziendali personalizzate.</p>
                    <a href="./runtime-action.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aggiungi un'azione Adobe I/O Runtime</span>
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
                    <p class="headline is-size-5 has-text-weight-bold">7. Test</p>
                    <p class="is-size-6">Verifica le estensioni durante lo sviluppo e condividi le estensioni completate con tester QA o UAT utilizzando un URL speciale.</p>
                    <a href="./test.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Test dell'estensione</span>
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
                    <a href="./deploy.md" title="Distribuzione delle estensioni" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/deploy/card.png" alt="Distribuzione delle estensioni">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">8. Distribuzione di produzione</p>
                    <p class="is-size-6">Distribuisci l'estensione ad Adobe I/O rendendola disponibile agli utenti AEM. Le estensioni possono essere aggiornate e rimosse.</p>
                    <a href="./deploy.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Distribuzione in produzione</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
</div>

## Estensioni di esempio

Esempio AEM estensioni della console Frammenti di contenuto .

<div class="columns is-multiline">
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/bulk-property-update.md" title="Estensione aggiornamento proprietà bulk" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/bulk-property-update/card.png" alt="Estensione aggiornamento proprietà bulk">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Estensione aggiornamento proprietà bulk</p>
                    <p class="is-size-6">Esplora un'estensione della barra delle azioni di esempio che aggiorna in blocco una proprietà sui frammenti di contenuto selezionati.</p>
                    <a href="./example-extensions/bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Esplorare l’estensione di esempio</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Image generation and upload to AEM extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/image-generation-and-image-upload.md" title="Generazione e caricamento delle immagini nell'estensione AEM" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/digital-image-generation/screenshot.png" alt="Generazione e caricamento delle immagini nell'estensione AEM">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Generazione e caricamento delle immagini nell'estensione AEM</p>
                    <p class="is-size-6">Esplora un'estensione della barra delle azioni di esempio che genera un'immagine utilizzando OpenAI, la carica in AEM e aggiorna la proprietà dell'immagine sul frammento di contenuto selezionato.</p>
                    <a href="./example-extensions/image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Esplorare l’estensione di esempio</span>
                    </a>
                </div>
            </div>
        </div>
    </div>



</div>
