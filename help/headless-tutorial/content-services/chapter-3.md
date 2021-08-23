---
title: Capitolo 3 - Authoring di frammenti di contenuto di eventi - Content Services
seo-title: Guida introduttiva a AEM Content Services - Capitolo 3 - Creazione di frammenti di contenuto di eventi
description: Il capitolo 3 dell’esercitazione AEM headless descrive la creazione e la creazione di frammenti di contenuto evento dal modello di frammento di contenuto creato nel capitolo 2.
seo-description: Il capitolo 3 dell’esercitazione AEM headless descrive la creazione e la creazione di frammenti di contenuto evento dal modello di frammento di contenuto creato nel capitolo 2.
feature: Frammenti di contenuto, API
topic: Senza testa, gestione dei contenuti
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 3%

---


# Capitolo 3 - Creazione di frammenti di contenuto evento

Il capitolo 3 dell’esercitazione AEM headless descrive la creazione e la creazione di frammenti di contenuto di eventi dal modello di frammento di contenuto creato in [Capitolo 2](./chapter-2.md).

## Creazione di un frammento di contenuto evento

Con un [!DNL Event] modello di frammento di contenuto creato e la configurazione AEM per WKND applicata alla cartella `/content/dam/wknd-mobile` Risorse (tramite la proprietà `cq:conf` ), è possibile creare un [!DNL Event] frammento di contenuto.

I frammenti di contenuto, che sono un tipo di risorsa, devono essere organizzati e gestiti in AEM Assets come altre risorse.

* Utilizza le cartelle internazionali nella struttura delle cartelle Assets se la traduzione è (o potrebbe essere) necessaria
* Organizza in modo logico i frammenti di contenuto in modo che siano facili da individuare e gestire

In questo passaggio, crea un nuovo [!DNL Event] per `Punkrock Fest` nella cartella delle risorse `/content/dam/wknd-mobile/en/events`.

1. Passa a **[!UICONTROL AEM] > [!UICONTROL Risorse] > [!UICONTROL File] > [!DNL WKND Mobile] >[!DNL English]** e crea cartelle di risorse **[!DNL Events]**.
1. In **[!UICONTROL Risorse] > [!UICONTROL File] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** crea un nuovo frammento di contenuto di tipo **[!DNL Event]** con un titolo **[!DNL Punkrock Fest]**.
1. Crea il frammento di contenuto [!DNL Event] appena creato.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] :  **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description=&quot;&quot;>**
   * [!DNL Event Date] :  **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] :  **Musica**
   * [!DNL Ticket Price] :  **10**
   * [!DNL Event Image] :  **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] :  **La casa dei rettili**
   * [!DNL Venue City] : **New York**

   Tocca **[!UICONTROL Salva]** nella barra delle azioni superiore per salvare le modifiche.

1. Utilizzando [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp), installa il pacchetto sottostante su AEM Author. Questo pacchetto contiene una serie di frammenti di contenuto evento.

   [Ottieni file: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.content.capitolo-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## Revisione della struttura JCR del frammento di contenuto

*Questa sezione è solo informativo e ha lo scopo di socializzare la struttura JCR sottostante dei frammenti di contenuto creati dai modelli di frammenti di contenuto.*

1. Apri **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** su AEM Author.
1. In CRXDE Lite, nel menu della gerarchia a sinistra, passa a [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) che è il nodo che rappresenta il frammento di contenuto [!DNL Punkrock Fest] [!DNL Event] nel JCR.
1. Espandi il nodo [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) .
Rivedi nel **riquadro Proprietà** che dispone di una proprietà `cq:model` che punta alla definizione del modello di frammento di contenuto [!DNL Event].
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Sotto il nodo `data` selezionare il nodo [master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) e rivedere le proprietà. Questo nodo contiene il contenuto raccolto durante la creazione di un modello di frammento di contenuto [!DNL Event]. I nomi delle proprietà JCR corrispondono ai nomi delle proprietà del modello di frammento di contenuto e i valori corrispondono ai valori creati del frammento di contenuto &quot;[!DNL Punkrock Fest]&quot; [!DNL Event].

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## Passaggio successivo

Si consiglia di installare il pacchetto di contenuti [com.adobe.aem.guides.wknd-mobile.content.capitolo-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) su AEM Author tramite [AEM [!UICONTROL Gestione pacchetti]](http://localhost:4502/crx/packmgr/index.jsp). Questo pacchetto contiene le configurazioni e il contenuto descritti in questo e nei precedenti capitoli dell&#39;esercitazione.

* [Capitolo 4 - Definizione dei modelli di servizi di contenuto AEM](./chapter-4.md)
