---
title: "Administracja usługi Menedżer StorSimple | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie zarządzania urządzeniem StorSimple przy użyciu usługi Menedżer StorSimple w klasycznym portalu Azure."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 2586582e-d85c-42e1-afb3-be734c1c0461
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: dcca578b2993c025e62f1eca7ecec0e62a092479
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-device"></a>Użyj usługi Menedżer StorSimple do administrowania urządzenia StorSimple
> [!NOTE]
> Klasyczny portal dla urządzenia StorSimple jest przestarzały. Menedżerowie urządzenia StorSimple zostanie automatycznie przełączona do nowego portalu Azure, zgodnie z harmonogramem wycofywanie. Otrzymasz wiadomość e-mail i powiadomienie portalu dla tego przeniesienia. Ten dokument również zostaną wkrótce wycofane. Aby wyświetlić wersję tego artykułu dla nowego portalu Azure, przejdź do [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-8000-manager-service-administration.md). Wszelkie pytania dotyczące przeniesienie, zobacz [— często zadawane pytania: przejść do portalu Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Omówienie
W tym artykule opisano interfejsie usługi Menedżer StorSimple, łącznie ze sposobem łączyć się z jej, różne opcje dostępne i łączy się do określonych przepływów pracy, które mogą być wykonywane za pośrednictwem tego interfejsu użytkownika. W tych wskazówkach ma zastosowanie do obu; fizyczne StorSimple oraz urządzenia wirtualnego.

Po przeczytaniu tego artykułu, dowiesz się:

* Połączyć się z usługą Menedżer StorSimple
* Przejdź do interfejsu użytkownika Menedżera StorSimple
* Administrowanie urządzenia StorSimple za pośrednictwem usługi Menedżer StorSimple

## <a name="connect-to-storsimple-manager-service"></a>Połączyć się z usługą Menedżer StorSimple
Usługę Menedżer StorSimple działa na platformie Microsoft Azure i łączy się wieloma urządzeniami StorSimple. Zarządzać tymi urządzeniami za pomocą centralnej portalu klasycznego Microsoft Azure działającego w przeglądarce. Aby połączyć się z usługą Menedżer StorSimple, wykonaj następujące czynności.

#### <a name="to-connect-to-the-service"></a>Aby połączyć się z usługą
1. Przejdź do [https://manage.windowsazure.com/](https://manage.windowsazure.com/).
2. Przy użyciu poświadczeń konta Microsoft, zaloguj się do klasycznego portalu Microsoft Azure (znajdujące się w prawym górnym rogu okienka).
3. Przewiń w dół w okienku nawigacji po lewej stronie do uzyskania dostępu do usługi Menedżer StorSimple.

## <a name="navigate-storsimple-manager-service-ui"></a>Przejdź do usługi Menedżer StorSimple interfejsu użytkownika
W poniższej tabeli przedstawiono hierarchii nawigacji na potrzeby usługi Menedżer StorSimple interfejsu użytkownika.

* **Menedżer StorSimple** strony docelowej przejście do strony poziomu usług interfejsu użytkownika mające zastosowanie do wszystkich urządzeń w ramach usługi.
* **Urządzenia** stronie umożliwia przejście do strony interfejsu użytkownika urządzenia — poziomu mające zastosowanie do określonego urządzenia.
* **Kontenery woluminów** stronie umożliwia przejście do strony woluminu, który zawiera wszystkie woluminy, które są skojarzone z urządzeniem.

#### <a name="storsimple-manager-service-navigational-hierarchy"></a>Hierarchia nawigacji usługi Menedżer StorSimple
| Strona początkowa | Strony poziomu usługi | Stron na poziomie urządzenia | Stron na poziomie urządzenia |
| --- | --- | --- | --- |
| Usługa StorSimple Manager |Pulpit nawigacyjny usług |Pulpit nawigacyjny urządzenia | |
| Urządzenia → |Monitorowanie | | |
| Katalog kopii zapasowej |Containers→ woluminu |Woluminy | |
| Konfigurowanie (usługi) |Zasady tworzenia kopii zapasowej | | |
| Zadania |Skonfiguruj (na urządzenie) | | |
| Alerty |Konserwacji | | |

![Zobacz film](./media/storsimple-manager-service-administration/Video_icon.png) **Zobacz film**

Aby obejrzeć film wideo, który przeprowadzi Cię przez interfejs użytkownika usługi Menedżer StorSimple, kliknij przycisk [tutaj](https://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/).

## <a name="administer-storsimple-device-using-storsimple-manager-service"></a>Administrowanie urządzenia StorSimple przy użyciu usługi Menedżer StorSimple
W poniższej tabeli przedstawiono podsumowanie typowych zadań zarządzania i złożonych przepływów pracy, które mogą być wykonywane w ramach usługi Menedżer StorSimple interfejsu użytkownika. Te zadania są zorganizowane w zależności od strony interfejsu użytkownika, na których są inicjowane.

Aby uzyskać więcej informacji na temat każdego przepływu pracy kliknij odpowiednią procedurę w tabeli.

#### <a name="storsimple-manager-workflows"></a>Menedżer StorSimple przepływy pracy
| Jeśli chcesz to zrobić... | Przejdź do strony interfejsu użytkownika... | Użyj tej procedury. |
| --- | --- | --- |
| Tworzenie usługi</br>Usuwanie usługi</br>Pobierz klucz rejestracji usługi</br>Wygeneruj ponownie klucz rejestracji usługi |Usługa StorSimple Manager |[Wdrażanie usługi Menedżer StorSimple](storsimple-manage-service.md) |
| Zmiana klucza szyfrowania danych usługi</br>Sprawdź dzienniki operacji |Pulpit nawigacyjny → usługi Menedżer StorSimple |[Pulpit nawigacyjny usługi Menedżer StorSimple](storsimple-service-dashboard.md) |
| Dezaktywować urządzenie</br>Usuwanie urządzenia |Menedżer StorSimple, urządzenia usługi → |[Dezaktywuj lub usuń urządzenie](storsimple-deactivate-and-delete-device.md) |
| Dowiedz się więcej o pracy awaryjnej odzyskiwania i urządzenia po awarii</br>Tryb failover urządzenia fizycznego</br>Urządzenie wirtualne w tryb failover</br>Odzyskiwanie po awarii ciągłości biznesowej (BCDR) |Menedżer StorSimple, urządzenia usługi → |[Tryb failover i odzyskiwania po awarii dla urządzenia StorSimple](storsimple-device-failover-disaster-recovery.md) |
| Kopie zapasowe woluminu z listy</br>Wybierz zestaw kopii zapasowych</br>Usuń zestaw kopii zapasowych |Katalog kopii zapasowej → usługi Menedżer StorSimple |[Zarządzanie kopiami zapasowymi](storsimple-manage-backup-catalog.md) |
| Klonowanie woluminu |Katalog kopii zapasowej → usługi Menedżer StorSimple |[Klonowanie woluminu](storsimple-clone-volume.md) |
| Przywróć zestaw kopii zapasowych |Katalog kopii zapasowej → usługi Menedżer StorSimple |[Przywróć zestaw kopii zapasowych](storsimple-restore-from-backup-set.md) |
| Dotyczących kont magazynu</br>Dodawanie konta magazynu</br>Edytuj konto magazynu</br>Usuwanie konta magazynu</br>Rotacją kluczy kont magazynu |Skonfiguruj → usługi Menedżer StorSimple |[Zarządzanie kontami magazynu](storsimple-manage-storage-accounts.md) |
| Informacje o szablonach przepustowości</br>Dodaj szablon przepustowości</br>Edytuj szablon przepustowości</br>Usuń szablon przepustowości</br>Użyj domyślnego szablonu przepustowości</br>Tworzenie szablonu przepustowości całodzienne, która rozpoczyna się od określonego czasu |Skonfiguruj → usługi Menedżer StorSimple |[Zarządzanie szablonami przepustowości](storsimple-manage-bandwidth-templates.md) |
| Informacje o rekordach kontroli dostępu</br>Utwórz rekord kontroli dostępu</br>Edytuj rekord kontroli dostępu</br>Usuń rekord kontroli dostępu |Skonfiguruj → usługi Menedżer StorSimple |[Zarządzanie rekordami kontroli dostępu](storsimple-manage-acrs.md) |
| Wyświetlanie szczegółów zadania</br>Anulowanie zadania |Zadania usługi → Menedżer StorSimple |[Zarządzanie zadaniami](storsimple-manage-jobs.md) |
| Otrzymywanie powiadomień o alertach</br>Zarządzanie alertami</br>Alerty dotyczące weryfikacji |Alerty usługi → Menedżer StorSimple |[Wyświetl alerty i zarządzaj nimi StorSimple](storsimple-manage-alerts.md) |
| Wyświetl połączone inicjatorów</br>Znaleźć numer seryjny urządzenia</br>Znajdź element docelowy IQN |Menedżer StorSimple usługi → urządzeń → pulpitu nawigacyjnego |[Pulpit nawigacyjny urządzenia StorSimple](storsimple-device-dashboard.md) |
| Tworzenie wykresów monitorowania |Menedżer StorSimple usługi → urządzeń monitorowanie → |[Monitorowanie urządzenia StorSimple](storsimple-monitor-device.md) |
| Dodaj kontener woluminów</br>Modyfikowanie kontenera woluminów</br>Usunięcie kontenera woluminów |Kontenery woluminów → urządzenia StorSimple Manager service → |[Zarządzanie kontenerami woluminów](storsimple-manage-volume-containers.md) |
| Dodawanie woluminu</br>Modyfikowanie woluminu</br>Przełącz do trybu offline woluminu</br>Usuwanie woluminu</br>Monitor woluminu |Woluminy StorSimple Menedżera usługi → urządzeń → kontenery woluminów → |[Zarządzanie woluminami](storsimple-manage-volumes.md) |
| Zmodyfikuj ustawienia urządzenia</br>Zmodyfikuj ustawienia czasu</br>Zmodyfikuj ustawienia DNS.md</br>Skonfiguruj interfejsy sieciowe |Menedżer StorSimple usługi → urządzeń skonfigurować → |[Modyfikowanie konfiguracji urządzenia dla urządzenia StorSimple](storsimple-modify-device-config.md) |
| Wyświetl ustawienia serwera proxy sieci web |Menedżer StorSimple usługi → urządzeń skonfigurować → |[Skonfiguruj serwer proxy sieci web dla urządzenia](storsimple-configure-web-proxy.md) |
| Zmodyfikuj hasło administratora urządzenia</br>Zmodyfikuj hasło programu StorSimple Snapshot Manager |Menedżer StorSimple usługi → urządzeń skonfigurować → |[Zmienianie haseł usługi StorSimple](storsimple-change-passwords.md) |
| Konfigurowanie zdalnego zarządzania |Menedżer StorSimple usługi → urządzeń skonfigurować → |[Zdalne nawiązywanie połączenia urządzenia StorSimple](storsimple-remote-connect.md) |
| Konfigurowanie ustawień alertów |Menedżer StorSimple usługi → urządzeń skonfigurować → |[Wyświetl alerty i zarządzaj nimi StorSimple](storsimple-manage-alerts.md) |
| Konfigurowanie protokołu CHAP dla urządzenia StorSimple |Menedżer StorSimple usługi → urządzeń skonfigurować → |[Konfigurowanie protokołu CHAP dla urządzenia StorSimple](storsimple-configure-chap.md) |
| Dodawanie zasad kopii zapasowych</br>Dodaj lub zmodyfikuj harmonogram</br>Usuń zasady tworzenia kopii zapasowej</br>Wykonaj kopię zapasową ręczne</br>Tworzenie niestandardowych zasad tworzenia kopii zapasowej z wieloma woluminami i harmonogramów |Zasady tworzenia kopii zapasowej → urządzeń → usługi Menedżer StorSimple |[Zarządzanie zasadami tworzenia kopii zapasowych](storsimple-manage-backup-policies.md) |
| Zatrzymaj kontrolery urządzeń</br>Uruchom ponownie kontrolery urządzeń</br>Zamknij kontrolery urządzeń</br>Resetowanie urządzenia do ustawień fabrycznych</br>(Powyżej są do lokalnego urządzenia) |Menedżer StorSimple usługi → urządzeń → konserwacji |[Zarządzanie kontrolera urządzenia StorSimple](storsimple-manage-device-controller.md) |
| Dowiedz się więcej o składniki sprzętowe StorSimple</br>Monitor stanu sprzętu</br>(Powyżej są do lokalnego urządzenia) |Menedżer StorSimple usługi → urządzeń → konserwacji |[Monitor składników sprzętowych](storsimple-monitor-hardware-status.md) |
| Utwórz pakiet pomocy technicznej |Menedżer StorSimple usługi → urządzeń → konserwacji |[Tworzenie i zarządzanie nimi pakietu obsługi](storsimple-create-manage-support-package.md) |
| Instalacja aktualizacji oprogramowania |Menedżer StorSimple usługi → urządzeń → konserwacji |[Aktualizowanie urządzenia](storsimple-update-device.md) |

## <a name="next-steps"></a>Następne kroki
Jeśli występują problemy z codziennych operacji urządzenia StorSimple lub za pomocą dowolnego z jego składniki sprzętowe, zobacz:

* [Rozwiązywanie problemów z urządzeniem operacyjne](storsimple-troubleshoot-operational-device.md)
* [Użyj StorSimple monitorowania wskaźników](storsimple-monitoring-indicators.md)

Jeśli nie można rozwiązać problemy, należy utworzyć żądania obsługi dotyczą [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md).

