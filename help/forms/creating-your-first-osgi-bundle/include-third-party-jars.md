---
title: Inclusione di file JAR di terze parti
description: Scopri come utilizzare il file jar di terze parti nel progetto AEM
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
jira: KT-11245
last-substantial-update: 2022-10-15T00:00:00Z
thumbnail: third-party.jpg
exl-id: e8841c63-3159-4f13-89a1-d8592af514e3
duration: 66
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# Inclusione di bundle di terze parti nel progetto AEM

In questo articolo esamineremo i passaggi necessari per includere il bundle OSGi di terze parti nel tuo progetto AEM. Ai fini di questo articolo includeremo il [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) nel nostro progetto AEM.  SE l’OSGi è disponibile nell’archivio maven, includi la dipendenza del bundle nel file POM.xml del progetto.

>[!NOTE]
> Si presume che il file jar di terze parti sia un bundle OSGi

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

Se il bundle OSGi si trova nel file system, crea una cartella denominata **localjar** nella directory base del progetto (C:\aemformsbundles\AEMFormsProcessStep\localjar) la dipendenza avrà un aspetto simile al seguente

```java
<dependency>
    <groupId>jsch</groupId>
    <artifactId>jsch</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/localjar/jsch-0.1.55-bundle.jar</systemPath>
</dependency>
```

## Creare la struttura di cartelle

Stiamo aggiungendo questo pacchetto al nostro progetto AEM **AEMFormsProcessStep** che risiede in **c:\aemformsbundles** cartella

* Apri **filter.xml** dalla cartella C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault del progetto Prendi nota dell’attributo root dell’elemento filtro.

* Crea la seguente struttura di cartelle C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* Il **apps/AEMFormsProcessStep-vendor-packages** è il valore dell&#39;attributo principale nel file filter.xml
* Aggiornare la sezione delle dipendenze del file POM.xml del progetto
* Apri il prompt dei comandi. Vai alla cartella del progetto (c:\aemformsbundles\AEMFormsProcessStep) nel mio caso. Esegui il comando seguente

```java
mvn clean install -PautoInstallSinglePackage
```

Se tutto va bene, il pacchetto viene installato insieme al bundle di terze parti nell’istanza AEM. Puoi controllare il bundle utilizzando [console web felix](http://localhost:4502/system/console/bundles). Il bundle di terze parti è disponibile nella cartella /apps del file `crx` archivio come mostrato di seguito
![terze parti](assets/custom-bundle1.png)
