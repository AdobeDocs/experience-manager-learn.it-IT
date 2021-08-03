---
title: Configurazione delle estensioni di Reader in AEM Forms OSGi
description: Aggiungi credenziali di estensione Reader all'archivio attendibilità in AEM Forms OSGi
feature: Estensioni Reader
feature-set: Reader Extensions
topics: development
audience: developer
doc-type: Tutorial
activity: implement
version: 6.4,6.5
topic: Amministrazione
role: Admin
level: Beginner
source-git-commit: 55a6ff5d01898b994aee60f214126c5c18a06a5e
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---


# Aggiungi credenziale estensioni di Reader{#configuring-reader-extension-osgi}

Il servizio DocAssurance può applicare diritti di utilizzo ai documenti PDF. Per applicare diritti di utilizzo ai documenti PDF, configurare i certificati.

## Crea un keystore per l&#39;utente fd-service

La credenziale delle estensioni del lettore è associata all’utente del servizio fd. Per aggiungere le credenziali all’utente del servizio fd, segui i passaggi seguenti. Se hai già creato il keystore per l&#39;utente fd-service, salta questa sezione

* Accedi alla tua istanza di AEM Author come amministratore
* Vai a Strumenti-Protezione-Utenti
* Scorri verso il basso l&#39;elenco degli utenti fino a trovare l&#39;account utente del servizio fd
* Fai clic sull&#39;utente del servizio fd
* Fai clic sulla scheda keystore
* Fai clic su Crea KeyStore
* Impostare la password di accesso KeyStore e salvare le impostazioni per creare la password KeyStore

### Aggiungi le credenziali al keystore utente fd-service

Segui il video per aggiungere le credenziali all&#39;utente del servizio fd

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=9&learn=on)











