---
title: Capitolo 3 - Creazione di frammenti di contenuto evento
seo-title: Guida introduttiva a AEM Content Services - Capitolo 3 - Creazione di frammenti di contenuto evento
description: Il capitolo 3 dell’esercitazione AEM headless descrive la creazione e la creazione di frammenti di contenuto evento dal modello di frammento di contenuto creato nel capitolo 2.
seo-description: Il capitolo 3 dell’esercitazione AEM headless descrive la creazione e la creazione di frammenti di contenuto evento dal modello di frammento di contenuto creato nel capitolo 2.
translation-type: tm+mt
source-git-commit: d7258f8acf6df680795ce61cc8383e60b5b7d722
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 1%

---


# Capitolo 3 - Creazione di frammenti di contenuto evento

Il capitolo 3 dell&#39;esercitazione AEM Headless descrive la creazione e la creazione di frammenti di contenuto di eventi dal modello di frammento di contenuto creato in [Capitolo 2](./chapter-2.md).

## Creazione di un frammento di contenuto evento

Con un modello di frammento di contenuto [!DNL Event] creato e la configurazione AEM per WKND applicata alla cartella `/content/dam/wknd-mobile` delle risorse (tramite la proprietà `cq:conf`), è possibile creare un frammento di contenuto [!DNL Event].

I frammenti di contenuto, che sono un tipo di risorsa, devono essere organizzati e gestiti in  AEM Assets come altre risorse.

* Usa le cartelle delle impostazioni internazionali nella struttura delle cartelle Risorse se la traduzione è (o potrebbe essere) richiesta
* Organizzare in modo logico i frammenti di contenuto in modo che siano facili da individuare e gestire

In questo passaggio, create un nuovo [!DNL Event] per `Punkrock Fest` nella cartella `/content/dam/wknd-mobile/en/events` delle risorse.

1. Andate a **[!UICONTROL AEM] > [!UICONTROL Risorse] > [!UICONTROL File] > [!DNL WKND Mobile] >[!DNL English]** e create le cartelle delle risorse **[!DNL Events]**.
1. In **[!UICONTROL Risorse] > [!UICONTROL File] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** create un nuovo frammento di contenuto di tipo **[!DNL Event]** con un titolo **[!DNL Punkrock Fest]**.
1. Creare il frammento di contenuto appena creato [!DNL Event].

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] :  **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description=&quot;&quot;>**
   * [!DNL Event Date] :  **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] :  **Musica**
   * [!DNL Ticket Price] :  **10**
   * [!DNL Event Image] :  **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] :  **La casa dei rettili**
   * [!DNL Venue City] : **New York**

   Toccate **[!UICONTROL Salva]** nella barra delle azioni superiore per salvare le modifiche.

1. Utilizzando [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp), installate il pacchetto riportato di seguito in AEM Author. Questo pacchetto contiene una serie di frammenti di contenuto evento.

   [Ottieni file: GitHub > Risorse > com.adobe.aem.guide.wknd-mobile.content.Chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## Revisione della struttura JCR del frammento di contenuto

*Questa sezione è solo informativa e ha lo scopo di socializzare la struttura JCR sottostante dei frammenti di contenuto creati dai modelli di frammenti di contenuto.*

1. Aprite **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** su AEM Author.
1. In CRXDE Lite, nel menu della gerarchia a sinistra, andate a [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) che è il nodo che rappresenta il frammento di contenuto [!DNL Punkrock Fest] [!DNL Event] nel JCR.
1. Espandere il nodo [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master).
Verificare nel **riquadro Proprietà** che la proprietà sia `cq:model` e che punti alla definizione del [!DNL Event] modello di frammento di contenuto.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Sotto il nodo `data` selezionare il nodo [master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) e rivedere le proprietà. Questo nodo contiene il contenuto raccolto durante la creazione di un modello di frammento di contenuto [!DNL Event]. I nomi delle proprietà JCR corrispondono ai nomi delle proprietà del modello di frammento di contenuto e i valori corrispondono ai valori creati del frammento di contenuto &quot;[!DNL Punkrock Fest]&quot; [!DNL Event].

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## Passaggio successivo

È consigliabile installare il pacchetto di contenuti [com.adobe.aem.guide.wknd-mobile.content.Chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) su AEM Author tramite [AEM [!UICONTROL Gestione pacchetti]](http://localhost:4502/crx/packmgr/index.jsp). Questo pacchetto contiene le configurazioni e il contenuto descritti in questo e nei precedenti capitoli dell&#39;esercitazione.

* [Capitolo 4 - Definizione dei modelli di AEM Content Services](./chapter-4.md)
