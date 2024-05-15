---
title: Supporto dell’override della configurazione in base al contesto per il modello dati del modulo
description: Imposta modelli dati modulo per comunicare con endpoint diversi in base agli ambienti.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-10423
exl-id: 2ce0c07b-1316-4170-a84d-23430437a9cc
duration: 80
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 7%

---

# Configurazioni cloud basate sul contesto

Quando crei la configurazione cloud nell’ambiente locale e dopo aver superato i test, vorrai utilizzare la stessa configurazione cloud negli ambienti a monte, ma senza dover modificare l’endpoint, la chiave/password segreta e/o il nome utente. Per ottenere questo caso d’uso, AEM Forms su Cloud Service ha introdotto la possibilità di definire configurazioni cloud basate sul contesto.
Ad esempio, la configurazione cloud dell’account di archiviazione Azure può essere riutilizzata negli ambienti di sviluppo, staging e produzione utilizzando stringhe di connessione e chiavi diverse per.

Per creare la configurazione cloud in base al contesto, sono necessari i seguenti passaggi

## Creare variabili di ambiente

È possibile configurare e gestire le variabili di ambiente standard tramite Cloud Manager. Vengono fornite all’ambiente di runtime e possono essere utilizzate nelle configurazioni OSGi. [Le variabili di ambiente possono essere valori specifici dell’ambiente o segreti dell’ambiente, in base alle modifiche apportate.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=en)



La schermata seguente mostra le variabili di ambiente azure_key e azure_connection_string definite
![environment_variables](assets/environment-variables.png)

Queste variabili di ambiente possono quindi essere specificate nei file di configurazione da utilizzare nell’ambiente appropriato. Ad esempio, se desideri che tutte le istanze di authoring utilizzino queste variabili di ambiente, definisci il file di configurazione nella cartella config.author come specificato di seguito

## Crea file di configurazione

Aprire il progetto in IntelliJ. Passa a config.author e crea un file denominato

```java
org.apache.sling.caconfig.impl.override.OsgiConfigurationOverrideProvider-integrationTest.cfg.json
```

![config.author](assets/config-author.png)

Copiare il testo seguente nel file creato nel passaggio precedente. Il codice in questo file esegue l&#39;override del valore delle proprietà accountName e accountKey con le variabili di ambiente **azure_connection_string** e **azure_key**.

```json
{
  "enabled":true,
  "description":"dermisITOverrideConfig",
  "overrides":[
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountName=\"$[env:azure_connection_string]\"",
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountKey=\"$[secret:azure_key]\""

  ]
}
```

>[!NOTE]
>
>Questa configurazione verrà applicata a tutti gli ambienti di authoring nell’istanza del servizio cloud. Per applicare la configurazione agli ambienti di pubblicazione, è necessario inserire lo stesso file di configurazione nella cartella config.publish del progetto intelliJ
>[!NOTE]
> Assicurati che la proprietà che viene sottoposta a override sia una proprietà valida della configurazione cloud. Passa alla configurazione cloud per trovare la proprietà che desideri escludere, come illustrato di seguito.

![cloud-config-property](assets/cloud-config-properties.png)

Per le configurazioni cloud basate su REST con autenticazione di base, in genere si desidera creare variabili di ambiente per le proprietà serviceEndPoint, userName e password.

## Passaggi successivi

[Invia il progetto AEM a Cloud Manager](./push-project-to-cloud-manager-git.md)
