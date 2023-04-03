---
title: Informazioni sulla gestione del colore con AEM Dynamic Media
description: Questo video illustra la gestione del colore di Dynamic Media e come può essere utilizzato per fornire funzionalità di anteprima della correzione del colore in per AEM Assets.
feature: Image Profiles, Video Profiles
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: a733532b-db64-43f6-bc43-f7d422d5071a
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 17%

---

# Informazioni sulla gestione del colore con AEM Dynamic Media{#understanding-color-management-with-aem-dynamic-media}

Questo video illustra la gestione del colore di Dynamic Media e come può essere utilizzato per fornire funzionalità di anteprima della correzione del colore in per AEM Assets.

>[!VIDEO](https://video.tv.adobe.com/v/16792?quality=12&learn=on)

>[!NOTE]
>
>[Abilita Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=it) in AEM per utilizzare questa funzione.

Questa funzione è disponibile per le versioni AEM 6.1 e 6.2 come Feature Pack.

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

### Elenco dei profili di colore Adobe predefiniti elencati di seguito {#list-of-default-adobe-color-profiles-are-listed-below}

| Nome | Spazio colore | Descrizione |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB (1998) |
| AppleRGB | RGB | Apple RGB |
| CIERGB | RGB | RGB CIE |
| CoatedFogra27 | CMYK | Rivestito FOGRA27 (ISO 12647-2:2004) |
| CoatedFogra39 | CMYK | Rivestito FOGRA39 (ISO 12647-2:2004) |
| ColGraCoated | CMYK | GRACoL 2006 rivestito (ISO 12647-2:2004) |
| ColorMatchRGB | RGB | RGB ColorMatch |
| EuropaISOCoated | CMYK | Europa ISO rivestito FOGRA27 |
| EuroscaleCoated | CMYK | Euroscale Coated v2 |
| EuroscaleNon rivestito | CMYK | Euroscala non rivestita v2 |
| JapanColorCoated | CMYK | Giappone Colore 2001 Rivestito |
| JapanColorNewspaper | CMYK | Japan Color 2002 Newspaper |
| JapanColorNon patinato | CMYK | Giappone Colore 2001 non patinato |
| JapanColorWebCoated | CMYK | Japan Color 2003 Web Coated |
| JapanWebCoated | CMYK | Japan Web Coated (Ad) |
| NewsprintSNAP2007 | CMYK | Newsprint USA (SNAP 2007) |
| NTSC | RGB | NTSC (1953) |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | ProPhoto RGB |
| PS4Default | CMYK | CMYK predefinito Photoshop 4 |
| PS5Default | CMYK | CMYK predefinito Photoshop 5 |
| Foglietto | CMYK | U.S. Sheetfed Coated v2 |
| FogliameNon rivestito | CMYK | U.S. Sheetfeed non rivestito v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | sRGB di RGB | IEC61966-2.1 |
| UncoatedFogra29 | CMYK | FOGRA29 non rivestito (ISO 12647-2:2004) |
| WebCoated | CMYK | U.S. Web Coated (SWOP) v2 |
| WebCoatedFogra28 | CMYK | Web Coated FOGRA28 (ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | Carta SWOP 2006 di grado 3 rivestita sul web |
| WebCoatedGrade5 | CMYK | Carta SWOP 2006 di grado 5 rivestita sul web |
| WebNon rivestito | CMYK | U.S. Web non rivestito v2 |
| WideGamutRGB | RGB | RGB Wide Gamut |

## Risorse aggiuntive{#additional-resources}

* [Configurazione della gestione del colore Dynamic Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
