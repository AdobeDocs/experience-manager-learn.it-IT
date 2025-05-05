---
title: Integrare AEM Forms e Adobe Campaign Standard
description: Integra AEM Forms con Adobe Campaign Standard utilizzando AEM Forms Form Data Model per recuperare informazioni sul profilo della campagna ACS, ecc.
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: e028837b-13d8-4058-ac25-ed095f49524c
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 44
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 1%

---

# Integrare AEM Forms e Adobe Campaign Standard

![formsandcampaign](assets/helpx-cards-forms.png)

Scopri come integrare AEM Forms con Adobe Campaign Standard (ACS).

ACS ha un set completo di API esposte che consente di interfacciare ACS con la tecnologia di nostra scelta. In questa esercitazione, ci concentreremo sull’interfaccia di AEM Forms con ACS.

Per integrare AEM Forms con ACS è necessario seguire i seguenti passaggi:

* [Configura l&#39;accesso API nell&#39;istanza ACS.](https://experienceleague.adobe.com/docs/campaign-standard/using/working-with-apis/get-started-apis.html?lang=it)
* Crea token web JSON.
* Scambia il token web JSON con il servizio Adobe Identity Management per un token di accesso.
* Includi questo token di accesso nell’intestazione HTTP di autorizzazione, insieme a X-API-Key in ogni richiesta all’istanza ACS.

Per iniziare, segui le seguenti istruzioni

* [Scarica e decomprimi le risorse correlate a questa esercitazione.](assets/aem-forms-and-acs-bundles.zip)
* Distribuisci i bundle utilizzando [la console Web Felix](http://localhost:4502/system/console/bundles)
* Specifica le impostazioni appropriate per Adobe Campaign nella configurazione Felix OSGI.
* [Crea un utente del servizio come menzionato in questo articolo](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Assicurati di distribuire il bundle OSGi associato all’articolo.
* Memorizza la chiave privata ACS in etc/key/campaign/private.key. Dovrai creare una cartella denominata campaign in etc/key.
* [Fornire l&#39;accesso in lettura alla cartella della campagna ai &quot;dati&quot; dell&#39;utente del servizio.](http://localhost:4502/useradmin)

## Passaggi successivi

[Generare JWT e token di accesso](partone.md)
