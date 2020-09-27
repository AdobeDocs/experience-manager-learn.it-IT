---
title: Creazione di un frammento di documento
description: 'Questa è la parte 5 di un''esercitazione con più passaggi per la creazione del primo documento di comunicazione interattiva. In questa parte, verrà creato un frammento di documento contenente il nome e l''indirizzo del destinatario. '
seo-description: 'Questa è la parte 5 di un''esercitazione con più passaggi per la creazione del primo documento di comunicazione interattiva. In questa parte, verrà creato un frammento di documento contenente il nome e l''indirizzo del destinatario. '
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
kt: 5958
thumbnail: 22350.jpg
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 0%

---


# Creazione di un frammento di documento

In questa parte, verrà creato un frammento di documento contenente il nome e l&#39;indirizzo del destinatario.

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

I frammenti di documento contengono il contenuto testuale dei documenti di comunicazione interattivi. Questo contenuto di testo può essere testo statico o inserito dai valori degli elementi del modello dati sottostante. Ad esempio, **Gentile _{name}_**, dove Gentile è testo statico e Nome è il nome dell’elemento del modello dati del modulo. In fase di esecuzione, questo si risolverà a **Caro Gloria Rios**o **Caro John Jacobs**a seconda del valore dell&#39;elemento name.

L&#39;editor Rich Text è sufficientemente intuitivo da consentire agli utenti aziendali di creare testo e di inserire elementi di dati del modulo. L&#39;editor di frammenti di documento è in grado di formattare il testo, specificare i tipi di font e gli stili, inserire caratteri speciali e creare collegamenti ipertestuali.

L&#39;editor di frammenti di documento è inoltre in grado di inserire condizioni in linea nel testo, come dimostrato in questo [video](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Assicurarsi che gli elementi del modello dati modulo inseriti in un frammento di documento siano discendenti dell&#39;elemento principale. Ad esempio, in questo caso di utilizzo verificare che gli elementi dell&#39;oggetto Utente selezionati siano gli elementi secondari dell&#39;oggetto balances

