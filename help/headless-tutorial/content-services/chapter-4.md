---
title: Capitolo 4 - Definizione dei modelli di Content Services - Content Services
description: Il capitolo 4 dell’esercitazione AEM headless descrive il ruolo dei modelli modificabili AEM contesto di Content Services AEM. I modelli modificabili vengono utilizzati per definire la struttura del contenuto JSON AEM Content Services in ultima analisi.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 0%

---

# Capitolo 4 - Definizione dei modelli di Content Services

Il capitolo 4 dell’esercitazione AEM headless descrive il ruolo dei modelli modificabili AEM contesto di Content Services AEM. I modelli modificabili vengono utilizzati per definire la struttura del contenuto JSON AEM Content Services espone ai clienti tramite la composizione di Content Services abilitato AEM Componenti.

## Ruolo dei modelli in AEM Content Services

AEM Modelli modificabili vengono utilizzati per definire i punti finali HTTP a cui si accede per esporre il contenuto dell’evento come JSON.

Tradizionalmente, i modelli modificabili vengono utilizzati per definire le pagine web, ma questo utilizzo è semplicemente una convenzione. I modelli modificabili possono essere utilizzati per la composizione **qualsiasi** insieme del contenuto; modalità di accesso a tale contenuto: come HTML in un browser, in quanto JSON utilizzato da JavaScript (AEM editor di SPA) o da un’app mobile è una funzione del modo in cui viene richiesta tale pagina.

In AEM Content Services, i modelli modificabili vengono utilizzati per definire la modalità di esposizione dei dati JSON.

Per [!DNL WKND Mobile] L’app creerà un singolo modello modificabile utilizzato per indirizzare un singolo endpoint API. Anche se questo esempio è semplice per illustrare i concetti di AEM headless, puoi creare più pagine (o endpoint) ciascuna con diversi set di contenuti da esporre per creare un’API più complessa e meglio organizzata.

## Informazioni sul punto finale dell’API

Per comprendere come comporre il nostro endpoint API e capire quale contenuto deve essere esposto al nostro [!DNL WKND Mobile] App, rivediamo la progettazione.

![Decomposizione della pagina API degli eventi](./assets/chapter-4/design-to-component-mapping.png)

Come possiamo vedere, abbiamo tre set logici di contenuti da fornire all’app mobile.

1. La **Logo**
2. La **Riga tag**
3. L&#39;elenco di **Eventi**

A questo scopo, possiamo mappare questi requisiti sui componenti AEM (e, nel nostro caso, sui componenti core AEM WCM) per esporre il contenuto richiesto come JSON.

1. La **Logo** viene visualizzata tramite un **Componente immagine**
2. La **Riga tag** viene visualizzata tramite un **Componente testo**
3. L&#39;elenco di **Eventi** viene visualizzata tramite un **Componente Elenco frammenti di contenuto** che a sua volta fa riferimento a un set di frammenti di contenuto evento.

>[!NOTE]
>
>Per supportare AEM’esportazione JSON di pagine e componenti da parte di Content Service, le pagine e i componenti devono **derivano AEM componenti core WCM**.
>
>[AEM componenti core WCM](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) dispongono di funzionalità incorporate per supportare uno schema JSON normalizzato di pagine e componenti esportati. Tutti i componenti WKND Mobile utilizzati in questa esercitazione (Pagina, Immagine, Testo ed Elenco frammenti di contenuto) sono derivati AEM componenti core WCM.

## Definizione del modello API per gli eventi

1. Passa a **[!UICONTROL Strumenti] > [!UICONTROL Generale] > [!UICONTROL Modelli] >[!DNL WKND Mobile]**.

1. Crea il **[!DNL Events API]** modello:

   1. Tocca **[!UICONTROL Crea]** nella barra delle azioni superiore
   1. Seleziona la **[!DNL WKND Mobile - Empty Page]** template
   1. Tocca **[!UICONTROL Successivo]** nella barra delle azioni superiore
   1. Invio **[!DNL Events API]** in [!UICONTROL Titolo modello] field
   1. Tocca **[!UICONTROL Crea]** nella barra delle azioni superiore
   1. Tocca **[!UICONTROL Apri]** apri il nuovo modello per la modifica

1. In primo luogo, consentiamo ai tre componenti AEM identificati di modellare il contenuto modificando il [!UICONTROL Criterio] della radice [!UICONTROL Contenitore di layout]. Assicurati che **[!UICONTROL Struttura]** modalità attiva, seleziona la **[!DNL Layout Container \[Root\]]** e tocca **[!UICONTROL Criterio]** pulsante .
1. Sotto **[!UICONTROL Proprietà] > [!UICONTROL Componenti consentiti]** cercare **[!DNL WKND Mobile]**. Consenti i seguenti componenti dalla [!DNL WKND Mobile] gruppo di componenti in modo che possano essere utilizzati [!DNL Events] Pagina API.

   * **[!DNL WKND Mobile > Image]**

      * Logo dell’app
   * **[!DNL WKND Mobile > Text]**

      * Testo introduttivo dell’app
   * **[!DNL WKND Mobile > Content Fragment List]**

      * Elenco delle categorie di eventi disponibili per la visualizzazione nell’app



1. Tocca **[!UICONTROL Fine]** segno di spunta nell&#39;angolo superiore destro al termine.
1. **Aggiorna** la finestra del browser per visualizzare la nuova [!UICONTROL Componenti consentiti] nella barra a sinistra.
1. Da Components Finder nella barra a sinistra, trascina i seguenti Componenti AEM:
   1. **[!DNL Image]** per il logo
   2. **[!DNL Text]** per la riga tag
   3. **[!DNL Content Fragment List]** per gli eventi
1. **Per ciascuno dei componenti di cui sopra**, selezionali e premi il pulsante **sbloccare** pulsante .
1. Tuttavia, assicurati che **Contenitore di layout** è **bloccato** per evitare l’aggiunta di altri componenti o la rimozione di questi tre componenti.
1. Tocca **[!UICONTROL Informazioni pagina] > [!UICONTROL Visualizza in Amministratore]** per tornare al [!DNL WKND Mobile] elenco dei modelli. Seleziona la nuova creazione **[!DNL Events API]** modello e tocca **[!UICONTROL Abilita]** nella barra delle azioni superiore.

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> I componenti utilizzati per la visualizzazione del contenuto vengono aggiunti al modello stesso e bloccati. Questo consente agli autori di modificare i componenti predefiniti, ma non di aggiungere o rimuovere in modo arbitrario i componenti, in quanto la modifica dell’API stessa potrebbe interrompere i presupposti intorno alla struttura JSON e interrompere le app consumiste. Tutte le API devono essere stabili.

## Passaggi successivi

Facoltativamente, installa il [com.adobe.aem.guides.wknd-mobile.content.capitolo-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) pacchetto di contenuti su AEM Author tramite [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp). Questo pacchetto contiene le configurazioni e il contenuto descritti in questo e nei precedenti capitoli dell&#39;esercitazione.

* [Capitolo 5 - Authoring delle pagine dei servizi per i contenuti](./chapter-5.md)
