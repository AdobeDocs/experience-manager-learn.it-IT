---
title: Guida introduttiva  AEM Forms e  Adobe Campaign Standard
seo-title: Guida introduttiva  AEM Forms e  Adobe Campaign Standard
description: Integrare  AEM Forms con  Adobe Campaign Standard utilizzando  AEM Forms Form Data Model per recuperare le informazioni sul profilo della campagna ACS, ecc.
seo-description: Integrare  AEM Forms con  Adobe Campaign Standard utilizzando  AEM Forms Form Data Model per recuperare le informazioni sul profilo della campagna ACS, ecc.
uuid: 56450c9b-3752-4a64-b1b3-8c78e81f5921
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 89245554-7b99-4e7e-9810-52191f9ea365
translation-type: tm+mt
source-git-commit: 3b44a9e2341ce23f737e6ef75c67fadd9870d2ac
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---


# Guida introduttiva  AEM Forms e  Adobe Campaign Standard {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

Questa esercitazione elenca i vari casi di utilizzo per l&#39;integrazione  AEM Forms con  Adobe Campaign Standard(ACS).

ACS dispone di una vasta gamma di API che consentono di interfacciare ACS con la tecnologia desiderata. In questa esercitazione, ci concentreremo sull&#39;interfaccia  AEM Forms con ACS.

Per integrare  AEM Forms con ACS è necessario seguire i seguenti passaggi:

* [Configurare l&#39;accesso API sull&#39;istanza ACS.](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* Crea token Web JSON.
* Scambiate il token Web JSON con  Adobe  servizio Identity Management per un token di accesso.
* Includete questo Token di accesso nell&#39;intestazione HTTP Autorizzazione, insieme a X-API-Key in ogni richiesta ad istanza ACS.

Per iniziare segui le istruzioni seguenti

* [Scaricate e decomprimete il file ZIP delle risorse correlate a questa esercitazione.](assets/aem-forms-and-acs-bundles.zip)
* Distribuire i bundle utilizzando la [console Web Felix](http://localhost:4502/system/console/bundles)
* Fornire le impostazioni appropriate per  Adobe Campaign nella configurazione Felix OSGI.
* [Create un utente di servizio come indicato in questo articolo](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Assicuratevi di distribuire il bundle OSGi associato all&#39;articolo.
* Memorizzate la chiave privata ACS in etc/key/campaign/private.key. Dovrete creare una cartella denominata campagna in etc/key.
* [Fornire l&#39;accesso in lettura alla cartella della campagna per l&#39;utente del servizio &quot;data&quot;.](http://localhost:4502/useradmin)
