---
title: Gestione del colore con AEM Dynamic Media
description: Questo video illustra Dynamic Media Color Management e come può essere utilizzato per fornire funzionalità di anteprima della correzione del colore in per AEM Assets.
feature: Image Profiles, Video Profiles
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Feature Video
exl-id: a733532b-db64-43f6-bc43-f7d422d5071a
duration: 274
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 11%

---

# Gestione del colore con AEM Dynamic Media{#understanding-color-management-with-aem-dynamic-media}

Questo video illustra Dynamic Media Color Management e come può essere utilizzato per fornire funzionalità di anteprima della correzione del colore in per AEM Assets.

>[!VIDEO](https://video.tv.adobe.com/v/41731?quality=12&learn=on&captions=ita)

>[!NOTE]
>
>[Abilita Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=it) in AEM per utilizzare questa funzione.

Questa funzione è disponibile per le versioni AEM 6.1 e 6.2 come Feature Pack.

## Modello XML per il nodo di configurazione Gestione colore {#xml-template-for-the-color-management-configuration-node}

Di seguito è riportato il modello XML per il nodo di configurazione Gestione colori. Questo modello XML può essere copiato nel progetto di sviluppo AEM e configurato con le configurazioni appropriate per il progetto.

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

### L’elenco dei profili colore predefiniti di Adobe è elencato di seguito {#list-of-default-adobe-color-profiles-are-listed-below}

| Nome | Spazio colore | Descrizione |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB (1998) |
| AppleRGB | RGB | Apple RGB |
| CIERGB | RGB | RGB CIE |
| Fogra27 rivestito | CMYK | FOGRA27 rivestito (ISO 12647-2:2004) |
| Fogra39 rivestito | CMYK | FOGRA39 rivestita (ISO 12647-2:2004) |
| GraCol rivestito | CMYK | GracoL 2006 rivestito (ISO 12647-2:2004) |
| ColorMatchRGB | RGB | RGB ColorMatch |
| EuropaISOCoato | CMYK | Europa FOGRA27 con rivestimento ISO |
| EuroscaleCoated | CMYK | Euroscale rivestita v2 |
| EuroscaleNon rivestite | CMYK | Euroscale senza rivestimento v2 |
| JapanColorCoated | CMYK | Rivestito colore Giappone 2001 |
| JapanColorNewspaper | CMYK | Giornale Japan Color 2002 |
| JapanColorUncoated | CMYK | Japan Color 2001 non patinata |
| JapanColorWebCoated | CMYK | Rivestimento Web Japan Color 2003 |
| JapanWebCoated | CMYK | Rivestito Web Giappone (Ad) |
| NewsprintSNAP2007 | CMYK | US Newsprint (SNAP 2007) |
| NTSC | RGB | NTSC, 1953 |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | RGB ProPhoto |
| PS4Default | CMYK | CMYK predefinito di Photoshop 4 |
| PS5Default | CMYK | CMYK predefinito di Photoshop 5 |
| SheetfedCoated | CMYK | U.S. Sheetfed Coated v2 |
| SheetfedUncoated | CMYK | U.S. Sheetfed non rivestito v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | RGB sRGB | IEC61966-2.1 |
| Fogra non rivestito29 | CMYK | FOGRA29 non rivestito (ISO 12647-2:2004) |
| WebCoated | CMYK | SWOP (U.S. Web Coated) v2 |
| WebCoatedFogra28 | CMYK | FOGRA28 rivestito con web (ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | Carta rivestita SWOP 2006 Grado 3 |
| WebCoatedGrade5 | CMYK | Carta rivestita SWOP 2006 Grado 5 |
| WebUncoated | CMYK | Web statunitense non rivestito v2 |
| WideGamutRGB | RGB | RGB ad ampia gamma |

## Risorse aggiuntive{#additional-resources}

* [Configurazione gestione colore elementi multimediali dinamici](https://helpx.adobe.com/it/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
