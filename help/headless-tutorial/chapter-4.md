---
title: Capitolo 4 - Definizione dei modelli di Content Services
seo-title: Guida introduttiva a AEM headless - Capitolo 4 - Definizione dei modelli di Content Services
description: Il capitolo 4 dell'AEM tutorial headless descrive il ruolo di AEM Modelli modificabili nel contesto di AEM Content Services. I modelli modificabili vengono utilizzati per definire la struttura di contenuto JSON AEM Content Services verrà in ultima istanza esposta.
seo-description: Il capitolo 4 dell'AEM tutorial headless descrive il ruolo di AEM Modelli modificabili nel contesto di AEM Content Services. I modelli modificabili vengono utilizzati per definire la struttura di contenuto JSON AEM Content Services verrà in ultima istanza esposta.
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '835'
ht-degree: 0%

---


# Capitolo 4 - Definizione dei modelli di Content Services

Il capitolo 4 dell&#39;AEM tutorial headless descrive il ruolo di AEM Modelli modificabili nel contesto di AEM Content Services. I modelli modificabili vengono utilizzati per definire la struttura di contenuto JSON AEM Content Services espone ai client tramite la composizione di Content Services abilitata AEM Componenti.

## Comprendere il ruolo dei modelli in AEM Content Services

AEM Modelli modificabili vengono utilizzati per definire i punti finali HTTP a cui si accede per esporre il contenuto dell’evento come JSON.

Tradizionalmente AEM Modelli modificabili vengono utilizzati per definire le pagine Web, ma questo utilizzo è semplicemente una convenzione. I modelli modificabili possono essere utilizzati per comporre **qualsiasi** set di contenuti; modalità di accesso al contenuto: come HTML in un browser, come JSON utilizzato da JavaScript (AEM SPA Editor) o da un&#39;app mobile è una funzione della modalità in cui viene richiesta la pagina.

In AEM Content Services, i modelli modificabili vengono utilizzati per definire il modo in cui vengono esposti i dati JSON.

Per l&#39; [!DNL WKND Mobile] app, creeremo un singolo modello modificabile che verrà utilizzato per guidare un singolo endpoint API. Anche se questo esempio è semplice per illustrare i concetti di AEM senza titolo, potete creare più pagine (o endpoint) ciascuna con diversi set di contenuti per creare un&#39;API più complessa e meglio organizzata.

## Punto finale dell&#39;API

Per capire come comporre il nostro endpoint API e capire quale contenuto deve essere esposto alla nostra [!DNL WKND Mobile] App, rivisitiamo la progettazione.

![Decomposizione pagina API eventi](./assets/chapter-4/design-to-component-mapping.png)

Come possiamo vedere, abbiamo tre set logici di contenuti da fornire all&#39;app mobile.

1. Il **logo**
2. Linea **tag**
3. Elenco degli **eventi**

A tal fine, possiamo mappare questi requisiti su AEM componenti (e, nel nostro caso, AEM componenti core WCM) per esporre il contenuto richiesto come JSON.

1. Il **logo** verrà visualizzato tramite un componente **Immagine**
2. La riga **** tag viene visualizzata tramite un componente **Testo**
3. L’elenco degli **eventi** viene visualizzato tramite un componente **Elenco frammenti di** contenuto che a sua volta fa riferimento a un set di frammenti di contenuto evento.

>[!NOTE]
>
>Per supportare AEM&#39;esportazione JSON di pagine e componenti da parte di Content Service, le pagine e i componenti devono **derivare AEM componenti** core di WCM.
>
>[AEM componenti](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) core di WCM dispongono di funzionalità incorporate per supportare uno schema JSON normalizzato di pagine e componenti esportati. Tutti i componenti WKND Mobile utilizzati in questa esercitazione (Pagina, Immagine, Testo ed Elenco frammenti di contenuto) sono derivati AEM componenti core di WCM.

## Definizione del modello API per gli eventi

1. Passare a **[!UICONTROL Strumenti]>[!UICONTROL Generale]>[!UICONTROL Modelli]>[!DNL WKND Mobile]**.

1. Create il **[!DNL Events API]** modello:

   1. Toccate **[!UICONTROL Crea]** nella barra delle azioni superiore
   1. Select the **[!DNL WKND Mobile - Empty Page]** template
   1. Toccate **[!UICONTROL Avanti]** nella barra delle azioni superiore
   1. Inserire **[!DNL Events API]** nel campo Titolo  modello
   1. Toccate **[!UICONTROL Crea]** nella barra delle azioni superiore
   1. Toccate **[!UICONTROL Apri]** per aprire il nuovo modello per la modifica

1. In primo luogo, consentiamo ai tre componenti AEM identificati di modellare il contenuto modificando il [!UICONTROL criterio] del contenitore [!UICONTROL di]layout principale. Assicurarsi che la modalità **[!UICONTROL Struttura]** sia attiva, selezionare il **[!DNL Layout Container \[Root\]]** criterio e toccare il pulsante **[!UICONTROL Criterio]** .
1. In **[!UICONTROL Proprietà]> Componenti[!UICONTROL consentiti]** , cercare **[!DNL WKND Mobile]**. Consentite i seguenti componenti dal gruppo di [!DNL WKND Mobile] componenti in modo che possano essere utilizzati nella pagina [!DNL Events] API.

   * **[!DNL WKND Mobile > Image]**

      * Logo dell’app
   * **[!DNL WKND Mobile > Text]**

      * Testo introduttivo dell&#39;app
   * **[!DNL WKND Mobile > Content Fragment List]**

      * Elenco delle categorie di eventi disponibili per la visualizzazione nell&#39;app



1. Toccate il segno di spunta **[!UICONTROL Fine]** nell’angolo in alto a destra al termine.
1. **Aggiorna** la finestra del browser per visualizzare l&#39;elenco dei componenti [!UICONTROL consentiti] nella barra a sinistra.
1. Da Browser componenti nella barra a sinistra, trascinate i seguenti componenti AEM:
   1. **[!DNL Image]** per il logo
   2. **[!DNL Text]** per la riga del tag
   3. **[!DNL Content Fragment List]** per gli eventi
1. **Per ciascuno dei componenti** di cui sopra, selezionateli e premete il pulsante **Sblocca** .
1. Tuttavia, accertatevi che il Contenitore **di** layout sia **bloccato** per impedire l&#39;aggiunta di altri componenti o la rimozione di questi tre.
1. Toccate Informazioni **pagina >[!UICONTROL Visualizza in Amministratore]** per tornare all’elenco dei [!DNL WKND Mobile] modelli. Selezionate il modello appena creato e toccate **[!DNL Events API]** Attiva **** nella barra delle azioni superiore.

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> I componenti utilizzati per la superficie del contenuto vengono aggiunti al Modello stesso e bloccati. Questo consente agli autori di modificare i componenti predefiniti, ma non di aggiungere o rimuovere arbitrariamente i componenti, poiché la modifica dell’API stessa potrebbe interrompere i presupposti intorno alla struttura JSON e interrompere le app. Tutte le API devono essere stabili.

## Passaggi successivi

Se necessario, installate il pacchetto di contenuti [com.adobe.aem.guide.wknd-mobile.content.Chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) su AEM Author tramite [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). Questo pacchetto contiene le configurazioni e il contenuto descritti in questo e nei precedenti capitoli dell&#39;esercitazione.

* [Capitolo 5 - Authoring delle pagine di Content Services](./chapter-5.md)
