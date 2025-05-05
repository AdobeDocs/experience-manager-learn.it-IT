---
title: Creare una configurazione di servizi cloud
description: Crea un’origine dati per la connessione a Salesforce utilizzando le credenziali OAuth
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms, Integrations
jira: KT-7148
thumbnail: 331755.jpg
exl-id: e2d56e91-c13e-4787-a97f-255938b5d290
duration: 173
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 16%

---

# Creazione di Data Source

Creare un&#39;origine dati con supporto REST utilizzando il file Swagger creato nel passaggio precedente.

>[!VIDEO](https://video.tv.adobe.com/v/3411551?quality=12&learn=on&captions=ita)

| Impostazione | Valore |
|---------------------|-----------------------------------------------------------------|
| URL OAuth | https://login.salesforce.com/services/oauth2/authorize |
| Ambito autorizzazione | api chatter_api id completo openid refresh_token visualforce web |
| Aggiorna URL token | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |
| URL token di accesso | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |


**I nomi di dominio dell&#39;URL del token di aggiornamento e di accesso dovranno essere modificati per corrispondere alle impostazioni dell&#39;account Salesforce**