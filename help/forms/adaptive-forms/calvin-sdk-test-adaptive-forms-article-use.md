---
title: Utilizzo di test automatizzati con AEM Adaptive Forms
description: Test automatizzato di Adaptive Forms tramite l’SDK Calvin
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 5a1364f3-e81c-4c92-8972-4fdc24aecab1
last-substantial-update: 2020-09-10T00:00:00Z
duration: 117
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 0%

---

# Utilizzo di test automatizzati con AEM Adaptive Forms {#using-automated-tests-with-aem-adaptive-forms}

Test automatizzato di Adaptive Forms tramite l’SDK Calvin

Calvin SDK è un’API di utilità per sviluppatori di Adaptive Forms per testare Adaptive Forms. L’SDK Calvin è basato su [Framework di test di Hobbes.js](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=it). L’SDK Calvin è disponibile da AEM Forms 6.3 in poi.

In questo tutorial, creerai quanto segue:

* Suite di test
* La suite di test conterrà uno o più test case
* I casi di test conterranno una o più azioni

## Guida introduttiva {#getting-started}

[Scaricare e installare le risorse tramite Gestione pacchetti](assets/testingadaptiveformsusingcalvinsdk1.zip)Il pacchetto contiene script di esempio e diversi Forms adattivi. Questi Forms adattivi sono generati con la versione 6.3 di AEM Forms. Si consiglia di creare nuovi moduli specifici per la versione di AEM Forms in uso, se si esegue il test su AEM Forms 6.4 o versione successiva. Gli script di esempio mostrano varie API dell’SDK Calvin disponibili per testare Adaptive Forms. I passaggi generali per testare l’AEM Adaptive Forms sono i seguenti:

* Passare al modulo da testare
* Imposta il valore del campo
* Inviare il modulo adattivo
* Verifica la presenza di messaggi di errore

Gli script di esempio nel pacchetto mostrano tutte le azioni di cui sopra.
Esaminiamo il codice di `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

Il codice riportato sopra crea una nuova suite di test.

* Il nome della suite di test in questo caso è &quot; `Mortgage Form Test` &quot;.
* Fornito è il percorso assoluto nel AEM del file js che contiene la suite di test.
* Parametro di registro se impostato su &#39; `true` &quot;, rende la suite di test disponibile nell’interfaccia utente di test.

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
>Se stai testando questa funzionalità su AEM Forms 6.4 o versioni successive, crea un nuovo modulo adattivo e utilizzalo per eseguire il test. L’utilizzo del modulo adattivo fornito con il pacchetto non è consigliato.

È possibile aggiungere dei test case alla suite di test da eseguire su un modulo adattivo.

* Per aggiungere un test case al gruppo di test, utilizza `addTestCase` metodo dell&#39;oggetto TestSuite.
* Il `addTestCase` Il metodo accetta un oggetto TestCase come parametro.
* Per creare TestCase, utilizza `hobs.TestCase(..)` metodo.
* Nota: il primo parametro è il nome del test case che verrà visualizzato nell’interfaccia utente.
* Dopo aver creato un test case è possibile aggiungere azioni al test case.
* Azioni, tra cui `navigateTo`, `asserts.isTrue` possono essere aggiunte come azioni al test case.

## Esecuzione dei test automatizzati {#running-the-automated-tests}

[Openthetestsuite](http://localhost:4502/libs/granite/testing/hobbes.html)Espandi la Suite di test ed esegui i test. Se tutto viene eseguito correttamente, verrà visualizzato il seguente output.

![calvinsdk](assets/calvinimage.png)

## Prova le suite di test di esempio {#try-out-the-sample-test-suites}

La confezione del campione comprende tre suite di test supplementari. Puoi provarli includendo i file appropriati nel file js.txt della libreria client, come illustrato di seguito:

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
