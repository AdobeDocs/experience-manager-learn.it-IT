---
title: Capitolo 3 - Authoring di frammenti di contenuto di eventi - Content Services
seo-title: Getting Started with AEM Content Services - Chapter 3 - Authoring Event Content Fragments
description: Il capitolo 3 dell’esercitazione AEM headless descrive la creazione e la creazione di frammenti di contenuto evento dal modello di frammento di contenuto creato nel capitolo 2.
seo-description: Chapter 3 of the AEM Headless tutorial covers creating and authoring Event Content Fragments from the Content Fragment Model created in Chapter 2.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 3%

---

# Capitolo 3 - Creazione di frammenti di contenuto evento

Il capitolo 3 dell’esercitazione AEM Headless riguarda la creazione e la creazione di frammenti di contenuto di eventi dal modello di frammento di contenuto creato in [Capitolo 2](./chapter-2.md).

## Creazione di un frammento di contenuto evento

Con un [!DNL Event] Modello a frammento di contenuto creato e la configurazione AEM per WKND applicata al `/content/dam/wknd-mobile` Cartella risorse (tramite `cq:conf` proprietà), a [!DNL Event] È possibile creare un frammento di contenuto.

I frammenti di contenuto, che sono un tipo di risorsa, devono essere organizzati e gestiti in AEM Assets come altre risorse.

* Utilizza le cartelle internazionali nella struttura delle cartelle Assets se la traduzione è (o potrebbe essere) necessaria
* Organizza in modo logico i frammenti di contenuto in modo che siano facili da individuare e gestire

In questo passaggio, crea un nuovo [!DNL Event] per `Punkrock Fest` in `/content/dam/wknd-mobile/en/events` cartella risorse.

1. Passa a **[!UICONTROL AEM] > [!UICONTROL Risorse] > [!UICONTROL File] > [!DNL WKND Mobile] >[!DNL English]** e creare cartelle di risorse **[!DNL Events]**.
1. Within **[!UICONTROL Risorse] > [!UICONTROL File] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** creare un nuovo frammento di contenuto di tipo **[!DNL Event]** con un titolo **[!DNL Punkrock Fest]**.
1. Crea il nuovo [!DNL Event] Frammento di contenuto.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description...=&quot;&quot;>**
   * [!DNL Event Date] : **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **Musica**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **La casa dei rettili**
   * [!DNL Venue City] : **New York**

   Tocca **[!UICONTROL Salva]** nella barra delle azioni superiore per salvare le modifiche.

1. Utilizzo [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp), installa il pacchetto sottostante su AEM Author. Questo pacchetto contiene una serie di frammenti di contenuto evento.

   [Ottieni file: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.content.capitolo-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## Revisione della struttura JCR del frammento di contenuto

*Questa sezione è solo informativo e ha lo scopo di socializzare la struttura JCR sottostante dei frammenti di contenuto creati dai modelli di frammenti di contenuto.*

1. Apri **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** su AEM Author.
1. In CRXDE Lite, nel menu della gerarchia a sinistra, passare a [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) che rappresenta il nodo [!DNL Punkrock Fest] [!DNL Event] Frammento di contenuto nel JCR.
1. Espandi la [dati](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) nodo.
Consulta la sezione **Riquadro delle proprietà** che dispone di una proprietà `cq:model` che indica [!DNL Event] Definizione del modello di frammento di contenuto.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Sotto `data` seleziona il nodo [maestro](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) e controlla le proprietà. Questo nodo contiene il contenuto raccolto durante la creazione di un [!DNL Event] Modello per frammento di contenuto. I nomi delle proprietà JCR corrispondono ai nomi delle proprietà del modello di frammento di contenuto e i valori corrispondono ai valori creati del &quot;[!DNL Punkrock Fest]&quot; [!DNL Event] Frammento di contenuto.

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## Passaggio successivo

Si consiglia di installare la [com.adobe.aem.guides.wknd-mobile.content.capitolo-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) pacchetto di contenuti su AEM Author tramite [AEM [!UICONTROL Gestione pacchetti]](http://localhost:4502/crx/packmgr/index.jsp). Questo pacchetto contiene le configurazioni e il contenuto descritti in questo e nei precedenti capitoli dell&#39;esercitazione.

* [Capitolo 4 - Definizione dei modelli di servizi di contenuto AEM](./chapter-4.md)
