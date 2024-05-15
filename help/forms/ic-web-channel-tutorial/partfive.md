---
title: Creazione di frammenti di documenti in cui inserire il nome e l'indirizzo del destinatario
description: Questa è la parte 5 di un tutorial in più passaggi per creare il tuo primo documento di comunicazione interattiva. In questa parte verrà creato un frammento di documento contenente il nome e l’indirizzo del destinatario.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: Development
role: Developer
level: Beginner
exl-id: 1d7093a8-3765-46ec-912a-b5a5503fd5af
duration: 219
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# Creazione di frammenti di documenti in cui inserire il nome e l&#39;indirizzo del destinatario {#creating-document-fragments-to-hold-the-recipient-name-and-address}

In questa parte verrà creato un frammento di documento contenente il nome e l’indirizzo del destinatario.

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

I frammenti di documento contengono il contenuto testuale dei documenti di comunicazione interattivi. Questo contenuto di testo può essere testo statico o inserito dai valori degli elementi del modello dati sottostante. Ad esempio Gentile {name}, dove Gentile è un testo statico e {name} è il nome dell’elemento dati del modulo. In fase di runtime, questo si risolverà in Dear Gloria Rios o Dear John Jacobs a seconda del valore dell&#39;elemento name.

L’editor Rich Text è abbastanza intuitivo da consentire a un utente aziendale di creare testo e inserire elementi dati del modulo. L’editor dei frammenti di documento consente di formattare il testo, specificare tipi di carattere e stili, inserire caratteri speciali e creare collegamenti ipertestuali.

L’editor del frammento di documento può anche inserire condizioni in linea nel testo, come illustrato in questo [video](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Assicurati che gli elementi del modello dati modulo inseriti in un frammento di documento siano discendenti dell’elemento principale. In questo caso d&#39;uso, ad esempio, accertarsi che gli elementi dell&#39;oggetto User selezionati siano figlio dell&#39;oggetto balance

## Passaggi successivi

[Crea documento di comunicazione interattiva](./partsix.md)