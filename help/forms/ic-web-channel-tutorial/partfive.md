---
title: Creazione di frammenti di documento per contenere il nome e l'indirizzo del destinatario
seo-title: Creazione di frammenti di documento per contenere il nome e l'indirizzo del destinatario
description: 'Questa è la parte 5 di un''esercitazione con più passaggi per la creazione del primo documento di comunicazione interattiva. In questa parte, verrà creato un frammento di documento contenente il nome e l''indirizzo del destinatario. '
seo-description: 'Questa è la parte 5 di un''esercitazione con più passaggi per la creazione del primo documento di comunicazione interattiva. In questa parte, verrà creato un frammento di documento contenente il nome e l''indirizzo del destinatario. '
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 0%

---


# Creazione di frammenti di documento per contenere il nome e l&#39;indirizzo del destinatario {#creating-document-fragments-to-hold-the-recipient-name-and-address}

In questa parte, verrà creato un frammento di documento contenente il nome e l&#39;indirizzo del destinatario.

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

I frammenti di documento contengono il contenuto testuale dei documenti di comunicazione interattivi. Questo contenuto di testo può essere testo statico o inserito dai valori degli elementi del modello dati sottostante. Ad esempio Gentile {name}, dove Gentile è testo statico e {name} è il nome dell’elemento dati del modulo. In fase di esecuzione, questo si risolverà a Caro Gloria Rios o Caro John Jacobs a seconda del valore dell&#39;elemento name.

L&#39;editor Rich Text è sufficientemente intuitivo da consentire agli utenti aziendali di creare testo e di inserire elementi di dati del modulo. L&#39;editor di frammenti di documento è in grado di formattare il testo, specificare i tipi di font e gli stili, inserire caratteri speciali e creare collegamenti ipertestuali.

L&#39;editor di frammenti di documento è inoltre in grado di inserire condizioni in linea nel testo, come dimostrato in questo [video](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Assicurarsi che gli elementi del modello dati modulo inseriti in un frammento di documento siano discendenti dell&#39;elemento principale. Ad esempio, in questo caso di utilizzo verificare che gli elementi dell&#39;oggetto Utente selezionati siano gli elementi secondari dell&#39;oggetto balances

