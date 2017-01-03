---
title: "Uwierzytelnianie i autoryzacja bazy danych SQL: Udzielanie dostępu | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat zarządzania zabezpieczeniami bazy danych SQL, w szczególności o zarządzaniu zabezpieczeniami dostępu i logowania za pomocą konta głównego poziomu serwera."
keywords: "zabezpieczenia bazy danych sql, zarządzanie zabezpieczeniami bazy danych, zabezpieczenia logowania, zabezpieczenia bazy danych, dostęp do bazy danych"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 0a65a93f-d5dc-424b-a774-7ed62d996f8c
ms.service: sql-database
ms.custom: authentication and authorization
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 09/14/2016
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: 498ee06fedf986a1b7fb4e362c4b5cb688f6a685
ms.openlocfilehash: 37cce678e0a9dfb081719cee4be7a04a838c1012


---
# <a name="sql-database-authentication-and-authorization-granting-access"></a>Uwierzytelnianie i autoryzacja bazy danych SQL: Udzielanie dostępu
> [!div class="op_single_selector"]
> * [Samouczek z wprowadzeniem](sql-database-get-started-security.md)
> * [Udzielanie dostępu](sql-database-manage-logins.md)
> 
> 

Od tego miejsca zaczyna się omówienie pojęć dotyczących dostępu do bazy danych SQL dla administratorów, użytkowników niebędących administratorami i ról.

## <a name="unrestricted-administrative-accounts"></a>Konta z uprawnieniami administracyjnymi bez ograniczeń
Możliwe są dwa rodzaje kont administracyjnych z nieograniczonymi uprawnieniami dostępu do wirtualnej głównej bazy danych i baz danych wszystkich użytkowników. Te konta noszą nazwę kont głównych poziomu serwera.

### <a name="azure-sql-database-subscriber-account"></a>Konto subskrybenta usługi Azure SQL Database
Konto jednej nazwy logowania jest tworzone podczas tworzenia logicznego wystąpienia SQL o nazwie konto subskrybenta usługi SQL Database. To konto używa do połączenia uwierzytelnienia programu SQL Server (nazwy użytkownika i hasła). To konto jest administratorem wystąpienia serwera logicznego i wszystkich baz danych użytkownika dołączonych do tego wystąpienia. Uprawnień konta subskrybenta nie można ograniczać. Może istnieć tylko jedno z tych kont.

### <a name="azure-active-directory-administrator"></a>Administrator usługi Azure Active Directory
Jedno konto usługi Azure Active Directory można skonfigurować również jako konto administratora. To konto może być indywidualnym użytkownikiem usługi Azure AD lub grupą usługi Azure AD zawierającą kilku użytkowników tej usługi. Skonfigurowanie administratora usługi Azure AD jest opcjonalne, ale aby można było połączyć się z usługą SQL Database za pomocą uwierzytelniania systemu Windows dla kont usługi Azure AD, musi on być skonfigurowany. Aby uzyskać więcej informacji na temat konfigurowania dostępu do usługi Azure Active Directory, zobacz artykuły [Connecting to SQL Database or SQL Data Warehouse By Using Azure Active Directory Authentication](sql-database-aad-authentication.md) (Łączenie się z usługą SQL Database lub SQL Data Warehouse przy użyciu uwierzytelnienia usługi Azure Active Directory) i [SSMS support for Azure AD MFA with SQL Database and SQL Data Warehouse](sql-database-ssms-mfa-authentication.md) (Obsługa programu SSMS w usłudze Azure AD MFA przy użyciu usługi SQL Database i SQL Data Warehouse).

### <a name="configuring-the-firewall"></a>Konfigurowanie zapory
W przypadku skonfigurowania zapory na poziomie serwera konto subskrybenta usługi Azure SQL Database i konto usługi Azure Active Directory mogą łączyć się z główną bazą danych i wszystkimi bazami danych użytkownika. Zaporę na poziomie serwera można skonfigurować za pośrednictwem portalu. Po nawiązaniu połączenia można również skonfigurować dodatkowe reguły zapory na poziomie serwera za pomocą instrukcji [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) języka Transact-SQL. Aby uzyskać więcej informacji dotyczących konfigurowania, zobacz artykuł [Tworzenie reguł zapory na poziomie serwera usługi Azure SQL Database i zarządzanie nimi za pomocą witryny Azure Portal](sql-database-configure-firewall-settings.md).

