---
title: Distribuire le risorse di esempio sul server
description: Verificare la funzionalità Salva come bozza per le comunicazioni interattive
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
duration: 46
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 2%

---

# Distribuire le risorse di esempio sul server

Seguire le istruzioni riportate di seguito per utilizzare questa funzionalità sul proprio server AEM

* [Creare lo schema del database](assets/icdrafts.sql)
* [Importare la libreria client](assets/icdrafts.zip)
* [Importare il modulo adattivo](assets/SavedDraftsAdaptiveForm.zip)
* Crea origine dati denominata _SaveAndContinue_

![Crea origine dati](assets/data-source.png)

| Nome proprietà | Valore proprietà |
|---|---|
| Nome origine dati | SaveAndContinue |
| Classe driver JDBC | com.mysql.cj.jdbc.Driver |
| URL connessione JDBC | jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&amp;useSSL=false&amp;characterEncoding=utf8&amp;useUnicode=true |

* [Distribuire il bundle icdraft](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Assicurati di _Abilita salvataggio tramite CCRDocumentInstanceService_ nella configurazione OSGI come mostrato di seguito
  ![Abilita bozze](assets/enable-drafts.png)
* Apri una comunicazione interattiva. Fate clic sul pulsante Salva come bozza per salvare
* [Visualizza bozze salvate](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>I file xml vengono memorizzati nella cartella principale dell&#39;installazione del server AEM. Il progetto di eclissi >è fornito per personalizzare la soluzione in base alle tue esigenze.

Il progetto di eclissi con implementazione di esempio può essere [scaricato da qui](assets/icdrafts-eclipse-project.zip)
