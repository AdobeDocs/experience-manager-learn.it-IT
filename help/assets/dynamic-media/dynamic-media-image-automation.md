---
title: Elaborazione batch Dynamic Media per trasparenza ed automazione dei contenuti
description: Scopri come utilizzare Dynamic Media in AEM per creare rappresentazioni virtuali, gestire la trasparenza e automatizzare l’elaborazione delle immagini per il riutilizzo scalabile dei contenuti.
feature: Image Profiles, Viewer Presets
topic: Content Management
role: User
level: Beginner, Intermediate, Experienced
doc-type: Feature Video
duration: 560
last-substantial-update: 2025-05-28T00:00:00Z
jira: KT-18197
source-git-commit: a509b7c41e6adb05e6c3b791ac2e99f635973322
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 6%

---


# Elaborazione batch Dynamic Media per trasparenza ed automazione dei contenuti

Scopri come utilizzare Dynamic Media in AEM per creare rappresentazioni virtuali, gestire la trasparenza e automatizzare l’elaborazione delle immagini per il riutilizzo scalabile dei contenuti.

>[!VIDEO](https://video.tv.adobe.com/v/3459589/?learn=on&enablevpops)


## Esempio di risorse Dynamic Media

Di seguito sono riportati alcuni esempi di risorse Dynamic Media e i relativi URL utilizzati nel video.

>[!BEGINTABS]

>[!TAB Esempi di trasparenza delle immagini]

Di seguito sono riportati gli URL di esempio di Dynamic Media Image Server utilizzati nel video.

| Anteprima | Descrizione | URL elementi multimediali dinamici |
|-----------|------------------|---------|
| ![Predefinito](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255){width="250"} | Predefiniti | [Collegamento](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255) |
| ![Composto con livello immagine di sfondo senza interruzioni](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | Composto con un livello di immagine di sfondo fluido | [Collegamento](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![Sfondo rosso](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | Sfondo rosso | [Collegamento](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![Ritagliato nel percorso ovale](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255){width="250"} | Ritagliato su tracciato ovale | [Collegamento](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255) |


>[!TAB Esempi di percorso immagine]

Di seguito sono riportati gli URL di esempio di Dynamic Media Image Server utilizzati nel video.

| Anteprima | Descrizione | URL elementi multimediali dinamici |
|-----------|------------------|---------|
| ![Normalizzato a 80 pixel (nessuna trasparenza)](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800){width="250"} | Normalizzato a 80 pixel (nessuna trasparenza){width="250"} | [Collegamento](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800) |
| ![Ritaglia nel percorso](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800){width="250"} | Ritaglia su percorso | [Collegamento](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800) |
| ![Ritaglia nel percorso](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800){width="250"} | Ritaglia su percorso | [Collegamento](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800) |
| ![Ritaglia su percorso e Ritaglia su percorso](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800){width="250"} | Ritaglia su tracciato e Ritaglia su tracciato | [Collegamento](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800) |
| ![Ritaglia in un altro percorso](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800){width="250"} | Ritaglia su un altro percorso | [Collegamento](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800) |
| ![Ritaglia in un altro percorso e crea uno sfondo rosso](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800){width="250"} | Ritaglia su un altro tracciato e crea uno sfondo rosso | [Collegamento](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800) |

>[!ENDTABS]


## API del server di immagini Dynamic Media

* [API di server e rendering immagini Dynamic Media](https://experienceleague.adobe.com/en/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/c-http-protocol-reference)
* [Anteprima snapshot Dynamic Media](https://snapshot.scene7.com/)
