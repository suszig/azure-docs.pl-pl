---
title: "Informacje o wersji usługi Azure Data Catalog | Dokumentacja firmy Microsoft"
description: "Informacje o wersji dla usługi Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 3aca9c49-45a4-4352-92e6-bd25ee3eacf7
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: d3db9bee0558cac5fb4f5b8fb4ab67a35ce0f141
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-data-catalog-release-notes"></a>Informacje o wersji usługi Azure Data Catalog
## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a>Informacje o 20 listopada 2015 wersji usługi Azure Data Catalog
### <a name="opening-data-sources-in-power-bi-desktop"></a>Otwieranie źródeł danych w programie Power BI Desktop
Korzystając z opcji "Otwórz w Power BI Desktop" z **Azure Data Catalog** portalu, użytkownicy mogą wystąpić jeden z dwóch problemów w aplikacji Power BI Desktop:

* Zostanie wyświetlone okno dialogowe z tytułem "Nie do otwartego dokumentu"
* Otwarcie aplikacji Power BI Desktop, ale plik może być pusta.

W każdej sytuacji, można rozwiązać problemu przez pobranie i zainstalowanie najnowszej wersji programu Power BI Desktop z [witryny PowerBI.com](https://powerbi.com).

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a>Informacje o 13 listopada 2015 wersji usługi Azure Data Catalog
### <a name="registering-and-connecting-to-teradata"></a>Rejestrowanie i nawiązywania połączenia z programu Teradata
Podczas łączenia się użytkowników źródła danych programu Teradata musi być zainstalowany odpowiedni sterownik ODBC programu Teradata, który pasuje do liczby bitów (32-bitowy lub 64-bitowy) oprogramowanie wykorzystywane.

Tej daty ADC zlecenia, najnowszej [sterownika ODBC programu Teradata dla systemu windows (wersja 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) jest zgodny z pakietu Office 2013, ale nie z pakietu Office 2016.

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a>Informacje o 13 lipca 2015 wersji usługi Azure Data Catalog
### <a name="registering-and-connecting-to-oracle-database"></a>Rejestrowanie i nawiązywania połączenia z bazą danych Oracle
Podczas nawiązywania połączenia ze źródłami danych bazy danych programu Oracle użytkownicy mają zainstalowany poprawne sterowniki Oracle zgodne bitowości oprogramowanie wykorzystywane (32-bitowy lub 64-bitowe).

* Podczas rejestrowania źródła danych Oracle na komputerze z 32-bitowego systemu Windows, będą używane 32-bitowe sterowniki Oracle
* Podczas rejestrowania źródła danych Oracle na komputerze z systemem 64-bitowego systemu Windows, będą używane sterowników Oracle 64-bitowych
* Podczas nawiązywania połączenia ze źródłami danych Oracle przy użyciu programu Excel na komputerze z 32-bitowej wersji pakietu Microsoft Office, w tym na 64-bitowym systemie Windows, 32-bitowe sterowniki Oracle będą używane
* Podczas nawiązywania połączenia ze źródłami danych Oracle przy użyciu programu Excel na komputerze z systemem 64-bitowej wersji pakietu Microsoft Office, będą używane sterowników Oracle 64-bitowych

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a>Rejestrowanie i podłączają je do programu SQL Server Reporting Services
Obsługa źródeł danych usługi SQL Server Reporting Services (SSRS) jest aktualnie ograniczona tylko do serwerów w trybie macierzystym. Obsługa usługi SSRS w trybie programu SharePoint zostanie dodany w nowszej wersji.

### <a name="opening-data-assets-in-excel"></a>Otwieranie zasobów danych w programie Excel
Podczas otwierania zasobów danych w programie Microsoft Excel z **Azure Data Catalog** portalu użytkowników może zostać wyświetlony monit o **powiadomienie o zabezpieczeniach programu Microsoft Excel** okno dialogowe. Standard, jest to oczekiwane zachowanie, a użytkownicy mogą wybrać **włączyć** aby kontynuować.

Aby uzyskać więcej informacji, zobacz [Włącz lub Wyłącz alerty zabezpieczeń dotyczące łącza i pliki z podejrzanych witrynach internetowych](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Serwer proxy i zasad konfiguracji i danych rejestracji źródła
Użytkownicy mogą wystąpić w sytuacji, gdy zalogować się do portalu wykazu danych Azure, ale podczas prób logowania do napotkania komunikat o błędzie, który uniemożliwia zalogowanie narzędzia rejestracji źródła danych.

Istnieją dwie możliwe przyczyny tego problemu zachowania:

**1 Przyczyna: Konfiguracja Active Directory Federation Services** narzędzia rejestracji źródła danych używa uwierzytelniania formularzy do sprawdzania poprawności logowania użytkowników w usłudze Active Directory. Do pomyślnego logowania należy włączyć uwierzytelnianie formularzy w ramach globalnych zasad uwierzytelniania przez administratora usługi Active Directory.

W niektórych sytuacjach zachowanie ten błąd może wystąpić tylko wtedy, gdy użytkownik jest w sieci firmowej lub tylko wtedy, gdy użytkownik nawiązuje połączenie z spoza sieci firmowej. Globalne zasady uwierzytelniania umożliwia metod uwierzytelniania należy włączyć osobno dla intranetu i ekstranetu połączeń. Błędy logowania może wystąpić, jeśli nie włączono uwierzytelniania formularzy dla sieci, z którym łączy się użytkownik.

Aby uzyskać więcej informacji, zobacz [Konfigurowanie zasad uwierzytelniania](https://technet.microsoft.com/library/dn486781.aspx).

**Przyczyny 2: Konfiguracja serwera proxy sieci** Jeśli sieć firmowa używa serwera proxy, narzędzie do rejestracji nie można nawiązać połączenia z usługą Azure Active Directory przez serwer proxy. Użytkownicy mogą upewnij się, że narzędzie do rejestracji, edytując plik konfiguracji narzędzia, dodanie do pliku w tej sekcji:

      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


Aby zlokalizować plik RegistrationTool.exe.config, uruchom narzędzie do rejestracji, a następnie otwórz narzędzie Menedżera zadań systemu Windows. Na karcie Szczegóły w Menedżerze zadań kliknij prawym przyciskiem myszy na RegistrationTool.exe i wybierz z menu podręcznego Otwórz lokalizację pliku.
