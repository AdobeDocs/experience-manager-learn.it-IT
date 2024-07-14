---
title: Capitolo 3 - Authoring di frammenti di contenuto di eventi - Content Services
description: Il capitolo 3 del tutorial AEM headless tratta la creazione e l’authoring di frammenti di contenuto di eventi dal modello per frammenti di contenuto creato nel capitolo 2.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
duration: 167
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 1%

---

# Capitolo 3 - Creazione di frammenti di contenuto di eventi

Il capitolo 3 del tutorial AEM Headless tratta la creazione e l’authoring di eventi e frammenti di contenuto dal modello per frammenti di contenuto creato nel [capitolo 2](./chapter-2.md).

## Creazione di un frammento di contenuto evento

Con un modello per frammenti di contenuto [!DNL Event] creato e la configurazione AEM per WKND applicata alla cartella di risorse `/content/dam/wknd-mobile` (tramite la proprietà `cq:conf`), è possibile creare un frammento di contenuto [!DNL Event].

I frammenti di contenuto, che sono un tipo di risorsa, devono essere organizzati e gestiti in AEM Assets come le altre risorse.

* Utilizza le cartelle locali nella struttura di cartelle di Assets se la traduzione è (o potrebbe essere) necessaria
* Organizzare in modo logico i frammenti di contenuto per facilitarne l’individuazione e la gestione

In questo passaggio, creare un nuovo [!DNL Event] per `Punkrock Fest` nella cartella delle risorse `/content/dam/wknd-mobile/en/events`.

1. Passa a **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL File] > [!DNL WKND Mobile] >[!DNL English]** e crea cartelle risorse **[!DNL Events]**.
1. In **[!UICONTROL Assets] > [!UICONTROL File] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** creare un nuovo frammento di contenuto di tipo **[!DNL Event]** con titolo **[!DNL Punkrock Fest]**.
1. Creare il frammento di contenuto [!DNL Event] appena creato.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;Immettere alcune righe di descrizione...>**
   * [!DNL Event Date] : **&lt;Seleziona una data futura>**
   * [!DNL Event Type] : **Musica**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **La casa dei rettili**
   * [!DNL Venue City] : **New York**

   Tocca **[!UICONTROL Salva]** nella barra delle azioni superiore per salvare le modifiche.

1. Utilizzando [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp), installa il pacchetto seguente in AEM Author. Questo pacchetto contiene diversi frammenti di contenuto evento.

   [Ottieni file: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## Revisione della struttura JCR del frammento di contenuto

*Questa sezione è puramente informativa e ha lo scopo di socializzare la struttura JCR sottostante dei frammenti di contenuto creati da modelli per frammenti di contenuto.*

1. Apri **[CRXDE Liti](http://localhost:4502/crx/de/index.jsp)** nell&#39;istanza Autore AEM.
1. In CRXDE Lite, nel menu della gerarchia a sinistra, passa a [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content), che è il nodo che rappresenta il frammento di contenuto [!DNL Punkrock Fest] [!DNL Event] nel JCR.
1. Espandere il nodo [dati](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master).
Verificare nel riquadro **Proprietà** che la proprietà `cq:model` punti alla definizione del modello per frammenti di contenuto [!DNL Event].
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Sotto il nodo `data` selezionare il nodo [master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) e rivedere le proprietà. Questo nodo contiene il contenuto raccolto durante la creazione di un modello per frammenti di contenuto [!DNL Event]. I nomi delle proprietà JCR corrispondono a quelli della proprietà Modello per frammenti di contenuto e i valori corrispondono ai valori creati del frammento di contenuto [!DNL Event] &quot;[!DNL Punkrock Fest]&quot;.

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## Passaggio successivo

Si consiglia di installare il pacchetto di contenuti [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) in AEM Author tramite [AEM con [!UICONTROL Gestione pacchetti]](http://localhost:4502/crx/packmgr/index.jsp). Questo pacchetto contiene le configurazioni e il contenuto descritti in questo e nei capitoli precedenti dell’esercitazione.

* [Capitolo 4 - Definizione dei modelli di Content Services per l’AEM](./chapter-4.md)
