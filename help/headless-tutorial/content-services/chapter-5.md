---
title: Capitolo 5 - Authoring delle pagine dei servizi di contenuti - Content Services
description: Il capitolo 5 dell’esercitazione AEM Headless copre la creazione delle pagine dai modelli definiti nel capitolo 4. Queste pagine fungeranno da endpoint HTTP JSON.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---

# Capitolo 5 - Authoring delle pagine dei servizi per i contenuti

Il capitolo 5 dell’esercitazione AEM Headless copre la creazione della pagina dai modelli definiti nel capitolo 4. La pagina creata in questo capitolo fungerà da punto finale HTTP JSON per l’app mobile.

>[!NOTE]
>
> Architettura del contenuto della pagina di `/content/wknd-mobile/en/api` è stato precostruito. Le pagine di base di `en` e `api` essere orientati ad uno scopo architettonico e organizzativo, ma non sono strettamente necessari. Se il contenuto API può essere localizzato, è consigliabile seguire le consuete best practice per l’organizzazione delle pagine Copia lingua e Gestione multisito , in quanto la pagina API può essere localizzata come qualsiasi pagina di AEM Sites.

## Creazione della pagina API evento

1. Passa a **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Tocca l’etichetta della pagina API**, quindi tocca **Crea** nella barra delle azioni superiore e crea una nuova pagina API Eventi sotto la pagina API.
   1. Tocca **Crea** nella barra delle azioni superiore
   1. Seleziona **API eventi** template
   1. In **Nome** immissione campo **events**
   1. In **Titolo** immissione campo **API eventi**
   1. Tocca **Crea** nella barra delle azioni superiore per creare la pagina
   1. Tocca **Fine** per tornare all’amministratore di AEM Sites

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## Pagina Creazione dell’API degli eventi

>[!NOTE]
>
> Il progetto fornisce CSS per fornire alcuni stili di base per l’esperienza di authoring.

1. Modifica le **API eventi** per passare a **AEM > Siti > WKND Mobile > Inglese > API**, selezionando **API eventi** e tocco **Modifica** nella barra delle azioni superiore.
1. Aggiungi un **immagine logo** per visualizzare l’app, trascinala dal Finder al segnaposto del componente Immagine.
   * Utilizza il logo fornito in `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Aggiungi **riga di tag** per visualizzare al di sopra degli eventi.
   1. Modifica le **Testo** component
   1. Inserisci il testo:
      * `The WKND is here.`

1. Seleziona la **events** per visualizzare:
   1. Imposta la seguente configurazione sul **Proprietà** scheda:
      * Modello: **Evento**
      * Percorso padre: **/content/dam/wknd-mobile/en/events**
      * Tag: **&lt;leave blank=&quot;&quot;>**
   1. Imposta la seguente configurazione sul **Elementi** scheda:
      * Rimuovi gli elementi elencati, per garantire che siano esposti TUTTI gli elementi dei frammenti di contenuto dell’evento.

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## Rivedi l’output JSON della pagina API

L’output JSON e il relativo formato possono essere rivisti richiedendo la pagina con il `.model.json` selettore.

Questa struttura JSON (o schema) deve essere ben compresa dai consumatori di questa API. È fondamentale che i consumatori di API comprendano quali aspetti della struttura sono fissi (ad esempio, Logo (immagine) e tag live (testo) dell’API evento e fluidi (ad esempio gli eventi elencati in Componente elenco frammenti di contenuto).

La risoluzione di questo contratto su un’API pubblicata potrebbe causare un comportamento non corretto nelle applicazioni che consumano contenuti.

1. Nelle nuove schede del browser, richiedi le pagine API degli eventi utilizzando la `.model.json` selettore, che richiama AEM’esportatore JSON di Content Services e serializza la pagina e i componenti in una struttura JSON normalizzata e ben definita.

   La struttura JSON prodotta da queste pagine è la struttura a cui devono essere allineate le app che consumano.

1. Richiedi il **API eventi** come **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   Il risultato dovrebbe essere simile a:

![Output JSON di AEM Content Services](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Questo JSON può essere inviato in un **ordinato** (formattato) moda per la leggibilità umana utilizzando `.tidy` selettore:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Passaggio successivo

Facoltativamente, installa il [com.adobe.aem.guides.wknd-mobile.content.capitolo-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) pacchetto di contenuti su AEM Author tramite [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp). Questo pacchetto contiene le configurazioni e il contenuto descritti in questo e nei precedenti capitoli dell&#39;esercitazione.

* [Capitolo 6 - Esposizione dei contenuti su AEM Publish as JSON](./chapter-6.md)
