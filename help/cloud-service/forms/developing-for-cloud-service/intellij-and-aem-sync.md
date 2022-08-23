---
title: Configurazione di IntelliJ con lo strumento Repo
description: Prepara il tuo IntelliJ per la sincronizzazione con AEM'istanza cloud ready
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8844
exl-id: 9a7ed792-ca0d-458f-b8dd-9129aba37df6
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 2%

---

# Installazione di Cygwin


Cygwin è un ambiente di programmazione e runtime compatibile con POSIX che viene eseguito in modo nativo su Microsoft Windows.
Installa [Cygwin](https://www.cygwin.com/). Ho installato in C:\cygwin64 folder
>[!NOTE]
> Assicurati di installare pacchetti zip, unzip, curl e rsync con l&#39;installazione di cygwin

Crea una cartella denominata adoberepo sotto la cartella c:\cloudmanager.

[Installare lo strumento repo].(https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo).Installing lo strumento repo non è altro che copiare il file repo e inserirlo nel tuo c:\cloudmanger\adoberepo folder.

Aggiungi quanto segue alla variabile di ambiente Path C:\cygwin64\bin;C:\CloudManager\adoberepo;

## Configurazione strumenti esterni

* Avvia IntelliJ
* Premere i tasti Ctrl+Alt+S per aprire la finestra delle impostazioni.
* Selezionare Strumenti->Strumenti esterni, quindi fare clic sul segno + e inserire quanto segue come mostrato nella schermata.
   ![rep](assets/repo.png)
* Assicurati di creare un gruppo denominato repo digitando &quot;repo&quot; nel campo a discesa Gruppo e tutti i comandi creati appartengono al gruppo **repo** gruppo


**Comando Put**
**Programma**: C:\cygwin64\bin\bash
**Argomenti**: -l C:\CloudManager\adoberepo\repo put -f \$FilePath\$
**Cavo da lavoro**: \$ProjectFileDir\$
![comando](assets/put-command.png)

**Ottieni comando**
**Programma**: C:\cygwin64\bin\bash
**Argomenti**: -l C:\CloudManager\adoberepo\repo get -f \$FilePath\$
**Cavo da lavoro**: \$ProjectFileDir\$
![get-command](assets/get-command.png)

**Comando di stato**
**Programma**: C:\cygwin64\bin\bash
**Argomenti**: -l C:\CloudManager\adoberepo\repo st -f \$FilePath\$
**Cavo da lavoro**: \$ProjectFileDir\$
![status-comando](assets/status-command.png)

**Comando Diff**
**Programma**: C:\cygwin64\bin\bash
**Argomenti**: -l C:\CloudManager\adoberepo\repo diff -f $FilePath$
**Cavo da lavoro**: \$ProjectFileDir\$
![diff-command](assets/diff-command.png)

Estrai il file .repo da [repo.zip](assets/repo.zip) e inseriscilo nella cartella principale dei progetti AEM. (C:\CloudManager\aem-banking-application). Apri il file .repo e assicurati che il server e le impostazioni delle credenziali corrispondano al tuo ambiente.
Apri il file .gitignore e aggiungi quanto segue nella parte inferiore del file e salva le modifiche \# repo.repo

Seleziona qualsiasi progetto all’interno del progetto aem-banking-application come ui.content e fai clic con il pulsante destro del mouse; dovresti visualizzare l’opzione repo e, nell’opzione repo, vedrai i 4 comandi aggiunti in precedenza.

## Imposta istanza autore AEM

I seguenti passaggi possono essere seguiti per configurare rapidamente l&#39;istanza cloud ready sul sistema locale.
* [Scarica l&#39;SDK AEM più recente](https://experience.adobe.com/#/downloads/content/software-distribution/it/aemcloud.html)

* [Scarica l’ultimo aggiornamento di AEM Forms](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* Crea la seguente struttura di cartelle c:\aemformscs\aem-sdk\author

* Estrai il file aem-sdk-quickstart-xxxxxxx.jar dal file zip AEM SDK e inseriscilo nel c:\aemformscs\aem-sdk\author folder.Rename file jar in aem-author-p4502.jar

* Apri il prompt dei comandi e passa a c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui. In questo modo verrà avviata l&#39;installazione di AEM.
* Accedi utilizzando le credenziali di amministratore/amministratore
* Interrompi l&#39;istanza AEM
* Crea la seguente struttura di cartelle.C:\aemformscs\aem-sdk\author\crx-quickstart\install
* Copia aem-forms-addon-xxxxxx.far nella cartella di installazione
* Apri il prompt dei comandi e passa a c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui. Questo distribuirà il pacchetto add-on dei moduli nella tua istanza AEM.
