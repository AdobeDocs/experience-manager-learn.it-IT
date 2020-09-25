---
title: Impostazione della distribuzione del documento del canale Web
seo-title: Impostazione della distribuzione del documento del canale Web
description: Questa è l'ultima parte di un'esercitazione multistep per la creazione del primo documento di comunicazione interattiva. In questa parte, esaminiamo la consegna di documenti per canali web via e-mail.
seo-description: Questa è l'ultima parte di un'esercitazione multistep per la creazione del primo documento di comunicazione interattiva. In questa parte, esaminiamo la consegna di documenti per canali web via e-mail.
uuid: c1066600-1abd-4401-b04f-b93c28603cc7
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
translation-type: tm+mt
source-git-commit: 4f51f7bf00827210d2631b9335768a9980f6655c
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 1%

---


# Impostazione della distribuzione del documento del canale Web {#setting-up-the-delivery-of-web-channel-document}


In questa parte, esaminiamo la consegna di documenti per canali web via e-mail.

Dopo aver definito e verificato il documento di comunicazione interattiva per il canale Web, è necessario un meccanismo di distribuzione per distribuire il documento per il canale Web al destinatario.

Per poter utilizzare l&#39;e-mail come meccanismo di consegna per il documento del canale Web, è necessario apportare una lieve modifica al modello dati del modulo.

[Per saperne di più sulla distribuzione dei canali web tramite e-mail](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

Accedete a  AEM Forms.

* Passa a Forms ->Integrazioni dati

* Aprire il modello dati RetirementAccountStatement in modalità di modifica.

* Selezionare l&#39;oggetto balances e fare clic sul pulsante Modifica.

* Selezionate l&#39;icona &quot;matita&quot; per aprire l&#39;argomento id in modalità di modifica.

* Modificate il binding in &quot;RequestAttribute&quot;.

* Impostare il numero di conto nel valore di binding come illustrato di seguito.

* In questo modo passeremo il numero di conto attraverso l&#39;attributo request al modello dati del modulo

* Assicuratevi di salvare le modifiche.
   ![fdm](assets/requestattribute.gif)

## Verifica distribuzione e-mail del documento canale Web {#test-email-delivery-of-web-channel-document}

* [Installare le risorse di esempio mediante il gestore pacchetti](assets/webchanneldelivery.zip)
* [Login a crx](http://localhost:4502/crx/de/index.jsp#)

* Passa a /home/users

* Cercare l&#39;utente admin nel nodo dell&#39;utente.

* Selezionate il nodo del profilo dell&#39;utente amministratore.

* Create una proprietà denominata &quot;accountnumber&quot;. Assicurarsi che il tipo di proprietà sia una stringa.

* Impostate il valore di questa proprietà accountnumber su &quot;3059827&quot;. Potete impostare questo valore su qualsiasi numero casuale desiderato.

* [Open getad.html](http://localhost:4502/content/getad.html)

* Il codice associato a questo URL ottiene il numero di account dell&#39;utente che ha effettuato l&#39;accesso. Questo numero di conto viene quindi passato come attributo request al file FDM. FDM recupererà quindi i dati associati a questo numero di conto e compilerà il documento del canale Web.
>[!NOTE]
Consulta il file **/apps/AEMForms/fetchad/GET.jsp** in crx. Assicurarsi che la variabile String webChannelDocument indichi un percorso di documento di comunicazione valido.
