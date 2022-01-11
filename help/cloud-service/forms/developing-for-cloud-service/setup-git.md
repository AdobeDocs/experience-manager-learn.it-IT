---
title: Installare e configurare Git
description: Inizializzare l’archivio Git locale
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8848
exl-id: 31487027-d528-48ea-b626-a740b94dceb8
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# Installa Git


[Installa Git](https://git-scm.com/downloads). È possibile selezionare le impostazioni predefinite e completare il processo di installazione.
Vai al prompt dei comandi Vai a c:\cloudmanager\aem-banking-app type in git —version. Dovresti vedere la versione di GIT installata sul tuo sistema

## Inizializza archivio Git locale

Assicurati di essere nella c:\cloudmanager\aem-banking-app folder

```
git init
```

Il comando precedente inizializza il progetto come archivio locale git

```
git add .
```

In questo modo tutti i file di progetto vengono aggiunti all’archivio Git, pronto per il commit nell’archivio Git

```
git commit -m "initial commit"
```

In questo modo i file vengono inviati all’archivio Git



## Registra l’archivio cloud manager con il nostro archivio Git locale

Accedere al repository di cloud manager
![accedere alle informazioni del rep](assets/cloud-manager-repo.png)
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

(La password è la password dell’archivio Git di cloud manager)

Registra l’archivio Git di cloud manager con il tuo archivio Git locale. Il comando sottostante associa **bankapp** con l’archivio git di cloud manager remoto. Avreste potuto usare un nome qualsiasi invece di **bankapp**


```shell
git remote add bankingapp https://git.cloudmanager.adobe.com/<cloud-manager-repo-path>
```

(Assicurati di utilizzare l&#39;URL del tuo archivio)

Controlla se l&#39;archivio remoto è registrato

```java
git remote -v
```
