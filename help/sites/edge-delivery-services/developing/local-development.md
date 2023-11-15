---
title: Configurare un ambiente di sviluppo locale per i Edge Delivery Services
description: Come impostare un ambiente di sviluppo locale per i Edge Delivery Services.
version: 6.5, Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
last-substantial-update: 2023-11-15T00:00:00Z
jira: KT-14483
thumbnail: 3425717.jpeg
source-git-commit: d17544c4f8dda03e5147a1f48dbbdae005ee9438
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 0%

---


# Configurare un ambiente di sviluppo locale

Come impostare un ambiente di sviluppo locale per i Edge Delivery Services.

>[!VIDEO](https://video.tv.adobe.com/v/3425717/?learn=on)


## Passaggi descritti nel video

1. Installare AEM CLI

   ```
   $ sudo npm install -g @adobe/aem-cli
   ```

1. Cambia la directory in una directory di progetto che sia un archivio Git creato da [Piastra riscaldante per AEM](https://github.com/adobe/aem-boilerplate) modello.

   ```
   $ git clone git@github.com:my-org/my-project.git
   $ cd my-project
   ```

1. Esegui AEM CLI per avviare l’istanza AEM locale.

   ```
   $ pwd
     /Users/my-user/my-project
   
   $ aem up
       ___    ________  ___                          __      __ 
      /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
     / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
    / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
   /_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/
   
   info: Starting AEM dev server vx.x.x
   info: Local AEM dev server up and running: http://localhost:3000/
   info: Enabled reverse proxy to https://main--my-project--my-org.hlx.page
   opening default browser: http://localhost:3000/
   ```

1. Apri http://localhost:3000/ nel browser per visualizzare il sito Web AEM.

