---
title: Installazione dei prerequisiti
description: Installare il software necessario per configurare l'ambiente di sviluppo
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8842
exl-id: 274018b9-91fe-45ad-80f2-e7826fddb37e
duration: 55
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 1%

---

# Installazione del software richiesto

Questa esercitazione ti guiderà attraverso i passaggi necessari per creare un progetto AEM Forms e sincronizzare il progetto AEM Forms con l’istanza AEM locale utilizzando IntelliJ e lo strumento Repo. Scoprirai anche come aggiungere il progetto all’archivio Git locale e inviare l’archivio Git locale all’archivio di Cloud Manager.





Questo tutorial farà riferimento in futuro a questa struttura di cartelle.

* [Installare JDK 11](https://www.oracle.com/java/technologies/downloads/#java11-windows). Ho scaricato jdk-11.0.6_windows-x64_bin.zip
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html).Ad esempio, se hai installato Maven nella cartella c:\maven, dovrai creare una variabile di ambiente denominata M2_HOME con il valore C:\maven\apache-maven-3.6.0. Aggiungere quindi M2_HOME\bin al percorso e salvare l&#39;impostazione.

## Creare un progetto Maven utilizzando l’archetipo di progetto AEM

* Crea una cartella denominata **cloudmanager**(è possibile assegnare qualsiasi nome) nell&#39;unità c
* Apri la finestra del prompt dei comandi e passa a **c:\cloudmanager**
* Copiare e incollare il contenuto del [file di testo](assets/creating-maven-project.txt) nella finestra del prompt dei comandi. Potrebbe essere necessario modificare DarchetypeVersion=30 a seconda del [ultima versione](https://github.com/adobe/aem-project-archetype/releases). L&#39;ultima versione era 30 al momento della stesura di questo articolo.
* Esegui il comando premendo Invio. Se tutto va correttamente, dovrebbe comparire il messaggio di completamento della generazione.

## Passaggi successivi

[Installazione di IntelliJ](./intellij-set-up.md)