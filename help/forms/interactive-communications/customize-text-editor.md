---
title: Personalizzare l’editor di testo
description: Scopri come personalizzare l’editor di testo.
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
feature: Interactive Communication
last-substantial-update: 2023-04-19T00:00:00Z
jira: KT-13126
exl-id: e551ac8d-0bfc-4c94-b773-02ff9bba202e
duration: 139
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---

# Personalizzare l’editor di testo{#customize-text-editor}

## Panoramica {#overview}

È possibile personalizzare l’editor di testo utilizzato per creare frammenti di documento per aggiungere più font e dimensioni di font. Questi tipi di carattere includono i tipi di carattere inglesi e non inglesi, ad esempio giapponesi.

È possibile personalizzare le impostazioni seguenti per i tipi di carattere:

* Famiglia e dimensione font
* Proprietà quali altezza e spaziatura tra lettere
* Valori predefiniti per la famiglia e la dimensione del carattere, l&#39;altezza, la spaziatura tra lettere e il formato della data
* Rientri punto elenco

A questo scopo, devi:

1. [Personalizzare i font modificando il file tbxeditor-config.xml in CRX](#customizefonts)
1. [Aggiungere caratteri personalizzati al computer client](#addcustomfonts)

## Personalizzare i font modificando il file tbxeditor-config.xml in CRX {#customizefonts}

Per personalizzare i tipi di carattere modificando il file tbxeditor-config.xml, effettuare le seguenti operazioni:

1. Vai a `https://'[server]:[port]'/[ContextPath]/crx/de` e accedi come amministratore.
1. Nella cartella delle app, crea una cartella denominata config con un percorso/struttura simile a quello della cartella config, in libs/fd/cm/config, seguendo la procedura riportata di seguito:

   1. Fare clic con il pulsante destro del mouse sulla cartella degli elementi nel percorso seguente e selezionare **Sovrapponi nodo**:

      `/libs/fd/cm/config`

      ![Sovrapponi nodo](assets/overlay.png)

   1. Assicurati che la finestra di dialogo Sovrapponi nodo abbia i seguenti valori:

      **Percorso:** /libs/fd/cm/config

      **Posizione:** /apps/

      **Corrispondenza tipi di nodo:** selezionati

      ![Sovrapponi nodo](assets/overlay1.png)

   1. Fare clic su **OK**. La struttura di cartelle viene creata nella cartella delle app.

   1. Fare clic su **Salva tutto**.

1. Creare una copia del file tbxeditor-config.xml nella cartella di configurazione appena creata, attenendosi alla procedura descritta di seguito.

   1. Fare clic con il pulsante destro del mouse sul file tbxeditor-config.xml in libs/fd/cm/config e selezionare **Copia**.
   1. Fare clic con il pulsante destro del mouse sulla cartella seguente e selezionare **Incolla:**

      `apps/fd/cm/config`

   1. Per impostazione predefinita, il nome del file incollato è `copy of tbxeditor-config.xml.`. Rinominare il file in `tbxeditor-config.xml` e fare clic su **Salva tutto**.

1. Apri il file tbxeditor-config.xml in apps/fd/cm/config e apporta le modifiche necessarie.

   1. Fai doppio clic sul file tbxeditor-config.xml in apps/fd/cm/config. Viene aperto il file.

      ```xml
      <editorConfig>
         <bulletIndent>0.25in</bulletIndent>
      
         <defaultDateFormat>DD-MM-YYYY</defaultDateFormat>
      
         <fonts>
            <default>Times New Roman</default>
            <font>_sans</font>
            <font>_serif</font>
            <font>_typewriter</font>
            <font>Arial</font>
            <font>Courier</font>
            <font>Courier New</font>
            <font>Geneva</font>
            <font>Georgia</font>
            <font>Helvetica</font>
            <font>Tahoma</font>
            <font>Times New Roman</font>
            <font>Times</font>
            <font>Verdana</font>
         </fonts>
      
         <fontSizes>
            <default>12</default>
            <fontSize>8</fontSize>
            <fontSize>9</fontSize>
            <fontSize>10</fontSize>
            <fontSize>11</fontSize>
            <fontSize>12</fontSize>
            <fontSize>14</fontSize>
            <fontSize>16</fontSize>
            <fontSize>18</fontSize>
            <fontSize>20</fontSize>
            <fontSize>22</fontSize>
            <fontSize>24</fontSize>
            <fontSize>26</fontSize>
            <fontSize>28</fontSize>
            <fontSize>36</fontSize>
            <fontSize>48</fontSize>
            <fontSize>72</fontSize>
         </fontSizes>
      
         <lineHeights>
            <default>2</default>     
            <lineHeight>2</lineHeight>
            <lineHeight>3</lineHeight>
            <lineHeight>4</lineHeight>
            <lineHeight>5</lineHeight>
            <lineHeight>6</lineHeight>
            <lineHeight>7</lineHeight>
            <lineHeight>8</lineHeight>
            <lineHeight>9</lineHeight>
            <lineHeight>10</lineHeight>
            <lineHeight>11</lineHeight>
            <lineHeight>12</lineHeight>
            <lineHeight>13</lineHeight>
            <lineHeight>14</lineHeight>
            <lineHeight>15</lineHeight>
            <lineHeight>16</lineHeight>
         </lineHeights>
      
         <letterSpacings>
            <default>0</default>
            <letterSpacing>0</letterSpacing>
            <letterSpacing>1</letterSpacing>
            <letterSpacing>2</letterSpacing>
            <letterSpacing>3</letterSpacing>
            <letterSpacing>4</letterSpacing>
            <letterSpacing>5</letterSpacing>
            <letterSpacing>6</letterSpacing>
            <letterSpacing>7</letterSpacing>
            <letterSpacing>8</letterSpacing>
            <letterSpacing>9</letterSpacing>
            <letterSpacing>10</letterSpacing>
            <letterSpacing>11</letterSpacing>
            <letterSpacing>12</letterSpacing>
            <letterSpacing>13</letterSpacing>
            <letterSpacing>14</letterSpacing>
            <letterSpacing>15</letterSpacing>
            <letterSpacing>16</letterSpacing>
         </letterSpacings>
      </editorConfig>
      ```

   1. Apportare le modifiche necessarie nel file per modificare le impostazioni dei caratteri seguenti:

      * Aggiungi o rimuovi la famiglia e la dimensione del carattere
      * Proprietà quali altezza e spaziatura tra lettere
      * Valori predefiniti per la famiglia e la dimensione del carattere, l&#39;altezza, la spaziatura tra lettere e il formato della data
      * Rientri punto elenco

      Per aggiungere ad esempio un tipo di carattere giapponese denominato Sazanami Mincho Medium, è necessario inserire nel file XML la seguente voce: `<font>Sazanami Mincho Medium</font>`. È inoltre necessario che questo tipo di carattere sia installato nel computer client utilizzato per accedere e utilizzare la personalizzazione del tipo di carattere. Per ulteriori informazioni, vedere [Aggiungere caratteri personalizzati al computer client](#addcustomfonts).

      È inoltre possibile modificare le impostazioni predefinite per vari aspetti del testo e, rimuovendo le voci, rimuovere i caratteri dall&#39;editor di testo.

   1. Fare clic su **Salva tutto**.

## Aggiungere caratteri personalizzati al computer client {#addcustomfonts}

Quando si accede a un tipo di carattere nell&#39;editor di testo di comunicazioni interattive, è necessario che sia presente nel computer client utilizzato per accedere alla comunicazione interattiva. Per poter utilizzare un font personalizzato nell’editor di testo, devi innanzitutto installarlo sul computer client.

Per ulteriori informazioni sull&#39;installazione dei tipi di carattere, vedere:

* [Installa o disinstalla i tipi di carattere in Windows](https://windows.microsoft.com/en-us/windows-vista/install-or-uninstall-fonts)
* [Nozioni di base di Mac: Rubrica caratteri](https://support.apple.com/en-us/HT201749)

## Accedere alle personalizzazioni dei caratteri {#access-font-customizations}

Dopo aver apportato modifiche ai font nel file tbxeditor-config.xml in CRX e aver installato i font richiesti nel computer client utilizzato per accedere ad AEM Forms, le modifiche vengono visualizzate nell’editor di testo.

Ad esempio, il tipo di carattere Sazanami Mincho Medium aggiunto nella procedura [Personalizza i tipi di carattere modificando il file tbxeditor-config.xml in CRX](#customizefonts) viene visualizzato nell&#39;interfaccia utente dell&#39;editor di testo come segue:

![sazanamiminchointext](assets/sazanamiminchointext.png)

>[!NOTE]
>
>Per visualizzare il testo in giapponese, è innanzitutto necessario immettere il testo con caratteri giapponesi. L&#39;applicazione di un carattere giapponese personalizzato formatta il testo solo in un determinato modo. L’applicazione di un font giapponese personalizzato non modifica i caratteri inglesi o di altro tipo in caratteri giapponesi.
