---
title: Capitolo 5 - Authoring delle pagine dei servizi di contenuti - Content Services
description: Il capitolo 5 dell’esercitazione AEM Headless copre la creazione di pagine dai modelli definiti nel capitolo 4. Queste pagine fungeranno da endpoint HTTP JSON.
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---


# Capitolo 5 - Authoring delle pagine dei servizi per i contenuti

Il capitolo 5 dell’esercitazione AEM Headless copre la creazione di una pagina dai modelli definiti nel capitolo 4. La pagina creata in questo capitolo fungerà da punto finale HTTP JSON per l’app mobile.

>[!NOTE]
>
> L’architettura del contenuto della pagina di `/content/wknd-mobile/en/api` è stata pregenerata. Le pagine di base di `en` e `api` hanno uno scopo architettonico e organizzativo, ma non sono strettamente necessarie. Se il contenuto API può essere localizzato, è consigliabile seguire le consuete best practice per l’organizzazione delle pagine Copia lingua e Gestione multisito , in quanto la pagina API può essere localizzata come qualsiasi pagina di AEM Sites.

## Creazione della pagina API evento

1. Passa a **[!UICONTROL AEM] > [!UICONTROL Siti] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Tocca l’etichetta della pagina** API, quindi tocca il pulsante  **** Crea nella barra delle azioni superiore e crea una nuova pagina API Eventi nella pagina API.
   1. Tocca **Crea** nella barra delle azioni superiore
   1. Seleziona il modello **API eventi**
   1. Nel campo **Nome** immetti **eventi**
   1. Nel campo **Titolo** immetti **API eventi**
   1. Tocca **Crea** nella barra delle azioni superiore per creare la pagina
   1. Tocca **Fine** per tornare all’amministratore di AEM Sites

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## Pagina Creazione dell’API degli eventi

>[!NOTE]
>
> Il progetto fornisce CSS per fornire alcuni stili di base per l’esperienza di authoring.

1. Modifica la pagina **API eventi** passando a **AEM > Sites > WKND Mobile > Inglese > API**, seleziona la pagina **API eventi** e tocca **Modifica** nella barra delle azioni superiore.
1. Aggiungi un’ **immagine logo** per visualizzarla nell’app trascinandola dal Finder sul segnaposto del componente Immagine.
   * Utilizza il logo fornito in `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Aggiungi **riga tag** per visualizzare sopra gli eventi.
   1. Modifica il componente **Testo**
   1. Inserisci il testo:
      * `The WKND is here.`

1. Seleziona **eventi** da visualizzare:
   1. Imposta la seguente configurazione nella scheda **Proprietà** :
      * Modello: **Evento**
      * Percorso padre: **/content/dam/wknd-mobile/en/events**
      * Tag: **&lt;Lascia vuoto>**
   1. Imposta la seguente configurazione nella scheda **Elementi** :
      * Rimuovi gli elementi elencati, per garantire che siano esposti TUTTI gli elementi dei frammenti di contenuto dell’evento.

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## Rivedi l’output JSON della pagina API

L’output JSON e il relativo formato possono essere rivisti richiedendo la pagina con il selettore `.model.json` .

Questa struttura JSON (o schema) deve essere ben compresa dai consumatori di questa API. È fondamentale che i consumatori di API comprendano quali aspetti della struttura sono fissi (ad esempio, Logo (immagine) e tag live (testo) dell’API evento e fluidi (ad esempio gli eventi elencati in Componente elenco frammenti di contenuto).

La risoluzione di questo contratto su un’API pubblicata potrebbe causare un comportamento non corretto nelle applicazioni che consumano contenuti.

1. Nelle nuove schede del browser, richiedi le pagine API degli eventi utilizzando il selettore `.model.json`, che richiama l’esportatore JSON di AEM Content Services e serializza la pagina e i componenti in una struttura JSON normalizzata e ben definita.

   La struttura JSON prodotta da queste pagine è la struttura a cui devono essere allineate le app che consumano.

1. Richiedi la pagina **API Eventi** come **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   Il risultato dovrebbe essere simile a:

![Output JSON di AEM Content Services](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Questo JSON può essere emesso in modo **ordinato** (formattato) per una leggibilità umana utilizzando il selettore `.tidy`:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Passaggio successivo

Facoltativamente, installa il pacchetto di contenuti [com.adobe.aem.guides.wknd-mobile.content.capitolo-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) su AEM Author tramite [Gestione pacchetti di AEM](http://localhost:4502/crx/packmgr/index.jsp). Questo pacchetto contiene le configurazioni e il contenuto descritti in questo e nei precedenti capitoli dell&#39;esercitazione.

* [Capitolo 6 - Esposizione dei contenuti su AEM Publish as JSON](./chapter-6.md)
