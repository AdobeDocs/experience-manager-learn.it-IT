---
title: Generare rapporti sui campi dati del modulo inviati tramite Adobe Analytics
description: Integrare AEM Forms CS con Adobe Analytics per creare rapporti sui campi dati dei moduli
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 439167be96959baea54f50a221c6d26f8fab78b2
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# Definire la regola

Nella proprietà Tags abbiamo creato 2 nuovi [regole](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html)(**Errore di convalida del campo e FormSubmit**).
![modulo adattivo](assets/rules.png)


## Errore di convalida campo

Il **Errore di convalida campo** la regola viene attivata ogni volta che si verifica un errore di convalida nel campo del modulo adattivo. Ad esempio, nel modulo, se il numero di telefono o l’e-mail non sono nel formato previsto, viene visualizzato un messaggio di errore di convalida.
La regola Errore di convalida del campo viene configurata impostando l’evento su _**Adobe Experience Manager Forms-Error**_ come mostrato nella schermata

![Stato-richiedente-residenza](assets/field_validation_error_rule.png)

Adobe Analytics - Imposta variabili è configurato come segue

![imposta azione](assets/field_validation_action_rule.png)

## Regola invio modulo

La regola di invio del modulo viene attivata ogni volta che un modulo adattivo viene inviato correttamente.
La regola di invio del modulo è configurata utilizzando _**Adobe Experience Manager Forms - Invia**_ evento

![form-submit-rule](assets/form-submit-rule.png)

Nella regola Invio modulo, il valore dell’elemento dati _**RicorrentiStatoDiResidenza**_ è mappato su prop5 e il valore dell&#39;elemento di dati FormTitle è mappato su prop8.

Adobe Analytics - Imposta variabili è configurato come segue.
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

Quando sei pronto a testare il codice dei tag,[pubblicare le modifiche apportate ai tag](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) mediante il flusso di pubblicazione.
