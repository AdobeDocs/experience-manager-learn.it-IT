---
title: Guida introduttiva ad AEM Forms e Adobe Campaign Standard
description: Integra AEM Forms con Adobe Campaign Standard utilizzando AEM Forms Form Data Model per recuperare le informazioni sul profilo della campagna ACS, ecc.
feature: Forms adattivo, modello dati modulo
version: 6.3,6.4,6.5
topic: Sviluppo
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---


# Guida introduttiva ad AEM Forms e Adobe Campaign Standard {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

Questa esercitazione elenca i vari casi d’uso per l’integrazione di AEM Forms con Adobe Campaign Standard(ACS).

ACS dispone di un set completo di API disponibili che consentono l&#39;interazione di ACS con la tecnologia di nostra scelta. In questa esercitazione, ci concentreremo sull’interfaccia di AEM Forms con ACS.

Per integrare AEM Forms con ACS è necessario seguire i seguenti passaggi:

* [Imposta l&#39;accesso API sull&#39;istanza ACS.](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* Crea token web JSON.
* Sostituisci il token web JSON con Adobe Identity Management Service per un token di accesso.
* Includi questo token di accesso nell’intestazione HTTP di autorizzazione, insieme a X-API-Key in ogni richiesta all’istanza ACS.

Per iniziare segui le seguenti istruzioni

* [Scarica e decomprimi le risorse correlate a questa esercitazione.](assets/aem-forms-and-acs-bundles.zip)
* Distribuisci i bundle utilizzando [Felix web console](http://localhost:4502/system/console/bundles)
* Fornisci le impostazioni appropriate per Adobe Campaign nella configurazione Felix OSGI.
* [Crea un utente di servizio come menzionato in questo articolo](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Assicurati di distribuire il bundle OSGi associato all&#39;articolo.
* Memorizza la chiave privata ACS in etc/key/campaign/private.key. Sarà necessario creare una cartella denominata campaign in etc/key.
* [Fornisci l&#39;accesso in lettura alla cartella della campagna all&#39;utente del servizio &quot;dati&quot;.](http://localhost:4502/useradmin)
