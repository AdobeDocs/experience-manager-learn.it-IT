---
title: Capitolo 3 - Authoring di frammenti di contenuto di eventi - Content Services
description: Il capitolo 3 del tutorial AEM headless tratta la creazione e l’authoring di frammenti di contenuto di eventi dal modello per frammenti di contenuto creato nel capitolo 2.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
duration: 205
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---

# Capitolo 3 - Creazione di frammenti di contenuto di eventi

Il capitolo 3 del tutorial AEM Headless tratta la creazione e l’authoring di eventi e frammenti di contenuto dal modello per frammenti di contenuto creato in [Capitolo 2](./chapter-2.md).

## Creazione di un frammento di contenuto evento

Con un [!DNL Event] Modello per frammenti di contenuto creato e configurazione AEM per WKND applicata al `/content/dam/wknd-mobile` Cartella risorse (tramite `cq:conf` proprietà), a [!DNL Event] È possibile creare un frammento di contenuto.

I frammenti di contenuto, che sono un tipo di risorsa, devono essere organizzati e gestiti in AEM Assets come le altre risorse.

* Utilizza le cartelle locali nella struttura di cartelle Risorse se la traduzione è (o potrebbe essere) necessaria
* Organizzare in modo logico i frammenti di contenuto per facilitarne l’individuazione e la gestione

In questo passaggio, crea un nuovo [!DNL Event] per `Punkrock Fest` nel `/content/dam/wknd-mobile/en/events` cartella delle risorse.

1. Accedi a **[!UICONTROL AEM] > [!UICONTROL Risorse] > [!UICONTROL File] > [!DNL WKND Mobile] >[!DNL English]** e creare cartelle di risorse **[!DNL Events]**.
1. Entro **[!UICONTROL Risorse] > [!UICONTROL File] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** creare un nuovo frammento di contenuto di tipo **[!DNL Event]** con titolo **[!DNL Punkrock Fest]**.
1. Crea il nuovo elemento creato [!DNL Event] Frammento di contenuto.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description...=&quot;&quot;>**
   * [!DNL Event Date] : **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **Musica**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **La casa dei rettili**
   * [!DNL Venue City] : **New York**

   Tocca **[!UICONTROL Salva]** nella barra delle azioni superiore per salvare le modifiche.

1. Utilizzo di [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp), installa il pacchetto seguente su AEM Author. Questo pacchetto contiene diversi frammenti di contenuto evento.

   [Ottieni file: GitHub > Risorse > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## Revisione della struttura JCR del frammento di contenuto

*Questa sezione è solo informativa e ha lo scopo di socializzare la struttura JCR sottostante dei frammenti di contenuto creati da modelli per frammenti di contenuto.*

1. Apri **[CRXDE Liti](http://localhost:4502/crx/de/index.jsp)** su Autore AEM.
1. In CRXDE Liti, nel menu della gerarchia a sinistra, passare a [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) che è il nodo che rappresenta [!DNL Punkrock Fest] [!DNL Event] Frammento di contenuto in JCR.
1. Espandi [dati](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) nodo.
Revisione in **Riquadro delle proprietà** che ha una proprietà `cq:model` che fa riferimento al [!DNL Event] Definizione del modello per frammenti di contenuto.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Sotto `data` nodo seleziona il [principale](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) e rivedere le proprietà. Questo nodo contiene il contenuto raccolto durante l’authoring di un [!DNL Event] Modello per frammenti di contenuto. I nomi delle proprietà JCR corrispondono a quelli della proprietà Modello per frammenti di contenuto e i valori corrispondono ai valori creati della &quot;[!DNL Punkrock Fest]&quot; [!DNL Event] Frammento di contenuto.

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## Passaggio successivo

Si consiglia di installare [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) pacchetto di contenuti su AEM Author tramite [AEM [!UICONTROL Gestione pacchetti]](http://localhost:4502/crx/packmgr/index.jsp). Questo pacchetto contiene le configurazioni e il contenuto descritti in questo e nei capitoli precedenti dell’esercitazione.

* [Capitolo 4 - Definizione dei modelli di Content Services per l’AEM](./chapter-4.md)
