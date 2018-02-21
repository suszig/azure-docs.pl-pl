---
title: "Baza danych Azure SQL — często zadawane pytania | Dokumentacja firmy Microsoft"
description: "Odpowiedzi na typowe pytania klientów poproś o baz danych w chmurze i baza danych SQL Azure, system zarządzania relacyjnymi bazami danych firmy Microsoft (RDBMS) oraz bazy danych jako usługa w chmurze."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 1da12abc-0646-43ba-b564-e3b049a6487f
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 02/12/2018
ms.author: carlrab
ms.openlocfilehash: 4efa053afd26bde208441c4b841c5d02142a2d18
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2018
---
# <a name="sql-database-faq"></a>SQL Database — często zadawane pytania

## <a name="what-is-the-current-version-of-sql-database"></a>Co to jest bieżąca wersja bazy danych SQL?
Bieżąca wersja bazy danych SQL jest w wersji 12. W wersji V11 została wycofana.

## <a name="what-is-the-sla-for-sql-database"></a>Co to jest umowy SLA dla bazy danych SQL?
Firma Microsoft gwarantuje, że przez co najmniej 99,99% czasu klienci będą mogli korzystać z łączności między pojedynczą lub elastyczną bazą danych usługi Microsoft Azure SQL Database w warstwach Podstawowa, Standardowa i Premium a bramą internetową firmy Microsoft. Aby uzyskać więcej informacji, zobacz [SLA](http://azure.microsoft.com/support/legal/sla/).

## <a name="how-do-i-reset-the-password-for-the-server-admin"></a>Jak zresetować hasła administratora serwera?
W [portalu Azure](https://portal.azure.com), kliknij przycisk **serwerów SQL**, wybierz serwer z listy, a następnie kliknij przycisk **Resetuj hasło**.

## <a name="how-do-i-manage-databases-and-logins"></a>Jak zarządzać baz danych i logowania
Zobacz [Zarządzanie bazami danych i logowaniami](sql-database-manage-logins.md).

## <a name="how-do-i-make-sure-only-authorized-ip-addresses-are-allowed-to-access-a-server"></a>Jak utworzyć się, że tylko autoryzowani adresy IP mogą uzyskać dostęp do serwera?
Zobacz [porady: Konfigurowanie ustawień zapory w bazie danych SQL](sql-database-configure-firewall-settings.md).

## <a name="how-does-the-usage-of-sql-database-show-up-on-my-bill"></a>Jak jest użycie bazy danych SQL wyświetlany na mojego rachunku?
Opłaty bazy danych SQL na przewidywalną, godzinową szybkość oparte na warstwie usługi + poziom wydajności dla pojedynczych baz danych lub Edtu dla każdej puli elastycznej. Rzeczywistego użycia jest obliczana i proporcjonalnie co godzinę, więc rachunku mogą być wyświetlane ułamków godzinę. Na przykład jeśli baza danych istnieje na 12 godzin w miesiącu, rachunku pokazuje użycie 0,5 dni. Ponadto warstwy usług i poziom wydajności i jednostek Edtu na pulę są podzielone w zestawieniu, aby ułatwić orientację w liczbie dni bazy danych, dla każdej z nich w ciągu jednego miesiąca.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Co zrobić, jeśli pojedynczej bazy danych jest aktywna na mniej niż godzinę lub używa wyższego poziomu usług mniej niż godzinę?
Opłaty są naliczane dla każdej godziny przy użyciu najwyższej warstwy usług istnieje baza danych + poziom wydajności, która zastosowana podczas tej godziny, niezależnie od użycia i czy baza danych była active przez mniej niż godzinę. Po utworzeniu pojedynczej bazy danych i usunąć go pięciu minut rachunku odzwierciedla opłat godzinę jedną bazę danych. 

Przykłady:

* Jeśli tworzenie podstawowej bazy danych, a następnie natychmiast uaktualnić go do standardowa S1, naliczane są opłaty szybkością standardowa S1 dla pierwszej godziny.
* Jeśli uaktualnienie bazy danych z Basic do Premium na 10:00 w dniu i zakończeniu uaktualniania 1:35 o godzinie następnego dnia są naliczane szybkością Premium, rozpoczynając od godziny 1:00 
* Jeśli starszą wersję bazy danych w warstwie Premium podstawowy o 11:00 wykonuje na 2:15:00, a następnie bazy danych jest rozliczana według szybkości Premium do 3:00 w dniu, po upływie którego jest rozliczana stawkami podstawowe.

## <a name="how-does-elastic-pool-usage-show-up-on-my-bill-and-what-happens-when-i-change-edtus-per-pool"></a>Jak jest użycie puli elastycznej wyświetlany na mojego rachunku i co się stanie po zmianie jednostek Edtu na pulę?
Puli elastycznej opłat Pokaż na rachunku jako elastycznej Dtu (Edtu) w przyrostach wyświetlany w obszarze jednostek Edtu na pulę na [stronie cen](https://azure.microsoft.com/pricing/details/sql-database/). Jest bezpłatna dla bazy danych dla puli elastycznej. Opłaty są naliczane dla każdej godziny puli istnieje w najwyższym liczbę jednostek eDTU, niezależnie od użycia i czy pula było aktywne przez mniej niż godzinę. 

Przykłady:

* Po utworzeniu standardowej puli elastycznej z 200 Edtu o godzinie 11:18 Dodawanie pięć baz danych do puli, naliczane są opłaty dla 200 Edtu całego godzinę od o godzinie 11 do końca dnia.
* W dniu 2, 5 o godzinie: 05 1 bazy danych rozpocznie korzystanie z 50 jednostek Edtu i przechowuje stałej za pośrednictwem dnia. Bazy danych 2-5 zmieniają się między 0 a Edtu 80. W ciągu dnia można dodać pięć innych baz danych używających różnych jednostek Edtu w ciągu dnia. Dzień 2 jest pełny dzień rozliczane według 200 eDTU. 
* W dniu 3, 5 o godzinie Możesz dodać inny 15 baz danych. W ciągu dnia do punktu, gdzie chcesz zwiększyć jednostek Edtu puli 200 400 8 o godzinie: 05 powoduje zwiększenie użycia bazy danych Opłaty na poziomie 200 eDTU były obowiązywać do 20: 00 i zwiększa do 400 jednostek Edtu dla pozostałych czterech godzin. 

## <a name="elastic-pool-billing-and-pricing-information"></a>Informacje o cenach i rozliczeniami puli elastycznej
Elastyczne pule są rozliczane według następującej charakterystyce:

* Pula elastyczna jest on rozliczany po jego utworzeniu, nawet jeśli nie ma baz danych w puli.
* Pula elastyczna jest rozliczane co godzinę. Jest to taką samą częstotliwością pomiarów, podobnie jak w przypadku poziomy wydajności pojedynczej bazy danych.
* Jeśli rozmiar puli elastycznej jest dopasowywany do nowej liczby jednostek Edtu, następnie puli jest nie rozliczane zgodnie z nowego liczbę jednostek Edtu przed zakończeniem operacji zmiany rozmiaru. Wynika z tego samego wzorca podczas zmieniania poziomu wydajności pojedynczej bazy danych.
* Cena puli elastycznej opiera się na liczbę jednostek Edtu puli. Cena puli elastycznej zależy od liczby i wykorzystania elastycznych baz danych w niej.
* Cena jest obliczana przez (liczba jednostek Edtu puli) x (cena jednostki na eDTU).

Cenie jednostkowej eDTU dla elastycznej puli jest wyższa niż cena jednostki DTU dla pojedynczej bazy danych w tej samej warstwie usług. Szczegóły można znaleźć w [cenniku usługi SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). 

Aby poznać warstwy jednostek Edtu i usług, zobacz [opcje bazy danych SQL i wydajność](sql-database-service-tiers.md).

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-pool-show-up-on-my-bill"></a>Jak jest użycie aktywna replikacja geograficzna w będą wyświetlane puli elastycznej na mojego rachunku?
W przeciwieństwie do pojedynczych baz danych przy użyciu [aktywna replikacja geograficzna](sql-database-geo-replication-overview.md) z elastycznych baz danych nie ma to bezpośredni wpływ rozliczeń.  Naliczane są tylko opłaty dla Edtu udostępnione dla poszczególnych pul (puli głównej i dodatkowej puli)

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>W jaki sposób funkcja inspekcji będzie obciążać mój rachunek?
Inspekcja jest wbudowana w usłudze SQL Database bez dodatkowych kosztów i jest dostępny dla baz danych Basic, Standard i Premium. Jednak do przechowywania dzienników inspekcji, używa funkcji inspekcji konta usługi Azure Storage oraz szybkości dla tabel i kolejek w usłudze Azure Storage są stosowane na podstawie rozmiaru dziennika inspekcji.

## <a name="how-do-i-find-the-right-service-tier-and-performance-level-for-single-databases-and-elastic-pools"></a>Jak znaleźć usługowy warstwę i poziom wydajności dla pojedynczych baz danych i pul elastycznych?
Dostępne są narzędzia kilka: 

* Dla lokalnych baz danych, użyj [advisor zmiany rozmiaru jednostek dtu w warstwie](http://dtucalculator.azurewebsites.net/) zaleca baz danych i Dtu wymagane i ocena wielu baz danych dla puli elastycznej.
* Jeśli pojedynczej bazy danych będzie korzystać z w puli, inteligentnego aparatu Azure zaleca puli elastycznej, jeśli wzorzec historycznych danych użycia, która go. Zobacz [monitorowanie i zarządzanie nimi puli elastycznej z portalu Azure](sql-database-elastic-pool-manage-portal.md). Aby uzyskać więcej informacji o sposobie wykonać obliczenia samodzielnie, zobacz [zagadnienia dotyczące cen i wydajności dla elastycznej puli](sql-database-elastic-pool.md)
* Aby sprawdzić, czy chcesz wybrać pojedynczej bazy danych w górę lub w dół, zobacz [wytyczne dotyczące wydajności dla pojedynczej bazy danych](sql-database-performance-guidance.md).

## <a name="how-often-can-i-change-the-service-tier-or-performance-level-of-a-single-database"></a>Jak często można zmienić poziomu warstwy lub wydajności usług pojedynczej bazy danych?
Jak często ma można zmienić warstwy usług (między podstawowa, standardowa i Premium) lub poziom wydajności w ramach warstwy usług (na przykład S1 S2). Dla wcześniejszych wersji baz danych można zmienić poziomu warstwy lub wydajności usług łącznie cztery razy w okresie 24 godzin.

## <a name="how-often-can-i-adjust-the-edtus-per-pool"></a>Jak często można dostosować jednostek Edtu na pulę?
Tyle razy, ile ma.

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-pool"></a>Jak długo trwa zmiana poziomu warstwy lub wydajności usług pojedynczej bazy danych lub przenieść bazę danych i elastycznej puli?
Zmiana warstwy usługi bazy danych i przenoszenia i puli wymaga bazy danych ma zostać skopiowany na platformie jako operacji w tle. Zmiana warstwy usług może zająć od kilku minut do kilku godzin w zależności od rozmiaru bazy danych. W obu przypadkach podczas przenoszenia baz danych pozostają online i dostępne. Aby uzyskać więcej informacji na temat zmieniania pojedynczych baz danych, zobacz [zmienić warstwy usługi bazy danych](sql-database-service-tiers.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Kiedy należy używać pojedynczej bazy danych, a elastycznych baz danych?
Ogólnie rzecz biorąc, elastyczne pule są przeznaczone dla typowe [oprogramowanie jako usługa (SaaS) aplikacji wzorzec](sql-database-design-patterns-multi-tenancy-saas-applications.md), gdy istnieje jedną bazę danych na dzierżawcy lub klienta. Kupowanie poszczególnych baz danych i przydzielanie dla każdej z nich nadmiernej ilości zasobów pod kątem zmiennego i szczytowego zapotrzebowania często nie jest ekonomiczne. Pule Zarządzanie łączną wydajność puli i baz danych górę i w dół Skalowanie automatyczne. Inteligentnego aparatu platformy Azure zaleca puli baz danych, gdy użycie wzorca gwarantuje on. Aby uzyskać więcej informacji, zobacz [wskazówki dotyczące puli elastycznej](sql-database-elastic-pool.md).

## <a name="what-does-it-mean-to-have-up-to-200-of-your-maximum-provisioned-database-storage-for-backup-storage"></a>Co oznacza dostępność do 200% maksymalnej aktywnej pojemności bazy danych na potrzeby kopii zapasowych?
Magazyn kopii zapasowych jest magazynem skojarzonym z kopii zapasowych automatycznych bazy danych, które są używane do [punkt-w — czas-przywracania](sql-database-recovery-using-backups.md#point-in-time-restore) i [geograficzne](sql-database-recovery-using-backups.md#geo-restore). Usługa Microsoft Azure SQL Database oferuje do 200% maksymalnej pojemności aktywnego magazynu bazy danych na potrzeby kopii zapasowych bez dodatkowych kosztów. Na przykład jeśli wystąpienie standardowe bazy danych o rozmiarze DB elastycznie 250 GB, są dostarczane z 500 GB magazynu kopii zapasowej bez dodatkowych opłat. Jeśli bazy danych przekroczy udostępnionego magazynu kopii zapasowych, możesz skrócić okres przechowywania, kontaktując się z pomocą techniczną platformy Azure lub zapłacić za dodatkowe kopii zapasowych magazynu rozliczane według stawki standardowej dostęp do odczytu geograficznie magazynu geograficznie nadmiarowego (RA-GRS). Aby uzyskać więcej informacji dotyczących rozliczeń RA-GRS Zobacz szczegóły cennika magazynu.

## <a name="im-moving-from-webbusiness-to-the-new-service-tiers-what-do-i-need-to-know"></a>Przeprowadzam z sieci Web/firmy, aby nowe warstwy usług, co należy wiedzieć?
Azure baz danych SQL w sieci Web i Business teraz zostały wycofane. Warstwy Basic, Standard, a Premium i elastyczna Zastąp Ustępujący baz danych w sieci Web i Business. 

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Co to jest opóźnienie replikacji oczekiwanego podczas replikowania geograficznie bazy danych między dwóch regionach w ramach tej samej lokalizacji geograficznej Azure?
Firma Microsoft są obecnie obsługiwane RPO pięciu sekund i opóźnienie replikacji została mniejszej niż że gdy dodatkowej geograficzna jest obsługiwana na platformie Azure zalecane sparowanego regionu i w tej samej warstwie usługi.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Co to jest opóźnienie replikacji oczekiwany po dodatkowej geograficzna jest tworzony w tym samym regionie co podstawowa baza danych?
Na podstawie empiryczne danych, nie istnieje za dużo różnica między wewnątrz regionu i opóźnienie replikacji między region podczas zalecanej platformy Azure sparowanym region jest używany. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Jeśli występuje awaria sieci między dwóch regionach, jak logika ponowień działa po skonfigurowaniu replikacji geograficznej?
W przypadku rozłączenia, aby ponownie ustanowić połączenia wykonanie co 10 sekund.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Co można zrobić, aby zagwarantować, że zmiany krytyczne w głównej bazie danych są replikowane?
Dodatkowej geograficzna jest replika async i firma Microsoft nie należy go przechowywać w pełnej synchronizacji z serwera podstawowego. Ale zapewniamy metodę, aby wymusić synchronizację w celu zapewnienia replikacji krytycznej zmiany (na przykład aktualizacji hasła). Wymuszone synchronizacji wpływa na wydajność, ponieważ wątek wywołujący blokuje dopóki wszystkie zatwierdzone transakcje są replikowane. Aby uzyskać więcej informacji, zobacz [operacja sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Jakie narzędzia są dostępne do monitorowania opóźnienie replikacji między podstawowej bazy danych i dodatkowej geograficzna?
Uwidaczniamy opóźnienie replikacji w czasie rzeczywistym między podstawowej bazy danych i geograficzna / pomocniczych za pośrednictwem DMV. Aby uzyskać więcej informacji, zobacz [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).

## <a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Aby przenieść bazę danych do innego serwera w tej samej subskrypcji
W [portalu Azure](https://portal.azure.com), kliknij przycisk **baz danych SQL**, wybierz bazę danych z listy, a następnie kliknij przycisk **kopiowania**. Zobacz [kopiowania bazy danych Azure SQL](sql-database-copy.md) uzyskać więcej szczegółowych informacji.

## <a name="to-move-a-database-between-subscriptions"></a>Aby przenieść bazę danych między subskrypcjami
W [portalu Azure](https://portal.azure.com), kliknij przycisk **serwerów SQL** , a następnie wybierz serwer, który jest hostem bazy danych z listy. Kliknij przycisk **Przenieś**, a następnie wybierz zasoby do przeniesienia i subskrypcję docelową.
