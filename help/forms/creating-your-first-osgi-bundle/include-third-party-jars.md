---
title: Compresi i barattoli di terze parti
description: Scopri come utilizzare un file jar di terze parti nel tuo progetto AEM
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
kt: 11245
last-substantial-update: 2022-10-15T00:00:00Z
thumbnail: third-party.jpg
source-git-commit: 9229a92a0d33c49526d10362ac4a5f14823294ed
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# Inclusione di bundle di terze parti nel progetto AEM

In questo articolo passeremo attraverso i passaggi necessari per includere il bundle OSGi di terze parti nel tuo progetto AEM.Per lo scopo di questo articolo includeremo il [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) nel nostro progetto AEM.  SE l&#39;OSGi è disponibile nell&#39;archivio maven, includi la dipendenza del bundle nel file POM.xml del progetto.

>[!NOTE]
> Si presume che il jar di terze parti sia un bundle OSGi

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

Se il tuo bundle OSGi è sul tuo file system, crea una cartella chiamata **localjar** sotto la directory di base del progetto (C:\aemformsbundles\AEMFormsProcessStep\localjar) la dipendenza avrà un aspetto simile a questo

```java
<dependency>
    <groupId>jsch</groupId>
    <artifactId>jsch</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/localjar/jsch-0.1.55-bundle.jar</systemPath>
</dependency>
```

## Creare la struttura della cartella

Stiamo aggiungendo questo bundle al nostro progetto AEM **AEMFormsProcessStep** che risiede nel **c:\aemformsbundles** cartella

* Apri **filter.xml** dal sito C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault folder of your project Make a note of the root attribute of the filter element.

* Crea la seguente struttura di cartelle C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* La **apps/AEMFormsProcessStep-vendor-packages** è il valore dell&#39;attributo root nel filtro.xml
* Aggiorna la sezione dipendenze del POM.xml del progetto
* Apri il prompt dei comandi. Passa alla cartella del progetto (c:\aemformsbundles\AEMFormsProcessStep) nel mio caso. Esegui il seguente comando

```java
mvn clean install -pAutoInstallSinglePackage
```

Se tutto va bene, il pacchetto viene installato insieme al bundle di terze parti nella tua istanza AEM. Puoi verificare la presenza del bundle utilizzando [console web felix](http://localhost:4502/system/console/bundles). Il bundle di terze parti è disponibile nella cartella /apps del `crx` repository come mostrato di seguito
![di terzi](assets/custom-bundle1.png)



