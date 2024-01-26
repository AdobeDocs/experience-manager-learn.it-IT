---
title: Installazione di AEM Forms su Linux
description: Scopri come installare librerie a 32 bit per far funzionare AEM Forms sull’installazione di Linux.
feature: Adaptive Forms
type: Tutorial
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-7593
exl-id: b9809561-e9bd-4c67-bc18-5cab3e4aa138
last-substantial-update: 2019-06-09T00:00:00Z
duration: 245
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '938'
ht-degree: 0%

---

# Installazione della versione a 32 bit delle librerie condivise

Quando AEM FORMS OSGi o AEM Forms j2EE vengono distribuiti su Linux, è necessario assicurarsi che siano installate e disponibili le versioni a 32 bit di un set di librerie condivise.  Le descrizioni provengono dai pacchetti stessi.

* expat (libreria C del parser XML orientato allo streaming per l&#39;analisi dell&#39;XML, scritta da James Clark)
* fontconfig (Libreria di configurazione e personalizzazione dei caratteri progettata per individuare i caratteri all&#39;interno del sistema e selezionarli in base ai requisiti specificati dalle applicazioni)
* freetype (motore di rendering dei caratteri), sviluppato per fornire supporto avanzato per i caratteri per una varietà di piattaforme e ambienti. Può aprire e gestire file di font, oltre a caricare, suggerire ed eseguire il rendering dei singoli glifi in modo efficiente. Non è un server font o una libreria di rendering di testo completa)
* glibc (librerie core per il sistema GNU e sistemi GNU/Linux, così come molti altri sistemi che utilizzano Linux come kernel)
* libcurl (libreria di trasferimento URL lato client)
* libICE (Inter-Client Exchange Library)
* libicu (libreria che fornisce supporto Unicode e locale completo e affidabile - Componenti internazionali per Unicode). Sono necessarie entrambe le edizioni a 64 bit e a 32 bit di questa libreria
* libSM (libreria X11 Session Management)
* libuuid (libreria di identificatori univoci universali compatibile con DCE, utilizzata per generare identificatori univoci per oggetti che possono essere accessibili oltre il sistema locale)
* libX11 (libreria lato client X11)
* libXau (protocollo di autorizzazione X11 - utile per limitare l&#39;accesso dei clienti al display)
* libxcb (associazione linguaggio C del protocollo X - XCB)
* libXext (libreria per estensioni comuni al protocollo X11)
* libXinerama (estensione X11 che fornisce supporto per l&#39;estensione di un desktop su più schermi. Il nome è un gioco di parole su Cinerama, un formato di film widescreen che utilizzava più proiettori. libXinerama è la libreria che si interfaccia con l&#39;estensione RandR)
* libXrandr (l&#39;estensione Xinerama è oggi in gran parte obsoleta ed è stata sostituita dall&#39;estensione RandR)
* libXrender (libreria client di X Rendering Extension) nss-softokn-freebl (Libreria libera per Servizi di sicurezza di rete)
* zlib (libreria di compressione dati generica, priva di brevetto e senza perdita di dati)

Da Red Hat Enterprise Linux 6 in poi, l&#39;edizione a 32 bit di una libreria avrà l&#39;estensione .686, mentre l&#39;edizione a 64 bit avrà .x86_64. Esempio: expat.i686. Prima di RHEL 6, le edizioni a 32 bit avevano l&#39;estensione .i386. Prima di installare le edizioni a 32 bit, accertarsi che siano installate le edizioni a 64 bit più recenti. Se l’edizione a 64 bit di una libreria è precedente alla versione a 32 bit in fase di installazione, viene visualizzato un errore simile al seguente:

0mErrore: Versioni multilib protette: libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mErrore: sono stati rilevati problemi di versione di più librerie.]

## Prima installazione

Su Red Hat Enterprise Linux, utilizzare YellowDog Update Modifier (YUM) per eseguire l&#39;installazione, come illustrato di seguito:

1. installazione yum expat.i686
2. yum install fontconfig.i686
3. installazione yum freetype.i686
4. installazione yum glibc.i686
5. installazione yum libcurl.i686
6. yum installare libICE.i686
7. installazione yum libicu.i686
8. yum install libicu
9. installazione yum libSM.i686
10. yum installare libuuid.i686
11. installazione yum libX11.i686
12. yum installare libXau.i686
13. installazione yum libxcb.i686
14. installazione yum libXext.i686
15. yum installare libXinerama.i686
16. yum installare libXrandr.i686
17. installazione yum libXrender.i686
18. installazione yum nss-softokn-freebl.i686
19. installazione yum zlib.i686

## Collegamenti simbolici

Inoltre, è necessario creare i symlink libcurl.so, libcrypto.so e libssl.so che puntano rispettivamente alle versioni a 32 bit delle librerie libcurl, libcrypto e libssl. È possibile trovare i file in /usr/lib/ ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## Aggiornamenti al sistema esistente

Durante l’aggiornamento possono verificarsi conflitti tra le architetture x86_64 e i686, ad esempio: Errore: Errore di controllo della transazione: file /lib/ld-2.28.so dall’installazione di glibc-2.28-72.el8.i686 è in conflitto con il file del pacchetto glibc32-2.28-42.1.el8.x86_64

Se riscontri questo problema, disinstalla prima il pacchetto che causa l’infrazione, come in questo caso: rimuovi glibc32-2.28-42.1.el8.x86_64

Detto e fatto, vuoi che le versioni x86_64 e i686 siano esattamente le stesse, come ad esempio da questo output al comando: yum info glibc

Ultimo controllo scadenza metadati: 0:41:33 fa su Sab 18 Gen 2020 11:37:8:00 EST.
Pacchetti installati Nome : glibc Versione : 2.28 Versione : 72.el8 Architettura : i686 Dimensione : 13 M Origine : glibc-2.28-72.el8.src.rpm Archivio : @System Da repository : BaseOS Riepilogo : Le librerie GNU libc URL : http://www.gnu.org/software/glibc/ Licenza : LGPLv2+ e LGPLv2+ con eccezioni e GPLv2+ e GPLv2+ con eccezioni e BSD e Inner-Net e ISC e dominio pubblico e GFDL Descrizione: Il pacchetto glibc contiene librerie standard utilizzate da più programmi su di rete. Per risparmiare spazio su disco e : memoria, nonché per semplificare l’aggiornamento, il codice di sistema comune è : mantenuto in un’unica posizione e condiviso tra i programmi. Questo particolare pacchetto : contiene i più importanti set di librerie condivise: la libreria standard C : e la libreria matematica standard. Senza queste due librerie, un : sistema Linux non funzionerà.

Nome : glibc Versione : 2.28 Versione : 72.el8 Architettura : x86_64 Dimensione : 15 M Origine : glibc-2.28-72.el8.src.rpm Archivio : @System Da repository : BaseOS Riepilogo : Le librerie GNU libc URL : http://www.gnu.org/software/glibc/ Licenza : LGPLv2+ e LGPLv2+ con eccezioni e GPLv2+ e GPLv2+ con eccezioni e BSD e Inner-Net e ISC e pubblico dominio e GFDL Descrizione: il pacchetto glibc contiene librerie standard utilizzate da : più programmi sul sistema. Per risparmiare spazio su disco e : memoria, nonché per semplificare l’aggiornamento, il codice di sistema comune è : mantenuto in un’unica posizione e condiviso tra i programmi. Questo particolare pacchetto : contiene i più importanti set di librerie condivise: la libreria standard C : e la libreria matematica standard. Senza queste due librerie, un : sistema Linux non funzionerà.

## Alcuni utili comandi yum

yum list install yum search [part_of_package_name]
yum cosa fornisce [nome_pacchetto]
installazione yum [nome_pacchetto]
reinstallazione yum [nome_pacchetto]
info yum [nome_pacchetto]
yum deplist [nome_pacchetto]
rimuovi yum [nome_pacchetto]
yum check-update [nome_pacchetto]
aggiornamento yum [nome_pacchetto]
