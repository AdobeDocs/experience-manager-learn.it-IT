---
title: Distribuire le risorse di esempio sul server
description: Verificare la funzionalità Salva come bozza per le comunicazioni interattive
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
duration: 28
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 2%

---

# Distribuire le risorse di esempio sul server

Segui le istruzioni seguenti per utilizzare questa funzionalità sul tuo server AEM

* [Creare lo schema del database](assets/icdrafts.sql)
* [Importare la libreria client](assets/icdrafts.zip)
* [Importare il modulo adattivo](assets/SavedDraftsAdaptiveForm.zip)
* Crea origine dati denominata _SaveAndContinue_

![Crea Data Source](assets/data-source.png)

| Nome proprietà | Valore proprietà |
|---|---|
| Nome origine dati | `SaveAndContinue` |
| Classe driver JDBC | `com.mysql.cj.jdbc.Driver` |
| URL connessione JDBC | `jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&useSSL=false&characterEncoding=utf8&useUnicode=true` |

* [Distribuire il bundle icdraft](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Assicurati di _abilitare il salvataggio utilizzando CCRDocumentInstanceService_ nella configurazione OSGI come mostrato di seguito
  ![Abilita bozze](assets/enable-drafts.png)
* Apri una comunicazione interattiva. Fate clic sul pulsante Salva come bozza per salvare
* [Visualizza bozze salvate](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>I file xml vengono memorizzati nella cartella principale dell’installazione del server AEM. Il progetto di eclissi >è fornito per personalizzare la soluzione in base alle tue esigenze.

Il progetto eclipse con implementazione di esempio può essere [scaricato da qui](assets/icdrafts-eclipse-project.zip)
