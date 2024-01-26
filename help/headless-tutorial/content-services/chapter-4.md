---
title: Capitolo 4 - Definizione dei modelli di Content Services - Content Services
description: Il capitolo 4 del tutorial AEM Headless tratta il ruolo dei modelli AEM modificabili nel contesto di AEM Content Services. I modelli modificabili vengono utilizzati per definire la struttura del contenuto JSON che AEM Content Services espone in ultima analisi.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
duration: 293
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 0%

---

# Capitolo 4 - Definizione dei modelli di Content Services

Il capitolo 4 del tutorial AEM Headless tratta il ruolo dei modelli AEM modificabili nel contesto di AEM Content Services. I modelli modificabili vengono utilizzati per definire la struttura del contenuto JSON che AEM Content Services espone ai clienti tramite la composizione dei componenti AEM abilitati per Content Services.

## Informazioni sul ruolo dei modelli in AEM Content Services

I modelli modificabili dell’AEM vengono utilizzati per definire gli endpoint HTTP a cui si accede per esporre il contenuto dell’evento come JSON.

In genere i modelli modificabili dell’AEM vengono utilizzati per definire le pagine web, ma questo utilizzo è semplicemente una convenzione. I modelli modificabili possono essere utilizzati per comporre **qualsiasi** set di contenuti; modalità di accesso al contenuto: come HTML in un browser, come JSON utilizzato da JavaScript (AEM SPA Editor) o un’app mobile è una funzione del modo in cui viene richiesta la pagina.

In AEM Content Services, i modelli modificabili vengono utilizzati per definire la modalità di esposizione dei dati JSON.

Per [!DNL WKND Mobile] app, creeremo un singolo modello modificabile che viene utilizzato per guidare un singolo endpoint API. Anche se questo esempio è semplice per illustrare i concetti di headless AEM, puoi creare più pagine (o endpoint) ciascuna delle quali esporre diversi set di contenuti per creare un’API più complessa e meglio organizzata.

## Informazioni sull’endpoint API

Per capire come comporre il nostro endpoint API e capire quali contenuti dovrebbero essere esposti al nostro [!DNL WKND Mobile] App, rivediamo il design.

![Scomposizione pagina API eventi](./assets/chapter-4/design-to-component-mapping.png)

Come possiamo vedere, abbiamo tre set logici di contenuti da fornire all’app mobile.

1. Il **Logo**
2. Il **Linea tag**
3. L’elenco di **Eventi**

A tal fine, possiamo mappare questi requisiti sui Componenti AEM (e, nel nostro caso, sui Componenti core WCM dell’AEM) per esporre il contenuto richiesto come JSON.

1. Il **Logo** viene riprodotto tramite una **Componente immagine**
2. Il **Linea tag** viene riprodotto tramite una **Componente testo**
3. L’elenco di **Eventi** viene riprodotto tramite una **Componente Elenco frammenti di contenuto** che a sua volta fa riferimento a un insieme di Frammenti di contenuto evento.

>[!NOTE]
>
>Per supportare l’esportazione JSON delle pagine e dei componenti da parte del servizio di contenuti AEM, le pagine e i componenti devono: **derivare dai componenti core WCM dell’AEM**.
>
>[Componenti core WCM AEM](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) dispongono di funzionalità integrate per supportare uno schema JSON normalizzato di pagine e componenti esportati. Tutti i componenti mobili WKND utilizzati in questa esercitazione (Pagina, Immagine, Testo e Elenco frammenti di contenuto) sono derivati dai componenti core WCM dell’AEM.

## Definizione del modello API per gli eventi

1. Accedi a **[!UICONTROL Strumenti] > [!UICONTROL Generale] > [!UICONTROL Modelli] >[!DNL WKND Mobile]**.

1. Creare **[!DNL Events API]** modello:

   1. Tocca **[!UICONTROL Crea]** nella barra delle azioni superiore
   1. Seleziona la **[!DNL WKND Mobile - Empty Page]** modello
   1. Tocca **[!UICONTROL Successivo]** nella barra delle azioni superiore
   1. Invio **[!DNL Events API]** nel [!UICONTROL Titolo modello] campo
   1. Tocca **[!UICONTROL Crea]** nella barra delle azioni superiore
   1. Tocca **[!UICONTROL Apri]** apri il nuovo modello per la modifica

1. Innanzitutto, permettiamo ai tre Componenti AEM identificati di modellare il contenuto modificando il [!UICONTROL Policy] della radice [!UICONTROL Contenitore di layout]. Assicurati che **[!UICONTROL Struttura]** è attiva, seleziona la **[!DNL Layout Container \[Root\]]**, e tocca il **[!UICONTROL Policy]** pulsante.
1. Sotto **[!UICONTROL Proprietà] > [!UICONTROL Componenti consentiti]** cerca **[!DNL WKND Mobile]**. Consenti i seguenti componenti da [!DNL WKND Mobile] in modo che possano essere utilizzati sulla scheda [!DNL Events] pagina API.

   * **[!DNL WKND Mobile > Image]**

      * Logo per l’app

   * **[!DNL WKND Mobile > Text]**

      * Testo introduttivo dell’app

   * **[!DNL WKND Mobile > Content Fragment List]**

      * Elenco delle categorie di eventi disponibili per la visualizzazione nell’app

1. Tocca il **[!UICONTROL Fine]** al termine, nell’angolo in alto a destra.
1. **Aggiorna** finestra del browser per visualizzare [!UICONTROL Componenti consentiti] nella barra a sinistra.
1. Dal Finder Componenti nella barra a sinistra, trascina i seguenti Componenti AEM:
   1. **[!DNL Image]** per il Logo
   2. **[!DNL Text]** per la linea di tag
   3. **[!DNL Content Fragment List]** per gli eventi
1. **Per ciascuno dei componenti di cui sopra**, selezionarli e premere il tasto **sblocca** pulsante.
1. Tuttavia, assicurarsi che **contenitore layout** è **bloccato** per impedire l&#39;aggiunta di altri componenti o la rimozione di questi tre componenti.
1. Tocca **[!UICONTROL Informazioni pagina] > [!UICONTROL Visualizza in Amministrazione]** per tornare al [!DNL WKND Mobile] elenco di modelli. Seleziona la nuova **[!DNL Events API]** modello e tocco **[!UICONTROL Abilita]** nella barra delle azioni superiore.

>[!VIDEO](https://video.tv.adobe.com/v/28342?quality=12&learn=on)

>[!NOTE]
>
> I componenti utilizzati per far emergere il contenuto vengono aggiunti al modello e bloccati. Questo consente agli autori di modificare i componenti predefiniti, ma non di aggiungere o rimuovere arbitrariamente i componenti, in quanto la modifica dell’API stessa potrebbe interrompere i presupposti sulla struttura JSON e interrompere il consumo delle app. Tutte le API devono essere stabili.

## Passaggi successivi

Se necessario, installa [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) pacchetto di contenuti su AEM Author tramite [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp). Questo pacchetto contiene le configurazioni e il contenuto descritti in questo e nei capitoli precedenti dell’esercitazione.

* [Capitolo 5 - Authoring delle pagine di Content Services](./chapter-5.md)
