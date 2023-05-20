---
title: Impostazione della consegna del documento del canale web
seo-title: Setting up the delivery of web channel document
description: Questa è la parte finale di un tutorial a più passaggi per creare il tuo primo documento di comunicazione interattiva. In questa parte, esaminiamo la consegna del documento del canale web tramite e-mail.
seo-description: This is the final part of a multistep tutorial for creating your first interactive communications document. In this part, we look at the delivery of web channel document via email.
uuid: c1066600-1abd-4401-b04f-b93c28603cc7
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
topic: Development
role: Developer
level: Beginner
exl-id: 510d1782-59b9-41a6-a071-a16170f2cd06
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 1%

---

# Impostazione della consegna del documento del canale web {#setting-up-the-delivery-of-web-channel-document}


In questa parte, esaminiamo la consegna del documento del canale web tramite e-mail.

Dopo aver definito e testato il documento di comunicazione interattiva per il canale web, è necessario un meccanismo di consegna per consegnare il documento al destinatario.

Per poter utilizzare le e-mail come meccanismo di consegna per il documento del canale web, è necessario apportare una modifica minore al modello di dati del modulo.

[Per ulteriori informazioni sulla consegna del canale web tramite e-mail](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

Accedi ad AEM Forms.

* Passa a Forms ->Integrazioni dati

* Aprire il modello dati RetirementAccountStatement in modalità di modifica.

* Selezionare l&#39;oggetto Balance e fare clic sul pulsante Modifica.

* Seleziona l’icona &quot;pencil&quot; per aprire l’argomento id in modalità di modifica.

* Modificare l&#39;associazione in &quot;RequestAttribute&quot;.

* Impostare il numero di account nel valore di binding come illustrato di seguito.

* In questo modo viene trasmesso il numero account attraverso l’attributo di richiesta al modello dati del modulo

* Assicurati di salvare le modifiche.
   ![fdm](assets/requestattribute.gif)

## Verifica consegna e-mail del documento del canale web {#test-email-delivery-of-web-channel-document}

* [Installare le risorse di esempio tramite Gestione pacchetti](assets/webchanneldelivery.zip)
* [Accedi a crx](http://localhost:4502/crx/de/index.jsp#)

* Passa a /home/users

* Cerca l’utente amministratore sotto il nodo dell’utente.

* Seleziona il nodo del profilo dell’utente amministratore.

* Crea una proprietà denominata &quot;accountnumber&quot;. Assicurati che il tipo di proprietà sia una stringa.

* Impostare il valore di questa proprietà accountnumber su &quot;3059827&quot;. Puoi impostare questo valore su qualsiasi numero casuale, a tua scelta.

* [Apri getad.html](http://localhost:4502/content/getad.html)

* Il codice associato a questo URL otterrà il numero di account dell’utente connesso. Questo numero di conto viene quindi passato come requestattribute a FDM. FDM recupera quindi i dati associati a questo numero account e compila il documento del canale web.

>[!NOTE]
>
>Dai un&#39;occhiata al **/apps/AEMForms/fetchad/GET.jsp** file in crx. Assicurarsi che la variabile stringa webChannelDocument punti a un percorso di documento di comunicazione valido.

## Passaggi successivi

[Configura consegna e-mail](../interactive-communications/delivery-of-web-channel-document-tutorial-use.md)