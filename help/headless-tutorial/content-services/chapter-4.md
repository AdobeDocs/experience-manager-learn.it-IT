---
title: Capitolo 4 - Definizione dei modelli di Content Services - Content Services
seo-title: Guida introduttiva a AEM Headless - Capitolo 4 - Definizione dei modelli di Content Services
description: Il capitolo 4 dell’esercitazione AEM headless descrive il ruolo dei modelli modificabili AEM contesto di Content Services AEM. I modelli modificabili vengono utilizzati per definire la struttura del contenuto JSON AEM Content Services verrà in ultima analisi esposto.
seo-description: Il capitolo 4 dell’esercitazione AEM headless descrive il ruolo dei modelli modificabili AEM contesto di Content Services AEM. I modelli modificabili vengono utilizzati per definire la struttura del contenuto JSON AEM Content Services verrà in ultima analisi esposto.
feature: Frammenti di contenuto, API
topic: Senza testa, gestione dei contenuti
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 0%

---


# Capitolo 4 - Definizione dei modelli di Content Services

Il capitolo 4 dell’esercitazione AEM headless descrive il ruolo dei modelli modificabili AEM contesto di Content Services AEM. I modelli modificabili vengono utilizzati per definire la struttura del contenuto JSON AEM Content Services espone ai clienti tramite la composizione di Content Services abilitato AEM Componenti.

## Ruolo dei modelli in AEM Content Services

AEM Modelli modificabili vengono utilizzati per definire i punti finali HTTP a cui si accede per esporre il contenuto dell’evento come JSON.

Tradizionalmente, i modelli modificabili vengono utilizzati per definire le pagine web, ma questo utilizzo è semplicemente una convenzione. I modelli modificabili possono essere utilizzati per comporre **qualsiasi** set di contenuti; modalità di accesso a tale contenuto: come HTML in un browser, in quanto JSON utilizzato da JavaScript (AEM editor di SPA) o da un’app mobile è una funzione del modo in cui viene richiesta tale pagina.

In AEM Content Services, i modelli modificabili vengono utilizzati per definire la modalità di esposizione dei dati JSON.

Per l’ [!DNL WKND Mobile] app , creeremo un singolo modello modificabile che verrà utilizzato per indirizzare un singolo endpoint API. Anche se questo esempio è semplice per illustrare i concetti di AEM headless, puoi creare più pagine (o endpoint) ciascuna con diversi set di contenuti da esporre per creare un’API più complessa e meglio organizzata.

## Informazioni sul punto finale dell’API

Per capire come comporre il nostro endpoint API e capire quale contenuto deve essere esposto alla nostra [!DNL WKND Mobile] app, rivediamo la progettazione.

![Decomposizione della pagina API degli eventi](./assets/chapter-4/design-to-component-mapping.png)

Come possiamo vedere, abbiamo tre set logici di contenuti da fornire all’app mobile.

1. Il **Logo**
2. La **riga tag**
3. Elenco di **Eventi**

A questo scopo, possiamo mappare questi requisiti sui componenti AEM (e, nel nostro caso, sui componenti core AEM WCM) per esporre il contenuto richiesto come JSON.

1. Il **Logo** verrà visualizzato tramite un **Componente immagine**
2. Il **Tag Line** verrà visualizzato tramite un **componente Testo**
3. L’elenco di **Eventi** verrà visualizzato tramite un **componente Elenco frammenti di contenuto** che a sua volta fa riferimento a un set di frammenti di contenuto evento.

>[!NOTE]
>
>Per supportare AEM’esportazione JSON di pagine e componenti da parte di Content Service, le pagine e i componenti devono essere **derivati dai componenti core di AEM WCM**.
>
>[AEM ](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) componenti core WCM sono dotati di funzionalità incorporate per supportare uno schema JSON normalizzato di pagine e componenti esportati. Tutti i componenti WKND Mobile utilizzati in questa esercitazione (Pagina, Immagine, Testo ed Elenco frammenti di contenuto) sono derivati AEM componenti core WCM.