### <a name="administrator-access-path"></a>Ścieżka dostępu administratora
Po poprawnym skonfigurowaniu zapory na poziomie serwera konto subskrybenta usługi SQL Database i administratorzy programu SQL Server usługi Azure Active Directory mogą łączyć się przy użyciu narzędzi klienckich, takich jak SQL Server Management Studio lub SQL Server Data Tools. Tylko najnowsze narzędzia oferują wszystkie funkcje i możliwości. Na poniższym diagramie przedstawiono typową konfigurację dla tych dwóch kont administracyjnych.
    ![Ścieżka dostępu administratora](./media/sql-database-manage-logins/1sql-db-administrator-access.png)

Używając otwartego portu w zaporze na poziomie serwera, administratorzy mogą połączyć się z dowolną bazą danych SQL.

### <a name="connecting-to-a-database-by-using-sql-server-management-studio"></a>Łączenie się z bazą danych przy użyciu programu SQL Server Management Studio
Omówienie łączenia się przy użyciu programu SQL Server Management Studio można znaleźć w artykule [Nawiązywanie połączenia z bazą danych SQL Database za pomocą programu SQL Server Management Studio i wykonywanie przykładowego zapytania T-SQL](sql-database-connect-query-ssms.md).

> [!IMPORTANT]
> Zalecane jest używanie najnowszej wersji programu Management Studio, aby zachować synchronizację z aktualizacjami platformy Microsoft Azure i usługi SQL Database. [Zaktualizuj program SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="additional-special-accounts"></a>Dodatkowe konta specjalne
Usługa SQL Database oferuje dwie role administracyjne z ograniczeniami w wirtualnej głównej bazie danych, do której można dodawać konta użytkowników.

### <a name="database-creators"></a>Kreatory bazy danych
Konta administracyjne mogą tworzyć nowe bazy danych. Aby utworzyć dodatkowe konto, które może tworzyć bazy danych, należy utworzyć użytkownika w głównej bazie danych i dodać go do specjalnej roli bazy danych **dbmanager**. Użytkownik może być użytkownikiem zawartej bazy danych lub użytkownikiem opartym na nazwie logowania programu SQL Server w wirtualnej głównej bazie danych.

1. Połącz się z wirtualną główną bazą danych przy użyciu konta administracyjnego.
2. Krok opcjonalny: Utwórz nazwę logowania do uwierzytelniania w programie SQL Server przy użyciu instrukcji [CREATE LOGIN](https://msdn.microsoft.com/library/ms189751.aspx). Przykładowa instrukcja:
   
   ```
   CREATE LOGIN Mary WITH PASSWORD = '<strong_password>';
   ```
   
   > [!NOTE]
   > Podczas tworzenia nazwy logowania lub użytkownika zawartej bazy danych należy używać silnego hasła. Aby uzyskać więcej informacji, zobacz artykuł [Silne hasła](https://msdn.microsoft.com/library/ms161962.aspx).
   > 
   > 
   
   W celu poprawy wydajności nazwy logowania (nazwy główne na poziomie serwera) są tymczasowo przechowywane w pamięci podręcznej na poziomie bazy danych. Aby odświeżyć pamięć podręczną uwierzytelniania, zobacz artykuł [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx).
3. W wirtualnej głównej bazie danych utwórz użytkownika za pomocą instrukcji [CREATE USER](https://msdn.microsoft.com/library/ms173463.aspx). Użytkownik może być użytkownikiem uwierzytelnienia zawartej bazy danych usługi Azure Active Directory (jeśli skonfigurowano środowisko dla uwierzytelniania usługi Azure AD), użytkownikiem uwierzytelnienia zawartej bazy danych programu SQL Server lub użytkownikiem uwierzytelniania programu SQL Server w oparciu o nazwę logowania uwierzytelniania programu SQL Server (utworzonym w poprzednim kroku). Przykładowe instrukcje:
   
   ```
   CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
   CREATE USER Tran WITH PASSWORD = '<strong_password>';
   CREATE USER Mary FROM LOGIN Mary; 
   ```
4. Dodaj nowego użytkownika do roli bazy danych **dbmanager** przy użyciu instrukcji [ALTER ROLE](https://msdn.microsoft.com/library/ms189775.aspx). Przykładowe instrukcje:
   
   ```
   ALTER ROLE dbmanager ADD MEMBER Mary; 
   ALTER ROLE dbmanager ADD MEMBER [mike@contoso.com];
   ```
   
   > [!NOTE]
   > Dbmanager jest rolą bazy danych w wirtualnej głównej bazie danych, więc do roli dbmanager można dodać tylko użytkownika. Do roli na poziomie bazy danych nie można dodać nazwy logowania na poziomie serwera.
   > 
   > 
5. W razie potrzeby należy skonfigurować zaporę na poziomie serwera, aby umożliwić połączenie się nowemu użytkownikowi.

Teraz użytkownik może nawiązać połączenie z wirtualną główną bazą danych i tworzyć nowe bazy danych. Konto tworzące bazę danych staje się właścicielem bazy danych.

### <a name="login-managers"></a>Menedżerowie logowania
Jeśli chcesz, możesz wykonać te same kroki (tworzenie nazwy logowania i użytkownika, a następnie dodanie użytkownika do roli **loginmanager**), aby umożliwić użytkownikowi tworzenie nowych nazw logowania w wirtualnej głównej bazie danych. Zazwyczaj nie jest to konieczne, ponieważ firma Microsoft zaleca korzystanie z użytkowników zawartej bazy danych, którzy przeprowadzają uwierzytelnianie na poziomie bazy danych zamiast użytkowników przeprowadzających uwierzytelnianie w oparciu o nazwę logowania. Aby uzyskać więcej informacji, zobacz artykuł [Contained Database Users - Making Your Database Portable](https://msdn.microsoft.com/library/ff929188.aspx) (Użytkownicy zawartej bazy danych — tworzenie przenośnej bazy danych).

## <a name="non-administrator-users"></a>Użytkownicy niebędący administratorami
Ogólnie rzecz biorąc, konta inne niż administracyjne nie potrzebują dostępu do wirtualnej głównej bazy danych. Tworzenie użytkowników zawartej bazy danych na poziomie bazy danych przy użyciu instrukcji [CREATE USER (język Transact-SQL)](https://msdn.microsoft.com/library/ms173463.aspx). Użytkownik może być użytkownikiem uwierzytelnienia zawartej bazy danych usługi Azure Active Directory (jeśli skonfigurowano środowisko dla uwierzytelniania usługi Azure AD), użytkownikiem uwierzytelnienia zawartej bazy danych programu SQL Server lub użytkownikiem uwierzytelniania programu SQL Server w oparciu o nazwę logowania uwierzytelniania programu SQL Server (utworzonym w poprzednim kroku). Aby uzyskać więcej informacji, zobacz artykuł [Contained Database Users - Making Your Database Portable](https://msdn.microsoft.com/library/ff929188.aspx) (Użytkownicy zawartej bazy danych — tworzenie przenośnej bazy danych). 

Aby utworzyć użytkowników, połącz się z bazą danych i wykonaj instrukcje podobne do następujących:

```
CREATE USER Mary FROM LOGIN Mary; 
CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
```

Początkowo tylko jeden z administratorów lub jeden z właścicieli bazy danych mogą tworzyć użytkowników. Aby autoryzować dodatkowych użytkowników do tworzenia nowych użytkowników, należy udzielić im uprawnienia `ALTER ANY USER` przy użyciu instrukcji, takiej jak:

```
GRANT ALTER ANY USER TO Mary;
```

Aby przydzielić dodatkowym użytkownikom pełną kontrolę nad bazą danych, należy uczynić ich członkami ustalonej roli bazy danych **db_owner** za pomocą instrukcji `ALTER ROLE`.

> [!NOTE]
> Głównym powodem tworzenia użytkowników bazy danych opartych na nazwach logowania jest istnienie użytkowników uwierzytelniania programu SQL Server, którzy muszą mieć dostęp do wielu baz danych. Użytkownicy oparci na nazwach logowania są powiązani z nazwą logowania i tylko z jednym hasłem, które jest obsługiwane dla tej nazwy logowania. Użytkownicy zawartej bazy danych w poszczególnych bazach danych są poszczególnymi jednostkami i każdy z nich zachowuje własne hasło. Może to prowadzić do mylenia użytkowników zawartej bazy danych, jeśli nie zachowują oni identycznych haseł.
> 
> 

### <a name="configuring-the-database-level-firewall"></a>Konfigurowanie zapory na poziomie bazy danych
Najlepszym rozwiązaniem jest sytuacja, gdy użytkownicy niebędący administratorami mają dostęp do używanych baz danych tylko poprzez zaporę. Zamiast autoryzowania ich adresów IP poprzez zaporę na poziomie serwera i przydzielania dostępu do wszystkich baz danych użyj instrukcji [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx), aby skonfigurować zaporę na poziomie bazy danych. Nie można skonfigurować zapory na poziomie bazy danych za pomocą portalu.

### <a name="non-administrator-access-path"></a>Ścieżka dostępu użytkownika niebędącego administratorem
Gdy zapora na poziomie bazy danych jest odpowiednio skonfigurowana, użytkownicy bazy danych mogą łączyć się za pomocą narzędzia klienckiego, takiego jak SQL Server Management Studio lub SQL Server Data Tools. Tylko najnowsze narzędzia oferują wszystkie funkcje i możliwości. Na poniższym diagramie przedstawiono typowe ścieżki dostępu użytkowników niebędących administratorami.
![Ścieżka dostępu użytkownika niebędącego administratorem](./media/sql-database-manage-logins/2sql-db-nonadmin-access.png)

## <a name="groups-and-roles"></a>Grupy i role
Wydajne zarządzanie dostępem obejmuje korzystanie z uprawnień przypisanych do grup i ról, a nie do poszczególnych użytkowników. Na przykład w przypadku uwierzytelniania za pomocą usługi Azure Active Directory:

* Umieść użytkowników usługi Azure Active Directory w grupie tej usługi. Utwórz użytkownika zawartej bazy danych dla tej grupy. Umieść przynajmniej jednego użytkownika bazy danych w roli bazy danych. Następnie przypisz uprawnienia do tej roli bazy danych.

W przypadku uwierzytelniania programu SQL Server:

* Utwórz użytkowników zawartej bazy danych w bazie danych. Umieść przynajmniej jednego użytkownika bazy danych w roli bazy danych. Następnie przypisz uprawnienia do tej roli bazy danych.

Role bazy danych mogą być rolami wbudowanymi, takimi jak **db_owner**, **db_ddladmin**, **db_datawriter**, **db_datareader**, **db_denydatawriter** i **db_denydatareader**. Rola **db_owner** jest najczęściej używana do udzielenia pełnych uprawnień jedynie niewielkiej liczbie użytkowników. Inne ustalone role bazy danych ułatwiają szybkie tworzenie prostej bazy danych, ale nie zaleca się ich używania w większości przypadków tworzenia produkcyjnych baz danych. Na przykład ustalona rola bazy danych **db_datareader** pozwala na odczyt każdej tabeli w bazie danych, co nie zawsze jest niezbędne. Znacznie lepiej jest używać instrukcji [CREATE ROLE](https://msdn.microsoft.com/library/ms187936.aspx), aby tworzyć własne role użytkownika bazy danych i rozważnie udzielać każdej roli możliwie najniższych uprawnień niezbędnych do zaspokojenia potrzeb biznesowych. Gdy użytkownik jest członkiem wielu ról, łączą one uprawnienia ich wszystkich.

## <a name="permissions"></a>Uprawnienia
Istnieje ponad 100 uprawnień, których można indywidualnie udzielić lub odmówić w usłudze SQL Database. Wiele z tych uprawnień jest zagnieżdżonych. Na przykład uprawnienie `UPDATE` na schemacie obejmuje `UPDATE` uprawnienie dla każdej tabeli na tym schemacie. Podobnie jak w przypadku większości systemów, odmowa przyznania uprawnienia kasuje przyznanie. Ze względu na zagnieżdżoną naturę uprawnień oraz ich liczbę zaprojektowanie systemu zabezpieczającego bazę danych w prawidłowy sposób może wymagać starannej analizy. Rozpocznij od listy uprawnień [Uprawnienia (aparat bazy danych)](https://msdn.microsoft.com/library/ms191291.aspx) i przejrzyj [obszerny wykaz](http://go.microsoft.com/fwlink/?LinkId=229142) uprawnień.

## <a name="next-steps"></a>Następne kroki
[Zabezpieczanie bazy danych SQL](sql-database-security.md)

[Tworzenie tabeli \(— samouczek\)](https://msdn.microsoft.com/library/ms365315.aspx)

[Wstawianie i aktualizowanie danych w tabeli \(— samouczek\)](https://msdn.microsoft.com/library/ms365309.aspx)

[Odczytywanie danych w tabeli \(— samouczek\)](https://msdn.microsoft.com/library/ms365310.aspx)

[Tworzenie widoków i procedur składowanych](https://msdn.microsoft.com/library/ms365311.aspx)

[Udzielanie dostępu do obiektu bazy danych](https://msdn.microsoft.com/library/ms365327.aspx)

## <a name="additional-resources"></a>Dodatkowe zasoby
[Zabezpieczanie bazy danych SQL](sql-database-security.md)

[Usługa Security Center aparatu bazy danych programu SQL Server i bazy danych Azure SQL Database](https://msdn.microsoft.com/library/bb510589.aspx) 




<!--HONumber=Dec16_HO3-->


