---
title: Come impostare un ambiente di sviluppo rapido
description: Scopri come impostare un ambiente di sviluppo rapido per AEM as a Cloud Service.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
duration: 512
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 3%

---

# Come impostare un ambiente di sviluppo rapido

Scopri **come impostare** Rapid Development Environment (RDE) nell’AEM as a Cloud Service.

Questo video mostra:

- Aggiunta di un RDE al programma tramite Cloud Manager
- Flusso degli accessi RDE con Adobe IMS: come è simile a qualsiasi altro ambiente AEM as a Cloud Service
- Configurazione di [CLI estensibile Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) noto anche come `aio CLI`
- Configurazione di AEM RDE e Cloud Manager `aio CLI` plugin

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## Prerequisito

È necessario installare localmente quanto segue:

- [Node.js](https://nodejs.org/it/) (LTS - Supporto a lungo termine)
- [npm 8+](https://docs.npmjs.com/)

## Configurazione locale

Per distribuire [Progetto WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) codice e contenuto nell&#39;RDE dal computer locale, completa i passaggi seguenti.

### CLI estensibile Adobe I/O Runtime

Installare Adobe I/O Runtime Extensible CLI, noto anche come `aio CLI` eseguendo il comando seguente dalla riga di comando.

```shell
$ npm install -g @adobe/aio-cli
```

### Plug-in AEM

Installare i plug-in di Cloud Manager e AEM RDE utilizzando `aio cli`di `plugins:install` comando.

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager

$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
```

Il plug-in Cloud Manager consente agli sviluppatori di interagire con Cloud Manager dalla riga di comando.

Il plug-in AEM RDE consente agli sviluppatori di distribuire codice e contenuto dal computer locale.

Inoltre, per aggiornare i plug-in utilizza `aio plugins:update` comando.

## Configurare i plug-in AEM

I plug-in dell’AEM devono essere configurati per interagire con l’RDE. Innanzitutto, utilizza l’interfaccia utente di Cloud Manager e copia i valori dell’ID organizzazione, programma e ambiente.

1. ID organizzazione: copia il valore da **Immagine profilo > Informazioni account (interno) > Finestra modale > ID organizzazione corrente**

   ![ID organizzazione](./assets/Org-ID.png)

1. ID programma: copia il valore da **Panoramica del programma > Ambienti > {ProgramName}-rde > URI browser > numeri tra `program/` e`/environment`**

1. ID ambiente: copia il valore da **Panoramica del programma > Ambienti > {ProgramName}-rde > URI browser > numeri dopo`environment/`**

   ![ID programma e ambiente](./assets/Program-Environment-Id.png)

1. Quindi, utilizzando `aio cli`di `config:set` per impostare questi valori, eseguire il comando seguente.

   ```shell
   $ aio config:set cloudmanager_orgid <org-id>
   
   $ aio config:set cloudmanager_programid <program-id>
   
   $ aio config:set cloudmanager_environmentid <env-id>
   ```

Puoi verificare i valori di configurazione correnti eseguendo il seguente comando.

```shell
$ aio config:list
```

Inoltre, per cambiare organizzazione o sapere a quale organizzazione si è attualmente connessi, è possibile utilizzare il comando seguente.

```shell
$ aio where
```

## Verifica accesso RDE

Verificare l&#39;installazione e la configurazione del plug-in AEM RDE eseguendo il comando seguente.

```shell
$ aio aem:rde:status
```

Le informazioni sullo stato RDE vengono visualizzate come stato dell’ambiente, l’elenco delle _il tuo progetto AEM_ bundle e configurazioni nel servizio Author e Publish.

## Passaggio successivo

Scopri [come utilizzare](./how-to-use.md) un RDE per distribuire codice e contenuti dall&#39;ambiente di sviluppo integrato (IDE) preferito per cicli di sviluppo più rapidi.


## Risorse aggiuntive

[Abilitazione di RDE nella documentazione di un programma](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

Configurazione di [CLI estensibile Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) noto anche come `aio CLI`

[Utilizzo e comandi della CLI AIO](https://github.com/adobe/aio-cli#usage)

[Plug-in Adobe I/O Runtime CLI per interazioni con ambienti di sviluppo rapido AEM](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Plug-in CLI AIO di Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager)
