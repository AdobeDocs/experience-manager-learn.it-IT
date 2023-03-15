---
title: Report sui campi di dati del modulo inviati tramite Adobe Analytics
description: Integrare AEM Forms CS con Adobe Analytics per generare rapporti sui campi dei dati del modulo
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 672941b4047bb0cfe8c602e3b1ab75866c10216a
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# Definire la regola

Nella proprietà Tags abbiamo creato 2 nuovi [regole](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html) (**Errore di convalida dei campi e FormSubmit**).

![adattivo](assets/rules.png)


## Errore di convalida del campo

La **Errore di convalida del campo** la regola viene attivata ogni volta che si verifica un errore di convalida nel campo modulo adattivo. Ad esempio, nel modulo se il numero di telefono o l’e-mail non è nel formato previsto, viene visualizzato un messaggio di errore di convalida.

La regola di errore convalida del campo viene configurata impostando l’evento su _**Adobe Experience Manager Forms-Error**_ come mostrato nella schermata



![residenza del richiedente-Stato](assets/field_validation_error_rule.png)

Adobe Analytics - Imposta variabili è configurato come segue

![imposta azione](assets/field_validation_action_rule.png)

## Regola di invio modulo

La regola di invio del modulo viene attivata ogni volta che un modulo adattivo viene inviato correttamente.

La regola di invio del modulo è configurata utilizzando _**Adobe Experience Manager Forms - Invia**_ event

![form-submit-rule](assets/form-submit-rule.png)

Nella regola di invio del modulo, il valore dell’elemento dati _**RicorrenteStatoDiResidenza**_ è mappato a prop5 e il valore dell’elemento dati FormTitle è mappato a prop8.

Le variabili Adobe Analytics - Set sono configurate come segue.
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

Quando sei pronto per testare il codice dei tag,[pubblicare le modifiche apportate ai tag](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) utilizzo del flusso di pubblicazione.
