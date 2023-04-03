---
title: Creazione di un frammento di documento
description: Questa è la parte 5 di un tutorial in più passaggi per la creazione del primo documento di comunicazione interattiva. In questa parte verrà creato un frammento di documento per contenere il nome e l'indirizzo del destinatario.
seo-description: This is part 5 of a multi-step tutorial for creating your first interactive communications document. In this part, we will create document fragment to hold the recipient name and address.
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
kt: 5958
thumbnail: 22350.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 2fe3f950-bc2a-4e91-8d91-00438691727a
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Creazione di un frammento di documento

In questa parte verrà creato un frammento di documento per contenere il nome e l&#39;indirizzo del destinatario.

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

I frammenti di documento contengono il contenuto testuale dei documenti di comunicazione interattivi. Questo contenuto di testo può essere un testo statico o inserito dai valori degli elementi del modello dati sottostante. Esempio **Gentile _{name}_**, dove Gentile è testo statico e il nome è il nome dell’elemento del modello di dati del modulo. In fase di runtime, questo determinerà&#x200B;**Caro Gloria Rios**o **Gentile John Jacobs**a seconda del valore dell’elemento name.

L’editor Rich Text è abbastanza intuitivo da consentire agli utenti aziendali di creare testo e di inserire elementi di dati modulo. L’editor frammento di documento può formattare il testo, specificare tipi di font e stili, inserire caratteri speciali e creare collegamenti ipertestuali.

L’editor dei frammenti di documento può anche inserire condizioni in linea nel testo, come dimostrato in questo [video](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Assicurati che gli elementi del modello dati modulo inseriti in un frammento di documento siano discendenti dell’elemento principale. Ad esempio, in questo caso d&#39;uso verificare che gli elementi dell&#39;oggetto Utente selezionati siano figli dell&#39;oggetto balance
