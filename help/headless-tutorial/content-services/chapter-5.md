---
title: Capitolo 5 - Authoring delle pagine di Content Services - Content Services
description: Il capitolo 5 del tutorial AEM headless tratta la creazione delle pagine dai modelli definiti nel capitolo 4. Queste pagine fungeranno da endpoint HTTP JSON.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
duration: 189
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '570'
ht-degree: 0%

---

# Capitolo 5 - Authoring delle pagine di Content Services

Il capitolo 5 del tutorial AEM headless tratta la creazione della pagina dai modelli definiti nel capitolo 4. La pagina creata in questo capitolo fungerà da endpoint HTTP JSON per l’app mobile.

>[!NOTE]
>
> L&#39;architettura del contenuto della pagina di `/content/wknd-mobile/en/api` è stata predefinita. Le pagine di base di `en` e `api` hanno uno scopo architettonico e organizzativo, ma non sono strettamente necessarie. Se il contenuto API può essere localizzato, è consigliabile seguire le usuali best practice di organizzazione delle pagine Copia in lingua e Gestore multisito, in quanto la pagina API può essere localizzata come qualsiasi pagina di AEM Sites.

## Creazione della pagina API dell’evento

1. Passa a **[!UICONTROL AEM] > [!UICONTROL Siti] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Tocca l&#39;etichetta della pagina API**, quindi tocca il pulsante **Crea** nella barra delle azioni superiore e crea una nuova pagina API degli eventi nella pagina API.
   1. Tocca **Crea** nella barra delle azioni superiore
   1. Seleziona modello **Eventi API**
   1. Nel campo **Nome** immetti **eventi**
   1. Nel campo **Titolo** immetti **API eventi**
   1. Tocca **Crea** nella barra delle azioni superiore per creare la pagina
   1. Tocca **Fine** per tornare all&#39;amministratore AEM Sites

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## Creazione della pagina API degli eventi

>[!NOTE]
>
> Il progetto fornisce CSS per fornire alcuni stili di base per l’esperienza di authoring.

1. Modificare la pagina **Events API** passando a **AEM > Sites > WKND Mobile > English > API**, selezionando la pagina **Events API** e toccando **Edit** nella barra delle azioni superiore.
1. Aggiungi un&#39;immagine **logo** da visualizzare nell&#39;app trascinandola da Asset Finder al segnaposto del componente Immagine.
   * Utilizza il logo fornito trovato in `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Aggiungi **riga di tag** da visualizzare sopra gli eventi.
   1. Modifica il componente **Testo**
   1. Immettere il testo:
      * `The WKND is here.`

1. Seleziona **eventi** da visualizzare:
   1. Imposta la seguente configurazione nella scheda **Proprietà**:
      * Modello: **Evento**
      * Percorso principale: **/content/dam/wknd-mobile/en/events**
      * Tag: **&lt;Lascia vuoto>**
   1. Imposta la seguente configurazione nella scheda **Elementi**:
      * Rimuovi eventuali elementi elencati per garantire che SIANO ESPOSTI TUTTI gli elementi dei Frammenti di contenuto dell’evento.

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## Rivedi l’output JSON della pagina API

L&#39;output JSON e il relativo formato possono essere rivisti richiedendo la pagina con il selettore `.model.json`.

Questa struttura JSON (o schema) deve essere ben compresa dai consumatori di questa API. È fondamentale che i consumatori di API comprendano quali aspetti della struttura siano fissi (ossia. il logo (immagine) e il tag live (testo) dell’API degli eventi e che sono fluidi (ad esempio gli eventi elencati in Componente Elenco frammenti di contenuto).

L’interruzione di questo contratto su un’API pubblicata può causare un comportamento errato nell’utilizzo delle app.

1. Nelle nuove schede del browser, richiedi le pagine API degli eventi utilizzando il selettore `.model.json`, che richiama l’esportazione JSON di AEM Content Services e serializza la pagina e i componenti in una struttura JSON normalizzata e ben definita.

   La struttura JSON prodotta da queste pagine è quella a cui le app che utilizzano la struttura devono allinearsi.

1. Richiedi la pagina **Events API** come **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   Il risultato dovrebbe essere simile al seguente:

![Output JSON AEM Content Services](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Questo JSON può essere emesso in formato **tido** per la leggibilità umana utilizzando il selettore `.tidy`:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

## Passaggio successivo

Se necessario, installa il pacchetto di contenuti [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) su AEM Author tramite [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp). Questo pacchetto contiene le configurazioni e il contenuto descritti in questo e nei capitoli precedenti dell’esercitazione.

* [Capitolo 6 - Pubblicazione dei contenuti su AEM Publish come JSON](./chapter-6.md)
