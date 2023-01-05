---
title: Punti di interruzione reattivi
description: Scopri come configurare nuovi punti di interruzione reattivi per AEM Editor pagina reattivo.
version: Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
kt: 11664
thumbnail: kt-11664.jpeg
source-git-commit: c82965636ddeef7dc165e0bea079c99f1a16e0ca
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---


# Punti di interruzione reattivi

Scopri come configurare nuovi punti di interruzione reattivi per AEM Editor pagina reattivo.

## Creare punti di interruzione CSS

Innanzitutto, crea punti di interruzione multimediali nel CSS AEM griglia reattiva a cui il sito AEM reattivo aderisce.

In `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` crea i punti di interruzione da utilizzare insieme all’emulatore mobile. Prendi nota del `max-width` per ogni punto di interruzione, in quanto questo mappa i punti di interruzione CSS ai punti di interruzione AEM dell’Editor pagina reattivo.

![Creare nuovi punti di interruzione reattivi](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## Personalizzare i punti di interruzione del modello

Apri `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` file e aggiornamento `cq:responsive/breakpoints` con le nuove definizioni dei nodi punto di interruzione. Ogni [Punto di interruzione CSS](#create-new-css-breakpoints) deve avere un nodo corrispondente sotto `breakpoints` con `width` impostato sul punto di interruzione CSS `max-width`.

![Personalizzare i punti di interruzione reattivi del modello](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## Creare emulatori

È necessario definire AEM emulatori che consentano agli autori di selezionare la visualizzazione reattiva da modificare nell’Editor pagina.

Crea nodi emulatori sotto `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

Esempio, `/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`. Copia un nodo emulatore di riferimento da `/libs/wcm/mobile/components/emulators` in CRXDE Lite e aggiorna la copia per accelerare la definizione del nodo.

![Creazione di nuovi emulatori](./assets/responsive-breakpoints/create-new-emulators.jpg)

## Crea gruppo di dispositivi

Raggruppa gli emulatori in [renderli disponibili in AEM Editor pagina](#update-the-templates-device-group).

Crea `/apps/settings/mobile/groups/<name of device group>` struttura del nodo sotto `/ui.apps/src/main/content/jcr_root`.

![Crea nuovo gruppo di dispositivi](./assets/responsive-breakpoints/create-new-device-group.jpg)

Crea un `.content.xml` in `/apps/settings/mobile/groups/<device group name>` e definisci i nuovi emulatori utilizzando un codice simile a quello riportato di seguito:

![Crea nuovo dispositivo](./assets/responsive-breakpoints/create-new-device.jpg)

## Aggiorna il gruppo di dispositivi del modello

Infine, mappa il gruppo di dispositivi nuovamente al modello di pagina in modo che gli emulatori siano disponibili nell’Editor pagina per le pagine create da questo modello.

Apri `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` e aggiorna il `cq:deviceGroups` per fare riferimento al nuovo gruppo mobile (ad esempio, `cq:deviceGroups="[mobile/groups/customdevices]"`)
