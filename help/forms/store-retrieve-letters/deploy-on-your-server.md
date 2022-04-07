---
title: Distribuire le risorse di esempio sul server
description: Verifica la funzionalità Salva come bozza per le comunicazioni interattive
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
source-git-commit: db99787c48e49a9861de893e6cb7fbb7b31807b8
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 2%

---

# Distribuire le risorse di esempio sul server

Segui le istruzioni riportate di seguito per far funzionare questa funzionalità sul server AEM

* [Creare lo schema del database](assets/icdrafts.sql)
* [Importare la libreria client](assets/icdrafts.zip)
* [Importare il modulo adattivo](assets/SavedDraftsAdaptiveForm.zip)
* Crea origine dati denominata _SaveAndContinue_

![Crea origine dati](assets/data-source.png)

| Nome proprietà | Valore proprietà |
|---|---|
| Nome origine dati | SaveAndContinue |
| Classe del driver JDBC | com.mysql.cj.jdbc.Driver |
| URL di connessione JDBC | jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&amp;useSSL=false&amp;characterEncoding=utf8&amp;useUnicode=true |

* [Distribuzione del bundle icbots](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Assicurati di _Abilita Salva con CCRDocumentInstanceService_ nella configurazione OSGI come mostrato di seguito
   ![Abilita bozze](assets/enable-drafts.png)
* Apri qualsiasi comunicazione interattiva. Fai clic su Salva come bozza per salvare
* [Visualizza bozze salvate](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>I file xml vengono memorizzati nella cartella principale dell&#39;installazione del server AEM. Il progetto eclipse >ti viene fornito per personalizzare la soluzione in base alle tue esigenze.

Il progetto eclipse con implementazione di esempio può essere [scaricato da qui](assets/icdrafts-eclipse-project.zip)
