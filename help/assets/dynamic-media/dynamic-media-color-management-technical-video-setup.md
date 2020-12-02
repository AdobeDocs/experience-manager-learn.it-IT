---
title: Gestione del colore con AEM elementi multimediali dinamici
seo-title: Gestione del colore con AEM elementi multimediali dinamici
description: Questo video illustra la gestione dinamica del colore per i file multimediali e come possono essere utilizzati per fornire funzionalità di anteprima per la correzione del colore in  AEM Assets.
seo-description: Questo video illustra la gestione dinamica del colore per i file multimediali e come possono essere utilizzati per fornire funzionalità di anteprima per la correzione del colore in  AEM Assets.
uuid: dc14d067-11a2-4662-acfd-f9f6f1d738ee
discoiquuid: b2b9ccc9-96b5-4bea-9995-2e6b353c469d
sub-product: dynamic-media
feature: image-profiles, video-profiles
topics: images, videos, renditions, authoring, integrations, publishing, metadata
audience: developer, architect, administrator
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 13%

---


# Gestione del colore con AEM elemento multimediale dinamico{#understanding-color-management-with-aem-dynamic-media}

Questo video illustra la gestione dinamica del colore per i file multimediali e come possono essere utilizzati per fornire funzionalità di anteprima per la correzione del colore in  AEM Assets.

>[!VIDEO](https://video.tv.adobe.com/v/16792/?quality=9&learn=on)

>[!NOTE]
>
>[Abilitare il ](https://docs.adobe.com/docs/en/aem/6-0/administer/integration/dynamic-media/enabling-dynamic-media.html) supporto dinamico AEM utilizzare questa funzione.

Questa funzione è disponibile nelle versioni AEM 6.1 e 6.2 come Feature Pack.

## Modello XML per il nodo di configurazione Gestione colore {#xml-template-for-the-color-management-configuration-node}

Di seguito è riportato il modello XML per il nodo di configurazione Gestione colore. Questo modello XML può essere copiato nel progetto di sviluppo AEM e configurato con le configurazioni appropriate per il progetto.

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!--
    XML Node definition for: /etc/dam/imageserver/configuration/jcr:content/settings

 Adobe Docs

 * Image Server Configuration: https://docs.adobe.com/docs/en/aem/6-2/administer/content/dynamic-media/config-dynamic.html#Configuring%20Dynamic%20Media%20Image%20Settings

* Default Color Profile Configuration: https://docs.adobe.com/docs/en/aem/6-1/administer/content/dynamic-media/config-dynamic.html#Configuring%20the%20default%20color%20profiles

    iccprofileXXX values:
        Node name of color profile found at: /etc/dam/imageserver/profiles

    iccblackpointcompensation values:
        true | false

    iccdither values:
        true | false

    iccrenderintent values:
        0 for perceptual
        1 for relative colorimetric
        2 for saturation
        3 for absolute colorimetric

-->

<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
    xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:primaryType="nt:unstructured"

        bkgcolor="FFFFFF"
        defaultpix="300,300"
        defaultthumbpix="100,100"
        expiration="{Long}36000000"
        jpegquality="80"
        maxpix="2000,2000"
        resmode="SHARP2"
        resolution="72"
        thumbnailtime="[1%,11%,21%,31%,41%,51%,61%,71%,81%,91%]"
        iccprofilergb=""
        iccprofilecmyk=""
        iccprofilegray=""
        iccprofilesrcrgb=""
        iccprofilesrccmyk=""
        iccprofilesrcgray=""
        iccblackpointcompensation="{Boolean}true"
        iccdither="{Boolean}false"
        iccrenderintent="{Long}0"
/>
```

### Elenco dei profili colore  Adobe predefiniti elencati di seguito{#list-of-default-adobe-color-profiles-are-listed-below}

| Nome | Spazio colore | Descrizione |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB |  Adobe RGB (1998) |
| AppleRGB | RGB | Apple RGB |
| CIERGB | RGB | CIE RGB |
| CoatedFogra27 | CMYK | Coated FOGRA27 (ISO 12647-2:2004) |
| CoatedFogra39 | CMYK | Coated FOGRA39 (ISO 12647-2:2004) |
| CoatedGraCol | CMYK | GRACoL 2006 rivestito (ISO 12647-2:2004) |
| ColorMatchRGB | RGB | ColorMatch RGB |
| EuropeISOCoated | CMYK | Europe ISO Coated FOGRA27 |
| EuroscaleCoated | CMYK | Euroscale Coated v2 |
| EuroscaleUnprotected | CMYK | Euroscale non patinata v2 |
| JapanColorCoated | CMYK | Japan Color 2001 Coated |
| JapanColorNewspaper | CMYK | Japan Color 2002 Newspaper |
| JapanColorUnprotected | CMYK | Japan Color 2001 Non patinato |
| JapanColorWebCoated | CMYK | Japan Color 2003 Web Coated |
| JapanWebCoated | CMYK | Japan Web Coated (Ad) |
| NewsprintSNAP2007 | CMYK | Newsprint USA (SNAP 2007) |
| NTSC | RGB | NTSC (1953) |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | ProPhoto RGB |
| PS4Default | CMYK | CMYK predefinito per Photoshop 4 |
| PS5Default | CMYK | CMYK predefinito per Photoshop 5 |
| FogliettoCoated | CMYK | U.S. SheetFeed Coated v2 |
| FogliettoNon patinato | CMYK | U.S. SheetFeed non patinato v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | RGB sRGB | IEC61966-2.1 |
| Fogra29 non patinata | CMYK | FOGRA29 non patinata (ISO 12647-2:2004) |
| WebCoated | CMYK | Stati Uniti Web Coated (SWOP) v2 |
| WebCoatedFogra28 | CMYK | Web Coated FOGRA28 (ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | Carta SWOP 2006 con rivestimento Web 3 |
| WebCoatedGrade5 | CMYK | Carta SWOP 2006 con rivestimento Web |
| WebNon patinato | CMYK | U.S. Web non patinata v2 |
| WideGamutRGB | RGB | Ampia gamma RGB |

## Risorse aggiuntive{#additional-resources}

* [Configurazione della gestione del colore per elementi multimediali dinamici](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
