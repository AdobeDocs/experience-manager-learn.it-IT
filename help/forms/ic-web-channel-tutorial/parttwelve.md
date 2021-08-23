---
title: Impostazione della consegna del documento del canale web
seo-title: Impostazione della consegna del documento del canale web
description: Questa è la parte finale di un'esercitazione su più passaggi per la creazione del primo documento di comunicazione interattiva. In questa parte esaminiamo la consegna del documento del canale web via e-mail.
seo-description: Questa è la parte finale di un'esercitazione su più passaggi per la creazione del primo documento di comunicazione interattiva. In questa parte esaminiamo la consegna del documento del canale web via e-mail.
uuid: c1066600-1abd-4401-b04f-b93c28603cc7
feature: Comunicazione interattiva
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
topic: Sviluppo
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 2%

---


# Impostazione della consegna del documento del canale web {#setting-up-the-delivery-of-web-channel-document}


In questa parte esaminiamo la consegna del documento del canale web via e-mail.

Una volta definito e verificato il documento di comunicazione interattiva del canale web, è necessario un meccanismo di consegna per consegnare il documento del canale web al destinatario.

Per poter utilizzare l’e-mail come meccanismo di consegna per il documento del canale web, è necessario apportare una modifica minore al modello di dati del modulo.

[Per ulteriori informazioni sulla consegna dei canali web tramite e-mail](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

Accedi ad AEM Forms.

* Passa a Forms ->Integrazioni dati

* Apri il modello dati RetirementAccountStatement in modalità di modifica.

* Selezionare l&#39;oggetto balance e fare clic sul pulsante modifica.

* Seleziona l’icona &quot;matita&quot; per aprire l’argomento id in modalità di modifica.

* Modificare il binding in &quot;RequestAttribute&quot;.

* Impostare il numero di conto nel valore di binding come illustrato di seguito.

* In questo modo passiamo il numero di conto attraverso l’attributo di richiesta al modello di dati del modulo

* Assicurati di salvare le modifiche.
   ![fdm](assets/requestattribute.gif)

## Verifica della consegna di un documento del canale Web {#test-email-delivery-of-web-channel-document}

* [Installare le risorse di esempio utilizzando Gestione pacchetti](assets/webchanneldelivery.zip)
* [Accedi a crx](http://localhost:4502/crx/de/index.jsp#)

* Passa a /home/users

* Cerca l&#39;utente amministratore sotto il nodo dell&#39;utente.

* Seleziona il nodo del profilo dell&#39;utente amministratore.

* Crea una proprietà denominata &quot;numero contabile&quot;. Assicurati che il tipo di proprietà sia una stringa.

* Impostare il valore di questa proprietà account number su &quot;3059827&quot;. Puoi impostare questo valore su qualsiasi numero casuale desiderato.

* [Apri getad.html](http://localhost:4502/content/getad.html)

* Il codice associato a questo URL ottiene il numero di account dell’utente connesso. Questo numero di account viene quindi passato come requestattribute all’FDM. FDM recupererà quindi i dati associati a questo numero di account e compilerà il documento del canale web.

>[!NOTE]
>
>Guarda il file **/apps/AEMForms/fetchad/GET.jsp** in crx. Assicurati che la variabile String webChannelDocument punti a un percorso di documento di comunicazione valido.
