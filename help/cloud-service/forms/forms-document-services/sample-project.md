---
title: Progetto di esempio
description: Progetto di esempio che può essere importato ed eseguito nel tuo ambiente
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: f1fcc4bb-cc31-45e8-b7bb-688ef6a236bb
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# Testarlo nell’ambiente locale

* Importa il progetto

   * Scarica ed estrai il [progetto di esempio](./assets/formsdocumentservices.zip)
   * Apri **Java Development Environment** (IntelliJ IDEA, Eclipse o VS Code) e importa il progetto come progetto Maven
* Configurare le credenziali

   * Individua il file `resources/credentials/server_credentials.json`
   * Apri e **aggiorna le credenziali** specifiche per il tuo ambiente.
   * Assicurati che includa valori validi per:

     `clientId`, `clientSecret`,`adobeIMSV3TokenEndpointURL` e
     `scopes`

* Eseguire la classe Main

   * Passa a `src/main/java/Main.java` ed esegui il metodo principale

* Verificare l’esecuzione
   * Verificare l&#39;uscita nella finestra del terminale
