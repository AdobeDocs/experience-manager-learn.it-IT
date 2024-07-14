---
title: Generare rapporti sui campi dati del modulo inviati tramite Adobe Analytics
description: Integrare AEM Forms CS con Adobe Analytics per creare rapporti sui campi dati dei moduli
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 9982e041-fff7-4be6-91c9-e322d2fd3e01
duration: 41
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 1%

---

# Definire la regola

Nella proprietà Tags sono state create 2 nuove [regole](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html) (**Errore di convalida campo e FormSubmit**).

![modulo adattivo](assets/rules.png)


## Errore di convalida campo

La regola **Errore di convalida campo** viene attivata ogni volta che si verifica un errore di convalida nel campo del modulo adattivo. Ad esempio, nel modulo, se il numero di telefono o l’e-mail non sono nel formato previsto, viene visualizzato un messaggio di errore di convalida.

La regola Errore di convalida campo è configurata impostando l&#39;evento su _**Adobe Experience Manager Forms-Error**_ come mostrato nella schermata



![residenza-stato-richiedente](assets/field_validation_error_rule.png)

Adobe Analytics - Imposta variabili è configurato come segue

![imposta azione](assets/field_validation_action_rule.png)

## Regola invio modulo

La regola di invio del modulo viene attivata ogni volta che un modulo adattivo viene inviato correttamente.

La regola di invio del modulo è configurata utilizzando l&#39;evento _**Adobe Experience Manager Forms - Submit**_

![regola-invio-modulo](assets/form-submit-rule.png)

Nella regola Form Submit, il valore dell&#39;elemento dati _**ApplicantsStateOfResidence**_ è mappato a prop5 e il valore dell&#39;elemento dati FormTitle è mappato a prop8.

Adobe Analytics - Imposta variabili è configurato come segue.
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

Quando sei pronto a testare il codice dei tag,[pubblica le modifiche apportate ai tag](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) utilizzando il flusso di pubblicazione.

## Passaggi successivi

[Testare la soluzione](./test.md)