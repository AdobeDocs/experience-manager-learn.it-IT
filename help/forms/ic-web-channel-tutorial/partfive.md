---
title: Creazione di frammenti di documento per contenere il nome e l’indirizzo del destinatario
seo-title: Creating Document Fragments to hold the recipient name and address
description: Questa è la parte 5 di un tutorial in più passaggi per la creazione del primo documento di comunicazione interattiva. In questa parte verrà creato un frammento di documento per contenere il nome e l'indirizzo del destinatario.
seo-description: This is part 5 of a multi-step tutorial for creating your first interactive communications document. In this part, we will create document fragment to hold the recipient name and address.
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: Development
role: Developer
level: Beginner
exl-id: 1d7093a8-3765-46ec-912a-b5a5503fd5af
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---

# Creazione di frammenti di documento per contenere il nome e l’indirizzo del destinatario {#creating-document-fragments-to-hold-the-recipient-name-and-address}

In questa parte verrà creato un frammento di documento per contenere il nome e l&#39;indirizzo del destinatario.

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

I frammenti di documento contengono il contenuto testuale dei documenti di comunicazione interattivi. Questo contenuto di testo può essere un testo statico o inserito dai valori degli elementi del modello dati sottostante. Ad esempio, Gentile {name}, dove Gentile è un testo statico e {name} è il nome dell&#39;elemento dati del modulo. In fase di runtime, questo si risolverà a Caro Gloria Rios o Caro John Jacobs a seconda del valore dell&#39;elemento name.

L’editor Rich Text è abbastanza intuitivo da consentire agli utenti aziendali di creare testo e di inserire elementi di dati modulo. L’editor frammento di documento può formattare il testo, specificare tipi di font e stili, inserire caratteri speciali e creare collegamenti ipertestuali.

L’editor dei frammenti di documento può anche inserire condizioni in linea nel testo, come dimostrato in questo [video](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Assicurati che gli elementi del modello dati modulo inseriti in un frammento di documento siano discendenti dell’elemento principale. Ad esempio, in questo caso d&#39;uso verificare che gli elementi dell&#39;oggetto Utente selezionati siano figli dell&#39;oggetto balance
