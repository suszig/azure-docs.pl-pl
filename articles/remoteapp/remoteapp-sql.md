---
title: "Usługi SQL Azure z usługą Azure RemoteApp | Microsoft Docs"
description: "Dowiedz się, jak korzystać z usług SQL Azure z usługą Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
editor: 
ms.assetid: 35f81d75-bfd7-4980-807e-00339f2cb2a4
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 24d41c7eb6b5bd34c07d21318740ecf4c6db2d22
ms.lasthandoff: 03/31/2017


---
# <a name="sql-azure-with-azure-remoteapp"></a>Usługi SQL Azure z usługą Azure RemoteApp
> [!IMPORTANT]
> Usługa Azure RemoteApp nie będzie obsługiwana od 31 sierpnia 2017 r. Szczegółowe informacje zawiera [powiadomienie](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Często zdarza się, że klienci planujący hostowanie swoich aplikacji systemu Windows w chmurze za pomocą usługi Azure RemoteApp chcą także dokonać migracji ich danych (na przykład serwerów SQL Server) do chmury w celu osiągnięcia pełnego wdrożenia w chmurze. Pozwala to uzyskać całościowe rozwiązanie hostowane w chmurze, dostępne w dowolnym momencie z każdego miejsca przy użyciu dowolnego urządzenia za pośrednictwem usługi Azure RemoteApp. Poniżej znajdują się linki i odniesienia oraz wskazówki ułatwiające przeprowadzenie tego procesu.  

## <a name="migrate-your-sql-data"></a>Migrowanie danych SQL
Rozpocznij od [migracji bazy danych SQL Server do usługi Azure SQL Database](../sql-database/sql-database-cloud-migrate.md). 

## <a name="configure-azure-remoteapp"></a>Konfigurowanie usługi Azure RemoteApp
Możesz hostować aplikacje systemu Windows w usłudze Azure RemoteApp Poniżej znajduje się ogólny opis kroków, które należy wykonać:

1. Utwórz [maszynę wirtualną z szablonem usługi Azure RemoteApp](remoteapp-imageoptions.md). 
2. Zainstaluj wymaganą aplikację na maszynie wirtualnej.
3. Skonfiguruj aplikację w celu łączenia się z bazą danych SQL i potwierdź poprawność jej działania.
4. Użyj narzędzia sysprep i zamknij maszynę wirtualną. Przechwyć to jako obraz przeznaczony do użycia z usługą Azure. **Uwaga:** należy upewnić się, że aplikacja jest w stanie zachować informacje o łączności z bazą danych w czasie realizacji procesu programu sysprep. Jeśli aplikacja nie może zachować informacji o połączeniu z bazą danych, warto skontaktować się z dostawcą aplikacji i dowiedzieć się, jak można określić parametry połączenia.
5. Zaimportuj niestandardowy obraz do biblioteki usługi Azure RemoteApp, wybierając właściwą lokalizację geograficzną wdrożenia usług SQL Azure. 
6. Dokonaj wdrożenia kolekcji usługi RemoteApp w tym samym centrum danych, co wdrożenie usług SQL Azure, używając powyższego szablonu, po czym opublikuj aplikację. Wdrożenie usługi Azure RemoteApp w tym samym centrum danych, w którym zostały wdrożone usługi SQL Azure, zapewnia największą szybkość połączeń i skrócony czas oczekiwania. 

## <a name="app-and-sql-configuration-considerations"></a>Zagadnienia do rozważenia związane z konfiguracją aplikacji i bazy danych SQL:
Istnieje kilka kwestii, które należy wziąć pod uwagę podczas korzystania z usług Azure SQL z usługą RemoteApp:

Dowiedz się, [jak skonfigurować zaporę bazy danych Azure SQL](../sql-database/sql-database-firewall-configure.md). Zgodnie z artykułem: „Początkowo dostęp do serwera usługi Azure SQL Database jest blokowany przez zaporę. Aby rozpocząć korzystanie z serwera usługi Azure SQL Database, należy przejść do portalu klasycznego i określić reguły zapory na poziomie serwera, które umożliwią dostęp do serwera Azure SQL Database. Użyj reguł zapory do określenia zakresu dozwolonych adresów IP pochodzących z Internetu oraz możliwości podejmowania przez aplikacje platformy Azure prób połączenia się z serwerem usługi Azure SQL Database”.

Gdy komputer próbuje połączyć się w Internecie z serwerem bazy danych, zapora sprawdza źródłowy adres IP żądania i porównuje go z pełnym zestawem reguł na poziomie serwera i (jeśli jest to wymagane) na poziomie bazy danych. „Jeśli adres IP żądania należy do jednego z zakresów określonych w regułach zapory na poziomie serwera, zostanie ustanowione połączenie z serwerem usługi Azure SQL Database”. Dzięki temu można używać zakresów adresów IP, a nie tylko pojedynczych źródłowych adresów IP.

Postępuj zgodnie z instrukcjami krok po kroku opisanych w temacie [Konfigurowanie ustawień zapory dla usługi SQL Database przy użyciu witryny Azure Portal](../sql-database/sql-database-configure-firewall-settings.md), aby określić zakres adresów IP. Podczas konfigurowania reguł zapory SQL podaj zakres adresów IP podsieci, która jest przeznaczona do obsługi kolekcji usługi Azure RemoteApp. Powinno to umożliwić serwerom ARA nawiązanie połączenia z bazą danych SQL pomimo korzystania z dynamicznie przypisywanych adresów IP.

## <a name="troubleshooting"></a>Rozwiązywanie problemów
Istnieje kilka przyczyn spowolnienia działania aplikacji klienckiej hostowanej w usłudze Azure RemoteApp, która łączy się z bazą danych SQL hostowaną na platformie Azure lub lokalnie.  

* Opóźnienie sieci pomiędzy urządzeniem a platformą Azure jest wysokie. Aby uzyskać najlepszą wydajność, wybierz najlepsze i najszybsze dostępne połączenie sieciowe. Skorzystaj z witryny [azurespeed.com](http://azurespeed.com/) jako ogólnego narzędzia do testowania opóźnienia między urządzeniami a centrum danych Azure.  
* Aplikacja kliencka hostowana w usłudze Azure RemoteApp jest mocno obciążona. Wybór innego planu rozliczeniowego, na przykład Premium, umożliwi zwiększenie wydajności. Innym sposobem jest monitorowanie zasobów zużywanych przez aplikację. Podczas aktywnej sesji naciśnij kolejno klawisze Ctrl-Alt-End, co spowoduje uruchomienie ekranu SAS, wybierz Menedżera zadań i sprawdź wykorzystanie zasobów przez aplikację.
* Serwer SQL jest mocno obciążony lub nie jest zoptymalizowany. Postępuj zgodnie ze wskazówkami dotyczącymi rozwiązywania problemów z bazą danych SQL. 


