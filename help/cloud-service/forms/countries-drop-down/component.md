---
title: Creare la struttura per il componente Paesi
description: Creare la struttura dei componenti in crx
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: 87e790c9-6ef6-4337-90b8-687ca576b21a
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 2%

---

# Creare la struttura per il componente Paesi

Accedi all’istanza di AEM Forms e segui questi passaggi per creare un nuovo componente basato sul componente a discesa predefinito:

1. Passa a &quot;/apps/&lt;progetto>/components/adaptiveForm/dropdown&quot; in CRXDE Lite.
2. Copia il componente a discesa e incollalo allo stesso livello di directory.
3. Rinomina il componente copiato in paesi.
4. Aggiorna la proprietà jcr:title del nodo cq:template su Countries.
5. Salva le modifiche.

Ora disponi di un nuovo componente denominato Paesi, che è una replica esatta del componente predefinito a discesa. Questo funge da base per ulteriori personalizzazioni.

## Creare il file HTL

Per creare il file HTL per il componente Paesi:

1. Passa alla cartella dei paesi nell’archivio crx
2. Crea un nuovo file denominato countries.html.
3. Apri il file /apps/core/fd/components/form/dropdown/v1/dropdown/dropdown.html nel repository crx e copiane il contenuto.
4. Incolla il contenuto copiato in countries.html.
5. Modifica il codice per utilizzare il nuovo modello sling come mostrato nella schermata
6. Salva le modifiche.

![sling-model](assets/countriesdropdown.png)

Infine, sincronizza il progetto con questi aggiornamenti per garantire che le modifiche nell’archivio CRX siano visibili nel progetto AEM.


## Passaggi successivi

[Crea cq-dialog](./dialog.md)
