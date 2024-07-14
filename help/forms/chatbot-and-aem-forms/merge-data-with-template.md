---
title: Unire i dati con il modello XDP
description: Creare un documento PDF unendo i dati con il modello
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 6a865402-db3d-4e0e-81a0-a15dace6b7ab
duration: 15
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 0%

---

# Unire i dati con il modello XDP

Il passaggio successivo consiste nell&#39;unire i dati XML con il modello per generare il PDF. Questo PDF viene quindi inviato per le firme tramite Adobe Sign.

## Utilizzo di OutputService per generare il PDF

Per generare il PDF è stato utilizzato il metodo [generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) di OutputService.
Il PDF generato è stato quindi inviato per le firme utilizzando l’API REST di Adobe Sign.
