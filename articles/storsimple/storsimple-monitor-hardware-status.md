---
title: "StorSimple składniki sprzętowe i stan | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak monitorować składniki sprzętowe urządzenia StorSimple przez usługę Menedżer StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 0d56a2ba-daf0-45ad-9610-8b8712dd5750
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 686a4faf37342e844f3aa0166d9311a438fa753a
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="use-the-storsimple-manager-service-to-monitor-hardware-components-and-status"></a>Korzystanie z usługi Menedżer StorSimple monitorem składniki sprzętowe i stanu
> [!NOTE]
> Klasyczny portal dla urządzenia StorSimple jest przestarzały. Menedżerowie urządzenia StorSimple zostanie automatycznie przełączona do nowego portalu Azure, zgodnie z harmonogramem wycofywanie. Otrzymasz wiadomość e-mail i powiadomienie portalu dla tego przeniesienia. Ten dokument również zostaną wkrótce wycofane. Aby wyświetlić wersję tego artykułu dla nowego portalu Azure, przejdź do [korzystania z usługi Menedżer StorSimple, monitorem składniki sprzętowe i stan](storsimple-8000-monitor-hardware-status.md). Wszelkie pytania dotyczące przeniesienie, zobacz [— często zadawane pytania: przejść do portalu Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Omówienie
W tym artykule opisano różne składniki fizyczne i logiczne w lokalnego urządzenia StorSimple. Wyjaśniono również sposób monitorować stan składnika urządzenia za pomocą **konserwacji** strony w usłudze Menedżer StorSimple. 

**Konserwacji** strony pokazuje stan sprzętu wszystkie składniki urządzenia StorSimple. 

Na liście składników 8100 istnieją trzy sekcje, które opisują:

* **Udostępniane składniki** — te nie są częścią kontrolerów, takie jak stacje dysków, obudowy, składniki PCM temperatury PCM napięcia wiersz i czujników bieżącego wiersza.
* **Składniki kontrolera 0** — składniki, które znajdują się na kontrolerze 0, takie jak kontroler, expander sygnatury dostępu Współdzielonego i łącznika czujnikami temperatury kontrolera i różnych interfejsów sieciowych.
* **Składniki kontrolera 1** — składników, które stanowią kontrolera 1 podobne do tych szczegółowe dla kontrolera 0.

Urządzenia 8600 ma dodatkowe składniki, które odpowiadają obudowa rozszerzony Bunch dla dysków (EBOD). Na liście składników istnieją pięciu sekcji. Z nich istnieją trzy sekcje, które zawierają składniki w obudowie podstawowego i są takie same, jak firma opisana dla 8100. Istnieją dwa dodatkowe sekcje dla obudowa EBOD opisujące:

* **Składniki udostępnione obudowy EBOD** — składniki znajdujących się w EBOD obudowy i PCM, które nie są częścią EBOD kontrolera.
* **Składniki EBOD kontrolera 0** — składniki, które znajdują się w obudowie EBOD 0, takich jak kontroler EBOD czujnikami temperatury expander i łącznika i kontrolera SAS.
* **Składniki 1 kontrolera EBOD** — składników, które stanowią EBOD obudowa 1 podobne do tych szczegółowych dla EBOD obudowa 0.

> [!NOTE]
> **W sekcji Stan sprzętu nie ma na stronie konserwacji dla urządzenia wirtualnego StorSimple (1100).**
> 
> 

## <a name="monitor-the-hardware-status"></a>Monitor stanu sprzętu
Wykonaj poniższe kroki, aby wyświetlić stan sprzętu składnika urządzenia:

1. Przejdź do **urządzeń**, wybierz określonego urządzenia StorSimple. Kliknij, aby przejść do menu poziomie urządzeń, a następnie kliknij przycisk **konserwacji**. 
2. Zlokalizuj **stan sprzętu** sekcji, a następnie wybierz jedną z dostępnych składników (jak opisano powyżej). Po prostu kliknij strzałkę poprzedzających etykiety składnika rozwiń listę i wyświetlanie stanu różnych składników urządzenia. Zobacz [listę składników szczegółowe dla podstawowego obudowa](#component-list-for-primary-enclosure-of-storsimple-device) i [listy szczegółowe części obudowy EBOD](#component-list-for-ebod-enclosure-of-storsimple-device).
3. Interpretować stan składnika przy użyciu następujących schemat kodowania kolorów:
   
   * **Zielone wyboru** — Denotes **dobra kondycja** lub **OK** składnika.
   * **Żółty** — oznacza składnik **ostrzeżenie** stanu.
   * **Czerwony wykrzyknik** — Denotes składnika, który **błąd** lub **wymaga uwagi** stanu.
   * **Białe czarny tekst** — określa składnik, który nie jest obecny.
4. Jeśli wystąpią składnik, który nie znajduje się w **dobra kondycja** stanu, skontaktuj się z Microsoft Support. Alerty są włączone na urządzeniu, otrzymasz alert e-mail. Jeśli trzeba wymienić składniki sprzętowe nie powiodło się, zobacz [wymiana składników sprzętowych StorSimple](storsimple-hardware-component-replacement.md).

## <a name="component-list-for-primary-enclosure-of-storsimple-device"></a>Lista składników dla podstawowego Obudowa urządzenia StorSimple
W poniższej tabeli przedstawiono składniki fizyczne i logiczne zawarte w głównej obudowa lokalnego urządzenia StorSimple.

| Składnik | Moduł | Typ | Lokalizacja | Pole (jednostkę FRU replaceable unit)? | Opis |
| --- | --- | --- | --- | --- | --- |
| Dysk w gnieździe [0-11] |Stacje dysków |Fizyczne |Udostępniona |Tak |Jeden wiersz jest widoczne dla każdego z dysków SSD i dysków HDD w obudowie podstawowego. |
| Czujnik temperatury otoczenia |Obudowa |Fizyczne |Udostępniona |Nie |Mierzy temperatury w obudowie. |
| Czujnik temperatury środkowa |Obudowa |Fizyczne |Udostępniona |Nie |Mierzy temperatury środkowa. |
| Alarmu |Obudowa |Fizyczne |Udostępniona |Nie |Wskazuje, czy w podsystemie alarmu w obudowie funkcjonalności. |
| Obudowa |Obudowa |Fizyczne |Udostępniona |Tak |Wskazuje na obecność podstawę. |
| Obudowa ustawienia |Obudowa |Fizyczne |Udostępniona |Nie |Odwołanie do panelu przedniego obudowy. |
| Wiersz czujnikami napięcia |PCM |Fizyczne |Udostępniona |Nie |Wiele czujnikami napięcia wiersz ma ich stanie wyświetlony, wskazuje, czy napięcia mierzoną w granicach tolerancji. |
| Czujniki bieżącego wiersza |PCM |Fizyczne |Udostępniona |Nie |Wiele czujników bieżący wiersz ma ich stanie wyświetlony, wskazuje, czy bieżący mierzoną w granicach tolerancji. |
| Czujnikami temperatury w PCM |PCM |Fizyczne |Udostępniona |Nie |Wiele czujnikami temperatury, takich jak Inlet i czujników punkt aktywny mają ich stanie wyświetlony, wskazującą, czy zmierzone temperatury mieści się w granicach. |
| Zasilacz [0-1] |PCM |Fizyczne |Udostępniona |Tak |Jeden wiersz jest widoczne dla każdego zasilacze w dwóch PCMs, znajduje się tyłu na urządzeniu. |
| Chłodzenia [0-1] |PCM |Fizyczne |Udostępniona |Tak |Dla każdego z czterech wentylatory znajdującej się w dwóch PCMs zobaczy jeden wiersz. |
| Baterii [0-1] |PCM |Fizyczne |Udostępniona |Tak |Jeden wiersz, jest widoczne dla każdego z modułów baterii kopii zapasowej, które są umieszczone w PCM. |
| Metis |Nie dotyczy |Logiczne |Udostępniona |Nie dotyczy |Wyświetla stan baterie: Określa, czy należy ładowania i zbliża się koniec życia. |
| Klaster |Nie dotyczy |Logiczne |Udostępniona |Nie dotyczy |Wyświetla stan klastra, który jest tworzony między dwa moduły zintegrowanego kontrolera. |
| Węzeł klastra |Nie dotyczy |Logiczne |Udostępniona |Nie dotyczy |Wskazuje stan kontrolera jako część klastra. |
| Kworum klastra |Nie dotyczy |Logiczne | |Nie dotyczy |Wskazuje na obecność dysku większość członkostwa w puli magazynów dysku twardego. |
| Miejsce na dysku twardego danych |Nie dotyczy |Logiczne |Udostępniona |Nie dotyczy |Miejsce do magazynowania służy do danych w puli magazynu na dysku twardym (HDD). |
| Miejsce na dysku twardego zarządzania |Nie dotyczy |Logiczne |Udostępniona |Nie dotyczy |Miejsce zarezerwowane w puli magazynów dysku twardego dla zadań zarządzania. |
| Miejsce na dysku kworum |Nie dotyczy |Logiczne |Udostępniona |Nie dotyczy |Miejsce zarezerwowane w puli magazynów dysku dla kworum klastra. |
| Dysk twardy zamiany miejsca |Nie dotyczy |Logiczne |Udostępniona |Nie dotyczy |Miejsce zarezerwowane w puli magazynu dysk twardy do zastąpienia kontrolera. |
| Miejsca na dysku SSD danych |Nie dotyczy |Logiczne |Udostępniona |Nie dotyczy |Używanego miejsca w magazynie danych w puli magazynu półprzewodnikowych (SSD) dysku. |
| NVRAM dysków SSD miejsca |Nie dotyczy |Logiczne |Udostępniona |Nie dotyczy |Miejsca do magazynowania w puli pamięci masowej SSD przeznaczona do obsługi logiki NVRAM. |
| Dysk twardy puli magazynu |Nie dotyczy |Logiczne |Udostępniona |Nie dotyczy |Wyświetla stan puli magazynu logicznego, która jest tworzona z urządzenia dysków twardych. |
| Puli pamięci masowej SSD |Nie dotyczy |Logiczne |Udostępniona |Nie dotyczy |Wyświetla stan puli magazynu logicznego, która jest tworzona z urządzenia dyski SSD. |
| Kontroler [0-1] [state] |WE/WY |Fizyczne |Kontrolera |Tak |Wyświetla stan kontrolera, oraz czy jest w trybie aktywny lub wstrzymania w obudowie. |
| Czujnikami temperatury w kontrolerze |WE/WY |Fizyczne |Kontrolera |Nie |Wiele czujnikami temperatury, takich jak moduł we/wy, temperatury procesora CPU, czujników DIMM i PCIe ma ich stanie wyświetlony, wskazuje, czy temperatury napotkano mieści się w granicach. |
| SAS expander |WE/WY |Fizyczne |Kontrolera |Nie |Wskazuje stan serial expander SCSI (SAS) podłączone, który jest używany do nawiązania połączenia z kontrolerem zintegrowanej pamięci masowej. |
| Łącznik SAS [0-1] |WE/WY |Fizyczne |Kontrolera |Nie |Wskazuje stan poszczególnych łączników sygnatury dostępu Współdzielonego, który jest używany do nawiązania zintegrowanej pamięci masowej SAS expander. |
| Między połączeniami wzajemnymi SBB środkowa |WE/WY |Fizyczne |Kontrolera |Nie |Wskazuje stan łącznika środkowa, który jest używany do nawiązania połączenia środkowa każdego kontrolera. |
| Rdzeń procesora |WE/WY |Fizyczne |Kontrolera |Nie |Wskazuje stan rdzeni procesora w ramach każdego kontrolera. |
| Obudowa electronics zasilania |WE/WY |Fizyczne |Kontrolera |Nie |Wskazuje stan zasilania systemu obudowa. |
| Obudowa electronics diagnostyki |WE/WY |Fizyczne |Kontrolera |Nie |Wskazuje stan podsystemów diagnostyki udostępniane przez kontroler. |
| Kontroler zarządzania płytą główną (BMC) |WE/WY |Fizyczne |Kontrolera |Nie |Wskazuje stan kontrolera zarządzania płytą główną (BMC), który jest procesor specjalne usług, który monitoruje urządzenia sprzętowego przez czujniki i komunikuje się z administratorem systemu za pośrednictwem połączenia niezależne. |
| Ethernet |WE/WY |Fizyczne |Kontrolera |Nie |Wskazuje stan wszystkich interfejsów sieciowych, oznacza to, zarządzania i porty dane podane w kontrolerze. |
| NVRAM |WE/WY |Fizyczne |Kontrolera |Nie |Wskazuje stan NVRAM, trwałej pamięci baterii, która służy do przechowywania informacji o aplikacji o znaczeniu krytycznym w przypadku awarii zasilania kopię zapasową. |

## <a name="component-list-for-ebod-enclosure-of-storsimple-device"></a>Lista składników dla obudowa EBOD urządzenia StorSimple
W poniższej tabeli przedstawiono składniki fizyczne i logiczne zawartych w obudowie EBOD lokalnego urządzenia StorSimple.

| Składnik | Moduł | Typ | Lokalizacja | JEDNOSTKI FRU? | Opis |
| --- | --- | --- | --- | --- | --- |
| Dysk w gnieździe [0-11] |Stacje dysków |Fizyczne |Udostępniona |Tak |Dla poszczególnych dysków HDD przodu obudowy EBOD zobaczy jeden wiersz. |
| Czujnik temperatury otoczenia |Obudowa |Fizyczne |Udostępniona |Nie |Mierzy temperatury w obudowie. |
| Czujnik temperatury środkowa |Obudowa |Fizyczne |Udostępniona |Nie |Mierzy temperatury środkowa. |
| Alarmu |Obudowa |Fizyczne |Udostępniona |Nie |Wskazuje, czy w podsystemie alarmu w obudowie funkcjonalności. |
| Obudowa |Obudowa |Fizyczne |Udostępniona |Tak |Wskazuje na obecność podstawę. |
| Obudowa ustawienia |Obudowa |Fizyczne |Udostępniona |Nie |Odnosi się do OPS lub z panelu przedniego obudowy. |
| Wiersz czujnikami napięcia |PCM |Fizyczne |Udostępniona |Nie |Wiele czujnikami napięcia wiersz ma ich stanie wyświetlony, wskazuje, czy napięcia mierzoną w granicach tolerancji. |
| Czujniki bieżącego wiersza |PCM |Fizyczne |Udostępniona |Nie |Wiele czujników bieżący wiersz ma ich stanie wyświetlony, wskazuje, czy bieżący mierzoną w granicach tolerancji. |
| Czujnikami temperatury w PCM |PCM |Fizyczne |Udostępniona |Nie |Wiele czujnikami temperatury, takich jak Inlet i czujników punkt aktywny mają ich stanie wyświetlony, wskazuje, czy temperatury mierzoną w granicach tolerancji. |
| Zasilacz [0-1] |PCM |Fizyczne |Udostępniona |Tak |Jeden wiersz jest widoczne dla każdego zasilacze w dwóch PCMs, znajduje się tyłu na urządzeniu. |
| Chłodzenia [0-1] |PCM |Fizyczne |Udostępniona |Tak |Dla każdego z czterech wentylatory znajdującej się w dwóch PCMs zobaczy jeden wiersz. |
| Lokalny magazyn [dysk twardy] |Nie dotyczy |Logiczne |Udostępniona |Nie dotyczy |Wyświetla stan puli magazynu logicznego, która jest tworzona z urządzenia dysków twardych. |
| Kontroler [0-1] [state] |WE/WY |Fizyczne |Kontrolera |Tak |Wyświetla stan kontrolerów w EBOD module. |
| Czujnikami temperatury w EBOD |WE/WY |Fizyczne |Kontrolera |Nie |Wiele czujnikami temperatury przez każdy z kontrolerów ma ich stanie wyświetlony, wskazuje, czy temperatury napotkano mieści się w granicach. |
| SAS expander |WE/WY |Fizyczne |Kontrolera |Nie |Wskazuje stan expander sygnatury dostępu Współdzielonego, który jest używany do nawiązania połączenia z kontrolerem zintegrowanej pamięci masowej. |
| Łącznik SAS [0-2] |WE/WY |Fizyczne |Kontrolera |Nie |Wskazuje stan poszczególnych łączników sygnatury dostępu Współdzielonego, który jest używany do nawiązania zintegrowanej pamięci masowej SAS expander. |
| Między połączeniami wzajemnymi SBB środkowa |WE/WY |Fizyczne |Kontrolera |Nie |Wskazuje stan łącznika środkowa, który jest używany do nawiązania połączenia środkowa każdego kontrolera. |
| Obudowa electronics zasilania |WE/WY |Fizyczne |Kontrolera |Nie |Wskazuje stan zasilania systemu obudowa. |
| Obudowa electronics diagnostyki |WE/WY |Fizyczne |Kontrolera |Nie |Wskazuje stan podsystemów diagnostyki udostępniane przez kontroler. |
| Połączenie do urządzenia kontrolera |WE/WY |Fizyczne |Kontrolera |Nie |Wskazuje stan połączenia między modułem EBOD We/Wy i kontrolerów urządzeń. |

## <a name="next-steps"></a>Następne kroki
* Aby korzystać z usługi Menedżer StorSimple do administrowania urządzenia, przejdź do [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md).
* Rozwiązywać składnik urządzenia o stanie działanie lub uszkodzenie należy odwoływać się do [StorSimple wskaźniki monitorowania](storsimple-monitoring-indicators.md). 
* Aby zastąpić składnik sprzętowy nie powiodło się, zobacz [wymiana składników sprzętowych StorSimple](storsimple-hardware-component-replacement.md).
* Jeśli nadal występują problemy dotyczące urządzenia, [skontaktuj się z Microsoft Support](storsimple-contact-microsoft-support.md).

