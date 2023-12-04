---
title: Configurazione delle estensioni di Reader in AEM Forms OSGi
description: Aggiungere le credenziali delle estensioni di Reader all’archivio fonti attendibili in AEM Forms OSGi
feature: Reader Extensions
type: Tutorial
version: 6.4,6.5
topic: Administration
role: Admin
level: Beginner
exl-id: 1f16acfd-e8fd-4b0d-85c4-ed860def6d02
last-substantial-update: 2020-08-01T00:00:00Z
duration: 328
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# Aggiungi credenziali estensioni di Reader{#configuring-reader-extension-osgi}

Il servizio DocAssurance può applicare diritti di utilizzo ai documenti PDF. Per applicare i diritti di utilizzo ai documenti di PDF, configura i certificati.

## Crea keystore per utente fd-service

Le credenziali delle estensioni Reader sono associate all&#39;utente fd-service. Per aggiungere le credenziali all&#39;utente fd-service, procedere come segue. Se hai già creato il keystore per l&#39;utente fd-service, salta questa sezione

* Accedi all’istanza di authoring dell’AEM come amministratore
* Vai a Strumenti-Protezione-Utenti
* Scorri verso il basso l’elenco degli utenti fino a trovare l’account utente fd-service
* Fai clic sull’utente fd-service
* Fai clic sulla scheda keystore
* Fai clic su Crea registro chiavi
* Imposta la password di accesso al registro chiavi e salva le impostazioni per creare la password del registro chiavi

### Aggiungi credenziali al keystore utente fd-service

Segui il video per aggiungere le credenziali all’utente fd-service

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=12&learn=on)


Il comando per elencare i dettagli del file pfx è. Il comando seguente presuppone che ci si trovi nella stessa directory del file pfx.

**keytool -v -list -storetype pkcs12 -keystore &lt;name of=&quot;&quot; your=&quot;&quot; pfx=&quot;&quot; file=&quot;&quot;>**

Ad esempio, keytool -v -list -storetype pkcs12 -keystore 1005566.pfx dove 1005566.pfx è il nome del file pfx
