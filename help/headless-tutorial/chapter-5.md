---
title: Capitolo 5 - Authoring delle pagine di Content Services
description: Il capitolo 5 dell’esercitazione AEM Headless copre la creazione delle pagine dai modelli definiti nel capitolo 4. Queste pagine fungeranno da endpoint HTTP JSON.
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '594'
ht-degree: 0%

---


# Capitolo 5 - Authoring delle pagine di Content Services

Il capitolo 5 dell’esercitazione AEM Headless copre la creazione della pagina dai modelli definiti nel capitolo 4. La pagina creata in questo capitolo fungerà da punto finale JSON HTTP per l’app mobile.

>[!NOTE]
>
> L&#39;architettura del contenuto delle pagine di `/content/wknd-mobile/en/api` è stata pregenerata. Le pagine di base di `en` e `api` hanno uno scopo architettonico e organizzativo, ma non sono strettamente necessarie. Se il contenuto API può essere localizzato, è consigliabile seguire le consuete procedure ottimali per l&#39;organizzazione delle pagine Copia lingua e Gestione multisito, in quanto la pagina API può essere localizzata come qualsiasi  pagina AEM Sites.

## Creazione della pagina API evento

1. Andate a **[!UICONTROL AEM] > [!UICONTROL Siti] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Toccate l&#39;etichetta della pagina** API, quindi toccate il pulsante  **** Createbon nella barra delle azioni superiore e create una nuova pagina API per gli eventi nella pagina API.
   1. Toccate **Crea** nella barra delle azioni superiore
   1. Selezionare il modello **Events API**
   1. Nel campo **Name** immettere **events**
   1. Nel campo **Title** immettere **Events API**
   1. Toccate **Crea** nella barra delle azioni superiore per creare la pagina
   1. Toccate **Fine** per tornare all&#39;amministratore  AEM Sites

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## Authoring della pagina API Events

>[!NOTE]
>
> Il progetto fornisce CSS per fornire alcuni stili di base per l&#39;esperienza di authoring.

1. Modificate la pagina **Events API** andando a **AEM > Sites > WKND Mobile > English > API**, selezionando la pagina **Events API** e toccando **Edit** nella barra delle azioni superiore.
1. Aggiungete un&#39;immagine **logo** da visualizzare nell&#39;app trascinandola da Asset Finder al segnaposto del componente Immagine.
   * Utilizzate il logo fornito in `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Aggiungete **tag line** per visualizzare sopra gli eventi.
   1. Modificare il componente **Testo**
   1. Inserite il testo:
      * `The WKND is here.`

1. Selezionare **events** per visualizzare:
   1. Impostate la seguente configurazione nella scheda **Proprietà**:
      * Modello: **Evento**
      * Percorso padre: **/content/dam/wknd-mobile/en/events**
      * Tag: **&lt;Lascia vuoto>**
   1. Impostate la seguente configurazione nella scheda **Elements**:
      * Rimuovete tutti gli elementi elencati, per garantire che tutti gli elementi dei frammenti di contenuto dell&#39;evento siano esposti.

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## Rivedete l&#39;output JSON della pagina API

L&#39;output JSON e il relativo formato possono essere rivisti richiedendo la pagina con il selettore `.model.json`.

Questa struttura (o schema) JSON deve essere ben compresa dai consumatori di questa API. È fondamentale che i consumatori di API capiscano quali aspetti della struttura sono fissi (ad esempio. il logo dell&#39;API dell&#39;evento (Immagine) e i tag live (Testo) e che sono fluidi (es. gli eventi elencati in Elenco frammenti di contenuto).

La risoluzione di questo contratto su un&#39;API pubblicata potrebbe causare un comportamento non corretto nelle app utilizzate.

1. Nelle nuove schede del browser, richiedete le pagine dell&#39;API Events utilizzando il selettore `.model.json`, che richiama AEM Content Services&#39;JSON Exporter, e serializza la pagina e i componenti in una struttura JSON normalizzata e ben definita.

   La struttura JSON prodotta da queste pagine è la struttura a cui devono essere allineate le app che consumano.

1. Richiedete la pagina **Events API** come **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   Il risultato dovrebbe essere simile a:

![Output JSON di AEM Content Services](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Questo JSON può essere emesso in modo **ordinato** (formattato) per una leggibilità umana utilizzando il selettore `.tidy`:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Passaggio successivo

Se necessario, installate il pacchetto di contenuti [com.adobe.aem.guide.wknd-mobile.content.Chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) su AEM Author tramite [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). Questo pacchetto contiene le configurazioni e il contenuto descritti in questo e nei precedenti capitoli dell&#39;esercitazione.

* [Capitolo 6 - Esposizione dei contenuti su AEM Publish come JSON](./chapter-6.md)
