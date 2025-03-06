---
title: Progetto di esempio
description: Progetto di esempio che può essere importato ed eseguito nel tuo ambiente
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
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