## Definizione del modello API per gli eventi

1. Passa a **[!UICONTROL Strumenti] > [!UICONTROL Generale] > [!UICONTROL Modelli] >[!DNL WKND Mobile]**.

1. Crea il modello **[!DNL Events API]**:

   1. Tocca **[!UICONTROL Crea]** nella barra delle azioni superiore
   1. Seleziona il modello **[!DNL WKND Mobile - Empty Page]**
   1. Tocca **[!UICONTROL Avanti]** nella barra delle azioni superiore
   1. Inserisci **[!DNL Events API]** nel campo [!UICONTROL Titolo modello]
   1. Tocca **[!UICONTROL Crea]** nella barra delle azioni superiore
   1. Tocca **[!UICONTROL Apri]** per aprire il nuovo modello da modificare

1. In primo luogo, consentiamo ai tre componenti AEM identificati di modellare il contenuto modificando il [!UICONTROL Criterio] della directory principale [!UICONTROL Contenitore di layout]. Assicurati che la modalità **[!UICONTROL Struttura]** sia attiva, seleziona il **[!DNL Layout Container \[Root\]]** e tocca il pulsante **[!UICONTROL Criterio]** .
1. In **[!UICONTROL Proprietà] > [!UICONTROL Componenti consentiti]** cerca **[!DNL WKND Mobile]**. Consenti l&#39;utilizzo dei seguenti componenti dal gruppo di componenti [!DNL WKND Mobile] in modo che possano essere utilizzati nella pagina API [!DNL Events].

   * **[!DNL WKND Mobile > Image]**

      * Logo dell’app
   * **[!DNL WKND Mobile > Text]**

      * Testo introduttivo dell’app
   * **[!DNL WKND Mobile > Content Fragment List]**

      * Elenco delle categorie di eventi disponibili per la visualizzazione nell’app



1. Toccare il segno di spunta **[!UICONTROL Fine]** nell&#39;angolo superiore destro al termine.
1. **** Aggiorna la finestra del browser per visualizzare l’elenco dei componenti   consentiti nella barra a sinistra.
1. Da Components Finder nella barra a sinistra, trascina i seguenti Componenti AEM:
   1. **[!DNL Image]** per il logo
   2. **[!DNL Text]** per la riga tag
   3. **[!DNL Content Fragment List]** per gli eventi
1. **Per ciascuno dei componenti** di cui sopra, selezionali e premi il pulsante  **** Slockbutton.
1. Assicurati tuttavia che il **contenitore layout** sia **bloccato** per impedire l&#39;aggiunta di altri componenti o che questi tre componenti vengano rimossi.
1. Tocca **[!UICONTROL Informazioni pagina] > [!UICONTROL Visualizza in Admin]** per tornare all&#39;elenco dei modelli [!DNL WKND Mobile]. Seleziona il modello **[!DNL Events API]** appena creato e tocca **[!UICONTROL Abilita]** nella barra delle azioni superiore.

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> I componenti utilizzati per la visualizzazione del contenuto vengono aggiunti al modello stesso e bloccati. Questo consente agli autori di modificare i componenti predefiniti, ma non di aggiungere o rimuovere in modo arbitrario i componenti, in quanto la modifica dell’API stessa potrebbe interrompere i presupposti intorno alla struttura JSON e interrompere le app consumiste. Tutte le API devono essere stabili.

## Passaggi successivi

Facoltativamente, installa il pacchetto di contenuti [com.adobe.aem.guides.wknd-mobile.content.capitolo-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) su AEM Author tramite [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). Questo pacchetto contiene le configurazioni e il contenuto descritti in questo e nei precedenti capitoli dell&#39;esercitazione.

* [Capitolo 5 - Authoring delle pagine dei servizi per i contenuti](./chapter-5.md)
