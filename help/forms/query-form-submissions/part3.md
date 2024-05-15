---
title: Creare un’interfaccia query
description: Tutorial in più parti che illustra i passaggi necessari per eseguire query sugli invii di moduli memorizzati nel portale di Azure
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: 570a8176-ecf8-4a57-ab58-97190214c53f
duration: 25
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 1%

---

# Creare un’interfaccia query

È stata creata una semplice interfaccia di query per consentire all’&quot;Amministratore&quot; di inserire criteri di ricerca per recuperare invii di moduli specifici. I risultati vengono quindi visualizzati in un semplice formato tabulare con l’opzione per visualizzare un particolare invio di modulo.

![query-submissions](assets/query-submissions.png)

I menu a discesa in questa interfaccia sono a discesa a cascata. Le opzioni disponibili nel menu a discesa cambiano in base alle selezioni effettuate nel menu a discesa precedente.

I menu a discesa vengono compilati utilizzando le origini dati RESTful.

I risultati della ricerca vengono visualizzati in un componente personalizzato denominato &quot;SearchResults&quot;. Quando l’utente fa clic sul pulsante di visualizzazione, il modulo viene precompilato con i dati e gli allegati inviati.

## Passaggi successivi

[Scrivere il servizio di precompilazione](./part4.md)
