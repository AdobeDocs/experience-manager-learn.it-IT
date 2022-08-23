---
title: Utilizzo di test automatizzati con AEM Forms adattivo
description: Test automatizzato di Forms adattivo utilizzando Calvin SDK
feature: Adaptive Forms
doc-type: article
activity: develop
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 5a1364f3-e81c-4c92-8972-4fdc24aecab1
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 2%

---

# Utilizzo di test automatizzati con AEM Forms adattivo {#using-automated-tests-with-aem-adaptive-forms}

Test automatizzato di Forms adattivo utilizzando Calvin SDK

Calvin SDK è un&#39;API di utilità per gli sviluppatori di Forms adattivi per testare Adaptive Forms. L’SDK di Calvin è basato su [Framework di test di Hobbes.js](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=it). Calvin SDK è disponibile con AEM Forms 6.3 e versioni successive.

In questa esercitazione verrà creato quanto segue:

* Suite di test
* La suite di test contiene uno o più casi di test
* I casi di test contengono una o più azioni

## Guida introduttiva {#getting-started}

[Scaricare e installare le risorse utilizzando Gestione pacchetti](assets/testingadaptiveformsusingcalvinsdk1.zip)Il pacchetto contiene script di esempio e diversi Adaptive Forms.Questi Adaptive Forms sono generati utilizzando la versione AEM Forms 6.3. È consigliabile creare nuovi moduli specifici per la versione di AEM Forms in uso se si esegue il test su AEM Forms 6.4 o versione successiva. Gli script di esempio mostrano le varie API dell’SDK Calvin disponibili per testare l’Adaptive Forms. I passaggi generali per il test AEM Forms adattivo sono i seguenti:

* Passa al modulo da testare
* Imposta il valore del campo
* Inviare il modulo adattivo
* Verifica messaggi di errore

Gli script di esempio nel pacchetto mostrano tutte le azioni di cui sopra.
Esploriamo il codice di `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

Il codice riportato sopra crea una nuova suite di test.

* Il nome di TestSuite in questo caso è &#39; `Mortgage Form Test` &quot;.
* Fornito è il percorso assoluto in AEM al file js che contiene la suite di test.
* Il parametro di registro quando è impostato su &#39; `true` &#39;, rende la suite di test disponibile nell&#39;interfaccia utente di test.

```javascript
.addTestCase(new hobs.TestCase("Calculate amount to borrow")
        // navigate to the mortgage form  which is to be tested
        .navigateTo("/content/forms/af/cal/mortgageform.html?wcmmode=disabled")
  .asserts.isTrue(function () {
            return calvin.isFormLoaded()
        })
```

>[!NOTE]
>
>Se stai sottoponendo a test questa funzionalità su AEM Forms 6.4 o versioni successive, crea un nuovo modulo adattivo e utilizzalo per eseguire i test.Non si consiglia di utilizzare il modulo adattivo fornito con il pacchetto.

È possibile aggiungere casi di test alla suite di test da eseguire su un modulo adattivo.

* Per aggiungere un test case alla suite di test, utilizza `addTestCase` metodo dell&#39;oggetto TestSuite.
* La `addTestCase` prende un oggetto TestCase come parametro.
* Per creare TestCase, utilizza la variabile `hobs.TestCase(..)` metodo .
* Nota: Il primo parametro è il nome del caso di test che verrà visualizzato nell’interfaccia utente.
* Dopo aver creato un test case, puoi aggiungere azioni al test case.
* Azioni `navigateTo`, `asserts.isTrue` può essere aggiunto come azioni al test case.

## Esecuzione dei test automatizzati {#running-the-automated-tests}

[Openthetestsuite](http://localhost:4502/libs/granite/testing/hobbes.html)Espandi la suite di test ed esegui i test. Se tutto viene eseguito correttamente, verrà visualizzato il seguente output.

![calvinsdk](assets/calvinimage.png)

## Prova le suite di test di esempio {#try-out-the-sample-test-suites}

Come parte del pacchetto di campioni ci sono tre ulteriori suite di test. Puoi provarli includendo i file appropriati nel file js.txt della clientlibrary come mostrato di seguito:

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
