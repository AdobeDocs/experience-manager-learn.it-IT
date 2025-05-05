---
title: Utilizzo del sistema di stili in AEM Forms
description: Creare il progetto tema
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
exl-id: 4a02f494-ca0e-42d4-bbb9-6223ff8685e3
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# Verifica le modifiche

Crea un modulo adattivo basato sul modello **&quot;Blank with Core Components&quot;**. Trascinare e rilasciare 3 pulsanti sul modulo e assegnare loro l&#39;etichetta &quot;Corporate&quot;, &quot;Marketing&quot; e &quot;Default&quot;.
Assegnare le varianti di stile appropriate ai pulsanti Aziendale e Marketing selezionando il pennello di disegno come mostrato di seguito.

![stili](assets/marketing-variation.png)

Al terzo pulsante verrà applicato lo stile predefinito.

## Creare il progetto tema

Il passaggio successivo è quello di costruire il progetto tematico. Passa alla cartella principale del progetto tema ed esegui il comando _&#x200B;**npm run build**&#x200B;_ come mostrato nella schermata seguente.

![build-theme](assets/build-theme.png)

Una volta creato correttamente il progetto tematico, puoi testare le modifiche.

## Metodo semplice e veloce per testare il css

* Apri il file theme.css che si trova nella cartella Dist del progetto tema.Seleziona e copia l’intero contenuto del file.
* Visualizzare in anteprima il modulo creato nel passaggio precedente.
* Fai clic con il pulsante destro del mouse su uno dei pulsanti e seleziona Ispeziona per aprire la console per sviluppatori.
* Dalla console per sviluppatori, fai clic su theme.css per aprire theme.css
* Seleziona ed elimina l’intero contenuto di theme.css utilizzando il CTR-A e premi il pulsante Elimina.
* Copia e incolla il contenuto di theme.css creato nel passaggio precedente.
* I pulsanti devono essere aggiornati con gli stili appropriati, come illustrato di seguito.

![pulsanti finali](assets/final-state-buttons.png)

## Effettua il push delle modifiche

Se sei soddisfatto delle modifiche, puoi inviare le modifiche all&#39;istanza cloud utilizzando la [pipeline front-end](https://experienceleague.adobe.com/it/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/enable-frontend-pipeline-devops/create-frontend-pipeline)
