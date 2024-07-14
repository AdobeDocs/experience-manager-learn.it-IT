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
duration: 53
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# Inclusione di bundle di terze parti nel progetto AEM

In questo articolo esamineremo i passaggi necessari per includere il bundle OSGi di terze parti nel tuo progetto AEM. Ai fini di questo articolo includeremo [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) nel nostro progetto AEM.  SE l’OSGi è disponibile nell’archivio maven, includi la dipendenza del bundle nel file POM.xml del progetto.

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

Se il bundle OSGi si trova nel file system, crea una cartella denominata **localjar** nella directory base del progetto(C:\aemformsbundles\AEMFormsProcessStep\localjar). La dipendenza sarà simile alla seguente

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

Stiamo aggiungendo questo bundle al progetto AEM **AEMFormsProcessStep** che si trova nella cartella **c:\aemformsbundles**

* Apri **filter.xml** dalla cartella C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault del progetto
Prendere nota dell&#39;attributo root dell&#39;elemento filter.

* Crea la seguente struttura di cartelle C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* **apps/AEMFormsProcessStep-vendor-packages** è il valore dell&#39;attributo radice nel file filter.xml
* Aggiornare la sezione delle dipendenze del file POM.xml del progetto
* Apri il prompt dei comandi. Vai alla cartella del progetto (c:\aemformsbundles\AEMFormsProcessStep) nel mio caso. Esegui il comando seguente

```java
mvn clean install -PautoInstallSinglePackage
```

Se tutto va bene, il pacchetto viene installato insieme al bundle di terze parti nell’istanza AEM. Puoi controllare il bundle utilizzando [console Web Felix](http://localhost:4502/system/console/bundles). Il bundle di terze parti è disponibile nella cartella /apps dell&#39;archivio `crx` come mostrato di seguito
![terze parti](assets/custom-bundle1.png)
