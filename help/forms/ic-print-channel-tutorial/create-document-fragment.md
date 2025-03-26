---
title: Creazione di un frammento di documento
description: Questa è la parte 5 di un tutorial in più passaggi per creare il tuo primo documento di comunicazione interattiva. In questa parte verrà creato un frammento di documento contenente il nome e l’indirizzo del destinatario.
feature: Interactive Communication
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
jira: KT-5958
thumbnail: 22350.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 2fe3f950-bc2a-4e91-8d91-00438691727a
duration: 217
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 0%

---

# Creazione di un frammento di documento

In questa parte verrà creato un frammento di documento contenente il nome e l’indirizzo del destinatario.

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

I frammenti di documento contengono il contenuto testuale dei documenti di comunicazione interattivi. Questo contenuto di testo può essere testo statico o inserito dai valori degli elementi del modello dati sottostante. Ad esempio **Gentile _{name}_**, dove Gentile è il testo statico e Nome è il nome dell&#39;elemento del modello dati del modulo. In fase di runtime, verrà risolto in **Gentile Gloria Rios**o **Gentile John Jacobs**a seconda del valore dell&#39;elemento name.

L’editor Rich Text è abbastanza intuitivo da consentire a un utente aziendale di creare testo e inserire elementi dati del modulo. L’editor dei frammenti di documento consente di formattare il testo, specificare tipi di carattere e stili, inserire caratteri speciali e creare collegamenti ipertestuali.

L&#39;editor frammento di documento consente inoltre di inserire condizioni in linea nel testo, come illustrato in questo [video](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Assicurati che gli elementi del modello dati modulo inseriti in un frammento di documento siano discendenti dell’elemento principale. In questo caso d&#39;uso, ad esempio, accertarsi che gli elementi dell&#39;oggetto User selezionati siano figlio dell&#39;oggetto balance

## Passaggi successivi

[Crea documento canale di stampa](./create-print-channel-document.md)
