---
title: 'Utilizzo di test automatizzati con AEM Forms adattivo '
seo-title: 'Utilizzo di test automatizzati con AEM Forms adattivo '
description: Test automatico di Forms adattivo tramite Calvin SDK
seo-description: Test automatico di Forms adattivo tramite Calvin SDK
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
uuid: 3ad4e6d6-d3b1-4e4d-9169-847f74ba06be
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '460'
ht-degree: 0%

---


# Utilizzo di test automatizzati con AEM Forms adattivo {#using-automated-tests-with-aem-adaptive-forms}

Test automatico di Forms adattivo tramite Calvin SDK

Calvin SDK è un&#39;API di utilità per gli sviluppatori di Forms adattivi per testare l&#39;Forms adattivo. Calvin SDK è basato sul framework [di test di](https://docs.adobe.com/docs/en/aem/6-3/develop/ref/test-api/index.html)Hobbes.js. Calvin SDK è disponibile a partire  AEM Forms 6.3.

In questa esercitazione, creerete i seguenti elementi:

* Suite di test
* Test Suite contiene uno o più test case
* I casi di test contengono una o più azioni

## Guida introduttiva {#getting-started}

[Scaricate e installate le risorse tramite Package](assets/testingadaptiveformsusingcalvinsdk1.zip)ManagerIl pacchetto contiene script di esempio e diversi Forms adattivi.Questi Forms adattivi sono creati con  versione AEM Forms 6.3. È consigliabile creare nuovi moduli specifici della versione di  AEM Forms se si esegue il test su  AEM Forms 6.4 o versioni successive. Gli script di esempio mostrano diverse API Calvin SDK disponibili per il test di Forms adattivo. I passaggi generali per testare AEM Forms adattivo sono:

* Individuare il modulo da sottoporre a test
* Imposta il valore del campo
* Invio del modulo adattivo
* Verifica messaggi di errore

Gli script di esempio nel pacchetto illustrano tutte le azioni di cui sopra.
Esploriamo il codice di `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

Il codice riportato sopra crea una nuova suite di test.

* Il nome di TestSuite in questo caso è &#39; `Mortgage Form Test` &#39;.
* Fornito è il percorso assoluto in AEM al file js che contiene la suite di test.
* Il parametro register, se impostato su &#39; `true` &#39;, rende la suite di test disponibile nell&#39;interfaccia utente di test.

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
>Se state eseguendo il test di questa funzionalità su  AEM Forms 6.4 o versione successiva, create un nuovo modulo adattivo e utilizzatelo per eseguire il test. Non è consigliabile utilizzare il modulo adattivo fornito con il pacchetto.

I casi di test possono essere aggiunti alla suite di test da eseguire su un modulo adattivo.

* Per aggiungere un test case alla suite di test, utilizzate il `addTestCase` metodo dell&#39;oggetto TestSuite.
* Il `addTestCase` metodo utilizza un oggetto TestCase come parametro.
* Per creare TestCase, utilizzare il `hobs.TestCase(..)` metodo .
* Nota: Il primo parametro è il nome del Test Case che verrà visualizzato nell’interfaccia utente.
* Dopo aver creato un test case, potete aggiungere azioni al test case.
* Le azioni, incluse `navigateTo`, `asserts.isTrue` possono essere aggiunte come azioni al test case.

## Esecuzione dei test automatizzati {#running-the-automated-tests}

[](http://localhost:4502/libs/granite/testing/hobbes.html)Openthetestsuite: espandete la suite di test ed eseguite i test. Se tutto viene eseguito correttamente, verrà visualizzato il seguente output.

![calvinsdk](assets/calvinimage.png)

## Provate le suite di test di esempio {#try-out-the-sample-test-suites}

Nell’ambito del pacchetto di campioni sono disponibili tre suite di test supplementari. Potete provarli includendo i file appropriati nel file js.txt della clientlibrary come illustrato di seguito:

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
