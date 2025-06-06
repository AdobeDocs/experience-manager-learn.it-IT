---
title: Creazione del componente dell’elenco a discesa Paesi
description: Crea un componente elenco a discesa paesi basato su un componente elenco a discesa principale di AEM Forms.
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: aef151bc-daf1-4abd-914a-6299f3fb58e4
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 1%

---

# Creare un componente a discesa Paesi basato su un componente a discesa

La creazione di un nuovo componente core in Adobe Experience Manager (AEM) è un processo emozionante che prevede diversi passaggi, tra cui la definizione della struttura del componente, la personalizzazione della finestra di dialogo e l’implementazione di un modello Sling per la funzionalità dinamica.

Al termine di questo tutorial, avrai acquisito dimestichezza con le procedure seguenti:

* Crea e utilizza un modello Sling per recuperare i dati in modo dinamico.
* Personalizza la finestra di dialogo cq aggiungendo nuovi campi e nascondendone altri.
* Definisci una solida struttura di componenti personalizzata per il riutilizzo.

Il componente, denominato &quot;Paesi&quot;, consentirà agli utenti di selezionare un continente e di popolare dinamicamente un elenco a discesa con i paesi corrispondenti al continente scelto. Questo sarà basato sul componente predefinito dell’elenco a discesa, migliorato per questo caso d’uso specifico.

Immergiamo e creiamo questo componente dinamico e potente!

## Prerequisiti

La creazione di un nuovo componente core in Adobe Experience Manager (AEM) richiede il rispetto di diversi prerequisiti per garantire un processo di sviluppo fluido. Ecco cosa serve prima di iniziare:

* Ambiente di sviluppo AEM: un’installazione funzionale predisposta per il cloud in esecuzione in locale
* Accesso agli strumenti di sviluppo di AEM come Visual Studio Code o IntelliJ
* Configurazione di MAven e progetto AEM con archetipo più recente
* Conoscenza di base dei concetti di AEM
* Strumenti e impostazioni di base, ad esempio archivio Git, versione corretta di JDK


## Passaggi successivi

[Creare una struttura di componenti](./component.md)
