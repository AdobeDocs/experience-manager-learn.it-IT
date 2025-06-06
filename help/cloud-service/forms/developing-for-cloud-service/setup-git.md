---
title: Installare e configurare Git
description: Inizializzare l’archivio Git locale
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
jira: KT-8848
exl-id: 31487027-d528-48ea-b626-a740b94dceb8
duration: 48
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# Installare Git


[Installa Git](https://git-scm.com/downloads). È possibile selezionare le impostazioni predefinite e completare il processo di installazione.
Vai al prompt dei comandi
Passa a c:\cloudmanager\aem-banking-app
digita in git - version. Dovresti visualizzare la versione di GIT installata nel sistema

## Inizializza archivio Git locale

Verifica di trovarti nella cartella c:\cloudmanager\aem-banking-app

```
git init
```

Il comando precedente inizializzerà il progetto come archivio locale Git

```
git add .
```

In questo modo tutti i file di progetto vengono aggiunti all’archivio Git pronto per il commit nell’archivio Git

```
git commit -m "initial commit"
```

In questo modo i file vengono inviati all’archivio Git



## Registrare l’archivio di Cloud Manager con il nostro archivio Git locale

Accedere all’archivio di cloud manager
![accedere alle informazioni sulla rappresentazione](assets/cloud-manager-repo.png)
Ottieni le credenziali dell’archivio di Cloud Manager
![get-credentials](assets/cloud-manager-repo1.png)

Salva il nome utente nel file di configurazione

```java
git config --global credential.username "gbedekar-adobe-com"
```

salva la password nel file di configurazione

```java
git config --global user.password "XXXX"
```

(la password è la password dell’archivio Git di Cloud Manager)

Registra l’archivio Git di Cloud Manager con l’archivio Git locale. Il comando seguente associa **bankingapp** all&#39;archivio Git di Cloud Manager remoto. Avresti potuto usare un nome qualsiasi invece di **bankingapp**


```shell
git remote add bankingapp https://git.cloudmanager.adobe.com/<cloud-manager-repo-path>
```

(Assicurati di utilizzare l’URL dell’archivio)

Verifica se l’archivio remoto è registrato

```java
git remote -v
```

## Passaggi successivi

[Sincronizzare AEM con il progetto AEM in IntelliJ](./intellij-and-aem-sync.md)
