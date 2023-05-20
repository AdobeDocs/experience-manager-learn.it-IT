---
title: Guida introduttiva di AEM Forms e Adobe Campaign Standard
description: Integra AEM Forms con Adobe Campaign Standard utilizzando AEM Forms Form Data Model per recuperare informazioni sul profilo della campagna ACS, ecc.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: e028837b-13d8-4058-ac25-ed095f49524c
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Guida introduttiva di AEM Forms e Adobe Campaign Standard {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

Questo tutorial elenca i vari casi d’uso per l’integrazione di AEM Forms con Adobe Campaign Standard (ACS).

ACS ha un set completo di API esposte che consente di interfacciare ACS con la tecnologia di nostra scelta. In questa esercitazione, ci concentreremo sull’interfaccia di AEM Forms con ACS.

Per integrare AEM Forms con ACS è necessario seguire i seguenti passaggi:

* [Configura l’accesso API nell’istanza ACS.](https://experienceleague.adobe.com/docs/campaign-standard/using/working-with-apis/get-started-apis.html?lang=en)
* Crea token web JSON.
* Scambia il token web JSON con il servizio Identity Management Adobe per un token di accesso.
* Includi questo token di accesso nell’intestazione HTTP di autorizzazione, insieme a X-API-Key in ogni richiesta all’istanza ACS.

Per iniziare, segui le seguenti istruzioni

* [Scarica e decomprimi le risorse correlate a questa esercitazione.](assets/aem-forms-and-acs-bundles.zip)
* Distribuire i bundle utilizzando [Console web Felix](http://localhost:4502/system/console/bundles)
* Specifica le impostazioni appropriate per Adobe Campaign nella configurazione Felix OSGI.
* [Creare un utente di servizio come indicato in questo articolo](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Assicurati di distribuire il bundle OSGi associato all’articolo.
* Memorizza la chiave privata ACS in etc/key/campaign/private.key. Dovrai creare una cartella denominata campaign in etc/key.
* [Fornisci l’accesso in lettura alla cartella della campagna ai &quot;dati&quot; dell’utente del servizio.](http://localhost:4502/useradmin)

## Passaggi successivi

[Generare JWT e token di accesso](partone.md)
