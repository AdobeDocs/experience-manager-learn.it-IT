---
title: Installazione dei prerequisiti
description: Installare il software necessario per configurare l'ambiente di sviluppo
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8842
source-git-commit: d38da94bd4164163a16899b565c90b159194580a
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 2%

---


# Installazione del software richiesto

Questa esercitazione ti guiderà attraverso i passaggi necessari per creare un progetto AEM Forms, sincronizzare il progetto AEM Forms con l’istanza AEM locale utilizzando IntelliJ e lo strumento repo. Scoprirai anche come aggiungere il progetto all’archivio git locale e inviare l’archivio git locale all’archivio cloud manager.




Questa esercitazione farà riferimento alla struttura delle cartelle in corso.

* [Installa JDK 11](https://www.oracle.com/java/technologies/downloads/#java11-windows). Ho scaricato jdk-11.0.6_windows-x64_bin.zip
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html).Ad esempio, se hai installato Maven in c:\maven folder, dovrai creare una variabile di ambiente denominata M2_HOME con valore C:\maven\apache-maven-3.6.0. Quindi aggiungi M2_HOME\bin al percorso e salva l’impostazione.

## Crea progetto Maven utilizzando AEM Project Archetype

* Crea una cartella denominata **cloudmanager**(è possibile assegnargli un nome qualsiasi) nell&#39;unità c
* Apri la finestra del prompt dei comandi e passa a **c:\cloudmanager**
* Copia e incolla il contenuto del [file di testo](assets/creating-maven-project.txt) nella finestra del prompt dei comandi. Potrebbe essere necessario modificare il DarchetypeVersion=30 a seconda del [versione più recente](https://github.com/adobe/aem-project-archetype/releases). L&#39;ultima versione era di 30 al momento della stesura di questo articolo.
* Esegui il comando premendo Invio chiave.Se tutto funziona correttamente, dovresti visualizzare il messaggio di completamento della compilazione.





