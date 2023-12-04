---
title: Configurazione di IntelliJ con lo strumento Repo
description: Preparare IntelliJ per la sincronizzazione con l'istanza AEM cloud ready
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8844
exl-id: 9a7ed792-ca0d-458f-b8dd-9129aba37df6
duration: 125
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 0%

---

# Installazione di Cygwin


Cygwin è un ambiente di programmazione e runtime compatibile con POSIX che viene eseguito in modalità nativa su Microsoft Windows.
Installa [Cygwin](https://www.cygwin.com/). Ho installato nella cartella C:\cygwin64
>[!NOTE]
> Assicurati di installare i pacchetti zip, unzip, curl e rsync con l’installazione di cygwin

Creare una cartella denominata adoberepo in c:\cloudmanager.

[Installare lo strumento repository](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo) L&#39;installazione dello strumento repo richiede solo la copia del file repo e il suo inserimento nella cartella c:\cloudmanger\adoberepo.

Aggiungere quanto segue alla variabile di ambiente Path C:\cygwin64\bin;C:\CloudManager\adoberepo;

## Configurare gli strumenti esterni

* Avvia IntelliJ
* Premi Ctrl+Alt+S per aprire la finestra delle impostazioni.
* Selezionare Strumenti->Strumenti esterni, quindi fare clic sul segno + e immettere quanto segue come mostrato nella schermata.
  ![rep](assets/repo.png)
* Assicurati di creare un gruppo denominato repo digitando &quot;repo&quot; nel campo a discesa Gruppo e tutti i comandi creati appartengono al **repository** gruppo


**Comando Put**
**Programma**: C:\cygwin64\bin\bash
**Argomenti**: -l C:\CloudManager\adoberepo\repo put -f \$FilePath\$
**Direzione di lavoro**: \$ProjectFileDir\$
![put-command](assets/put-command.png)

**Comando Get**
**Programma**: C:\cygwin64\bin\bash
**Argomenti**: -l C:\CloudManager\adoberepo\repo get -f \$FilePath\$
**Direzione di lavoro**: \$ProjectFileDir\$
![get-command](assets/get-command.png)

**Comando di stato**
**Programma**: C:\cygwin64\bin\bash
**Argomenti**: -l C:\CloudManager\adoberepo\repo st -f \$FilePath\$
**Direzione di lavoro**: \$ProjectFileDir\$
![status-command](assets/status-command.png)

**Comando Diff**
**Programma**: C:\cygwin64\bin\bash
**Argomenti**: -l C:\CloudManager\adoberepo\repo diff -f $FilePath$
**Direzione di lavoro**: \$ProjectFileDir\$
![diff-command](assets/diff-command.png)

Estrai il file .repo da [repo.zip](assets/repo.zip) e inseriscilo nella cartella principale dei progetti AEM. (C:\CloudManager\aem-banking-application) Apri il file .repo e accertati che il server e le impostazioni delle credenziali corrispondano al tuo ambiente.
Apri il file .gitignore e aggiungi quanto segue nella parte inferiore del file e salva le modifiche \# archivio .repo

Seleziona qualsiasi progetto all’interno del progetto aem-banking-application, ad esempio ui.content e fai clic con il pulsante destro del mouse; dovresti vedere l’opzione archivio e sotto l’opzione archivio visualizzerai i 4 comandi aggiunti in precedenza.

## Configura istanza autore AEM{#set-up-aem-author-instance}

Per configurare rapidamente l’istanza Cloud Ready sul sistema locale, segui i passaggi seguenti.
* [Scarica l’SDK AEM più recente](https://experience.adobe.com/#/downloads/content/software-distribution/it/aemcloud.html)

* [Scarica il più recente componente aggiuntivo di AEM Forms](https://experience.adobe.com/#/downloads/content/software-distribution/it/aemcloud.html)

* Crea la seguente struttura di cartelle c:\aemformscs\aem-sdk\author

* Estrai il file aem-sdk-quickstart-xxxxxxx.jar dal file zip dell’SDK dell’AEM e inseriscilo nella cartella c:\aemformscs\aem-sdk\author.Rinomina il file jar in aem-author-p4502.jar

* Apri il prompt dei comandi e passa a c:\aemformscs\aem-sdk\author immetti il seguente comando java -jar aem-author-p4502.jar -gui. In questo modo verrà avviata l’installazione dell’AEM.
* Accedi con le credenziali di amministratore/amministratore
* Interrompere l’istanza AEM
* Crea la seguente struttura di cartelle.C:\aemformscs\aem-sdk\author\crx-quickstart\install
* Copia aem-forms-addon-xxxxxx.far nella cartella di installazione
* Apri il prompt dei comandi e passa a c:\aemformscs\aem-sdk\author immetti il seguente comando java -jar aem-author-p4502.jar -gui. Questo distribuirà il pacchetto di componenti aggiuntivi per Forms nell’istanza AEM.

## Passaggi successivi

[Sincronizzare i moduli e i modelli AEM con il progetto AEM](./deploy-your-first-form.md)
