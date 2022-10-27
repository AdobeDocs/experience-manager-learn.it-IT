---
title: Configurazione delle estensioni di Reader in AEM Forms OSGi
description: Aggiungi credenziali di estensioni di Reader all’archivio attendibilità in AEM Forms OSGi
feature: Reader Extensions
audience: developer
type: Tutorial
version: 6.4,6.5
topic: Administration
role: Admin
level: Beginner
exl-id: 1f16acfd-e8fd-4b0d-85c4-ed860def6d02
last-substantial-update: 2020-08-01T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# Aggiungi credenziale estensioni di Reader{#configuring-reader-extension-osgi}

Il servizio DocAssurance può applicare diritti di utilizzo ai documenti PDF. Per applicare i diritti di utilizzo ai documenti PDF, configurare i certificati.

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


Il comando per elencare i dettagli del file pfx è. Il seguente comando presuppone che ti trovi nella stessa directory del file pfx .

**keytool -v -list -storetype pkcs12 -keystore &lt;name of=&quot;&quot; your=&quot;&quot; pfx=&quot;&quot; file=&quot;&quot;>**

Ad esempio keytool -v -list -storetype pkcs12 -keystore 1005566.pfx dove 1005566.pfx è il nome del mio file pfx
