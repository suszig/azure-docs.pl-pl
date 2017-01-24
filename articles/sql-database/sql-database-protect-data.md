---
title: "Omówienie ochrony danych w usłudze Azure SQL Database | Microsoft Docs"
description: "Więcej informacji na temat ochrony danych w usłudze Azure SQL Database."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: secure and protect
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 12/21/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: f4712d70c0323e607ddcc021809f8097a621730d
ms.openlocfilehash: 67b32b224d86e8d0ed06d8248c3a2c5f5569c5cb


---
# <a name="protecting-data-within-your-sql-database"></a>Ochrona danych w usłudze SQL Database

Usługa SQL Database chroni dane przy użyciu szyfrowania.  

## <a name="overview"></a>Omówienie

Usługa SQL Database zabezpiecza dane, szyfrując przesyłane dane za pomocą protokołu [Transport Layer Security](https://support.microsoft.com/en-us/kb/3135244), zapisane dane za pomocą funkcji [Transparent Data Encryption](http://go.microsoft.com/fwlink/?LinkId=526242) oraz używane dane za pomocą funkcji [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx). Omówienie korzystania z tych funkcji ochrony danych w usłudze SQL Database zawiera artykuł dotyczący [ochrony i zabezpieczeń danych](sql-database-protect-data.md).

Można również rozważyć inne sposoby szyfrowania danych:

* [Szyfrowanie na poziomie komórki](https://msdn.microsoft.com/library/ms179331.aspx) służące do szyfrowania określonej kolumny lub nawet poszczególnych komórek danych przy użyciu różnych kluczy szyfrowania.
* Jeśli potrzebujesz sprzętowego modułu zabezpieczeń lub centralnego zarządzania hierarchią kluczy szyfrowania, rozważ użycie [funkcji Azure Key Vault w usłudze SQL Server w maszynie wirtualnej Azure](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).


Usługa SQL Database zabezpiecza dane, udostępniając możliwości inspekcji i wykrywania zagrożeń. 

### <a name="auditing"></a>Inspekcja
Funkcja inspekcji usługi SQL Database śledzi działania bazy danych i pozwala zachować zgodność z przepisami, rejestrując zdarzenia bazy danych w dzienniku inspekcji na koncie usługi Azure Storage. Inspekcja umożliwia zrozumienie trwających działań bazy danych, a także analizowanie i badanie działań historycznych w celu zidentyfikowania potencjalnych zagrożeń, podejrzanych nadużyć i naruszeń zabezpieczeń. Aby uzyskać więcej informacji, zobacz artykuł z [wprowadzeniem do funkcji inspekcji usługi SQL Database](sql-database-auditing-get-started.md).  

### <a name="threat-detection"></a>Wykrywanie zagrożeń
Wykrywanie zagrożeń uzupełnia funkcję inspekcji, stanowiąc dodatkową warstwę inteligentnych zabezpieczeń wbudowaną w usługę Azure SQL Database. Działa ono przez cały czas, ucząc się, profilując i wykrywając anomalie działań bazy danych. Będziesz otrzymywać alerty o podejrzanych działaniach, potencjalnych lukach w zabezpieczeniach, atakach polegających na wstrzyknięciu kodu SQL i anomaliach we wzorcach dostępu do bazy danych. Na alerty możesz odpowiadać za pomocą dostarczanych instrukcji z działaniami do wykonania. Aby uzyskać więcej informacji, zobacz artykuł [Get started with SQL Database Threat Detection](sql-database-threat-detection-get-started.md) (Wprowadzenie do usługi SQL Database Threat Detection).  

## <a name="connection-security"></a>Zabezpieczenia połączeń
Zabezpieczenia połączeń dotyczą sposobu ograniczania i zabezpieczania połączeń z bazą danych przy użyciu reguł zapory i szyfrowania połączeń.

Reguły zapory są używane zarówno przez serwer, jak i przez bazę danych do odrzucania prób połączenia z adresów IP, które nie zostały jawnie wymienione na liście dozwolonych hostów. Aby umożliwić danemu publicznemu adresowi IP aplikacji lub komputera klienta próbę nawiązania połączenia z nową bazą danych, należy najpierw utworzyć regułę zapory na poziomie serwera przy użyciu klasycznego portalu Azure, interfejsu API REST lub powłoki PowerShell. Najlepszym rozwiązaniem jest maksymalne ograniczenie zakresu adresów IP przepuszczanych przez zaporę serwera. Aby uzyskać więcej informacji, zobacz artykuł [Azure SQL Database Firewall](https://msdn.microsoft.com/library/ee621782) (Zapora usługi Azure SQL Database)

Wszystkie połączenia z usługą Azure SQL Database wymagają szyfrowania (SSL/TLS) podczas całego okresu, w którym dane są przesyłane do i z bazy danych. W parametrach połączenia aplikacji należy tak określić parametry, aby szyfrować połączenia i *nie* ufać certyfikatowi serwera (jest to wykonywane automatycznie po skopiowaniu parametrów połączenia z klasycznej witryny Azure Portal). W przeciwnym razie połączenie nie zweryfikuje tożsamości serwera i będzie podatne na ataki typu man-in-the-middle. Na przykład w przypadku sterownika ADO.NET te parametry połączenia mają wartość **Encrypt = True** i **TrustServerCertificate = False**. Aby uzyskać więcej informacji, zobacz artykuł [Azure SQL Database Connection Encryption and Certificate Validation](https://msdn.microsoft.com/library/azure/ff394108#encryption) (Szyfrowanie połączenia usługi Azure SQL Database i sprawdzanie poprawności certyfikatu).

## <a name="authentication"></a>Authentication
Uwierzytelnianie to sposób potwierdzenia tożsamości podczas nawiązywania połączenia z bazą danych. Usługa SQL Database obsługuje dwa typy uwierzytelniania:

* **Uwierzytelnianie SQL**, które używa nazwy użytkownika i hasła
* **Uwierzytelnianie usługi Azure Active Directory**, które korzysta z tożsamości zarządzanej przez usługę Azure Active Directory i jest obsługiwane w przypadku zarządzanych i zintegrowanych domen

Po utworzeniu serwera logicznego bazy danych należy określić nazwę logowania „server admin” przy użyciu nazwy użytkownika i hasła. Przy użyciu tych poświadczeń można wybrać metodę uwierzytelniania w każdej innej bazie danych na tym serwerze jako jej właściciel, czyli „dbo”. Aby skorzystać z uwierzytelniania przy użyciu usługi Azure Active Directory, należy utworzyć innego administratora serwera o nazwie „Azure AD admin”, co umożliwi administrowanie użytkownikami i grupami usługi Azure AD. Ten administrator może również wykonywać wszystkie operacje, które może wykonać zwykły administrator serwera. Zobacz artykuł [Connecting to SQL Database By Using Azure Active Directory Authentication](sql-database-aad-authentication.md) (Łączenie z usługą SQL Database przy użyciu uwierzytelniania usługi Azure Active Directory), aby poznać krok po kroku sposób tworzenia administratora usługi Azure AD w celu włączenia uwierzytelniania usługi Azure Active Directory.

Zalecanym najlepszym rozwiązaniem jest używanie przez daną aplikację osobnego konta do uwierzytelniania — w ten sposób można ograniczyć uprawnienia nadane aplikacji i zmniejszyć ryzyko złośliwych działań w przypadku, gdy kod aplikacji jest narażony na atak polegający na wstrzyknięciu kodu SQL. Zalecaną metodą jest utworzenie [użytkownika zawartej bazy danych](https://msdn.microsoft.com/library/ff929188), co umożliwia aplikacji bezpośrednie uwierzytelnianie w bazie danych. Można utworzyć użytkownika zawartej bazy danych, który używa uwierzytelniania programu SQL, wykonując następujące polecenie języka T-SQL po nawiązaniu połączenia z bazą danych użytkownika jako administrator serwera:

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password'; -- SQL Authentication
```

Jeśli utworzono administratora usługi Azure AD, można utworzyć użytkownika zawartej bazy danych, który używa uwierzytelniania usługi Azure Active Directory, wykonując następujące polecenie języka T-SQL po nawiązaniu połączenia z bazą danych użytkownika jako administrator usługi Azure AD:

```
CREATE USER [Azure_AD_principal_name | Azure_AD_group_display_name] FROM EXTERNAL PROVIDER; -- Azure Active Directory Authentication
```

W obu przypadkach parametry połączenia aplikacji używane podczas łączenia się z bazą danych muszą określać poświadczenia użytkownika zamiast nazwy logowania administratora serwera.

Aby uzyskać więcej informacji o uwierzytelnianiu w usłudze SQL Database, zobacz artykuł [Managing Databases and Logins in Azure SQL Database](sql-database-manage-logins.md) (Zarządzanie bazami danych i nazwami logowania w usłudze Azure SQL Database).

## <a name="authorization"></a>Autoryzacja
Autoryzacja określa funkcje możliwe do wykonania w usłudze Azure SQL Database, kontrolowane za pomocą członkostwa i uprawnień roli konta użytkownika. Zalecanym najlepszym rozwiązaniem jest przyznanie użytkownikom minimalnych niezbędnych uprawnień. Usługa Azure SQL Database umożliwia zarządzanie rolami przy użyciu języka T-SQL:

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

Konto administratora serwera, za pomocą którego nawiązujesz połączenie, jest członkiem roli db_owner, która ma uprawnienia do wykonywania wszystkich funkcji w bazie danych. Zapisz to konto, aby móc wdrażać uaktualnienia schematów i wykonywać inne operacje zarządzania. Używaj konta „ApplicationUser” z bardziej ograniczonymi uprawnienia do nawiązywania połączenia pomiędzy swoją aplikacją a bazą danych aplikacji, korzystając z minimalnych uprawnień wymaganych przez aplikację.

Istnieją następujące sposoby dalszego ograniczenia operacji wykonywanych przez użytkownika w usłudze Azure SQL Database:

* [Role bazy danych](https://msdn.microsoft.com/library/ms189121) inne niż db_datareader i db_datawriter mogą służyć do tworzenia bardziej zaawansowanych kont użytkowników aplikacji lub mniej zaawansowanych kont zarządzania.
* Szczegółowe [uprawnienia](https://msdn.microsoft.com/library/ms191291) umożliwiają kontrolę operacji, które można wykonać na poszczególnych kolumnach, tabelach, widokach, procedurach i innych obiektach bazy danych.
* [Personifikacji](https://msdn.microsoft.com/library/vstudio/bb669087) i [podpisywania modułów](https://msdn.microsoft.com/library/bb669102) można używać do bezpiecznego tymczasowego podnoszenia poziomu uprawnień.
* [Zabezpieczeń na poziomie wiersza](https://msdn.microsoft.com/library/dn765131) można używać do ograniczania zbioru wierszy, do których użytkownik może uzyskać dostęp.
* [Maskowania danych](sql-database-dynamic-data-masking-get-started.md) można używać do ograniczania ujawniania danych wrażliwych.
* [Procedury składowane](https://msdn.microsoft.com/library/ms190782) umożliwiają ograniczenie czynności wykonywanych w bazie danych.

Zarządzanie bazami danych i serwerami logicznymi z klasycznego portalu Azure lub za pomocą interfejsu API usługi Azure Resource Manager jest kontrolowane przez przypisania ról do konta użytkownika portalu. Aby uzyskać więcej informacji na ten temat, zobacz artykuł [Role-based access control in Azure Portal](../active-directory/role-based-access-control-configure.md) (Kontrola dostępu oparta na rolach w witrynie Azure Portal).

## <a name="encryption"></a>Szyfrowanie
Usługa Azure SQL Database ułatwia ochronę danych, szyfrując je, gdy pozostają nieużywane lub są przechowywane w plikach bazy danych i kopii zapasowych, przy użyciu funkcji [Transparent Data Encryption](http://go.microsoft.com/fwlink/?LinkId=526242). Aby zaszyfrować bazę danych, połącz się jako właściciel bazy danych i wykonaj polecenie:

```
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

Można również rozważyć inne sposoby szyfrowania swoich poufnych danych:

* [Szyfrowanie na poziomie komórki](https://msdn.microsoft.com/library/ms179331.aspx) służące do szyfrowania określonej kolumny lub nawet poszczególnych komórek danych przy użyciu różnych kluczy szyfrowania.
* Jeśli potrzebujesz sprzętowego modułu zabezpieczeń lub centralnego zarządzania hierarchią kluczy szyfrowania, rozważ użycie [funkcji Azure Key Vault w usłudze SQL Server w maszynie wirtualnej Azure](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).
* Funkcja [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) (w wersji zapoznawczej) powoduje, że szyfrowanie staje się przezroczyste dla aplikacji, i umożliwia klientom szyfrowanie poufnych danych w aplikacjach klienta bez udostępniania kluczy szyfrowania usłudze SQL Database.

## <a name="auditing"></a>Inspekcja
Inspekcja i śledzenie zdarzeń bazy danych mogą pomóc zachować zgodność z przepisami oraz zidentyfikować podejrzane działania. Usługa SQL Database Auditing śledzi zdarzenia bazy danych i zapisuje je w dzienniku inspekcji na koncie usługi Azure Storage. Usługa SQL Database Auditing jest również zintegrowana z usługą Microsoft Power BI, co ułatwia wykonywanie szczegółowych raportów i analiz. Aby uzyskać więcej informacji, zobacz artykuł [Get started with SQL Database Auditing](sql-database-auditing-get-started.md) (Wprowadzenie do usługi SQL Database Auditing).

Usługa SQL Database Threat Detection stanowi dodatkową warstwę zabezpieczeń powyżej warstwy inspekcji. Usługa umożliwia reagowanie na zagrożenia od razu po ich wystąpieniu, generując alerty zabezpieczeń w przypadku podejrzanych działań. Aby uzyskać więcej informacji, zobacz artykuł [Get started with SQL Database Threat Detection](sql-database-threat-detection-get-started.md) (Wprowadzenie do usługi SQL Database Threat Detection).  

## <a name="compliance"></a>Zgodność
Oprócz powyższych funkcji i funkcjonalności, które mogą pomóc aplikacji spełnić rozmaite wymagania dotyczące zgodności zabezpieczeń, usługa Azure SQL Database uczestniczy w regularnych inspekcjach i posiada certyfikaty zgodności z wieloma standardami zgodności. Aby uzyskać więcej informacji, zobacz witrynę [Centrum zaufania Microsoft Azure](https://azure.microsoft.com/support/trust-center/), w której można znaleźć aktualną listę [certyfikatów zgodności usługi SQL Database](https://azure.microsoft.com/support/trust-center/services/).

## <a name="next-steps"></a>Następne kroki

- Aby zapoznać się z przeglądem wszystkich funkcji zabezpieczeń usługi SQL Database, zobacz [omówienie zabezpieczeń usługi SQL](sql-database-security-overview.md).
- Omówienie korzystania z funkcji kontroli dostępu w usłudze SQL Database zawiera artykuł dotyczący [kontroli dostępu](sql-database-control-access.md).
- Omówienie aktywnego monitorowania zawierają artykuły z [wprowadzeniem do funkcji inspekcji usługi SQL Database](sql-database-auditing-get-started.md) i [wprowadzeniem do funkcji wykrywania zagrożeń usługi SQL Database](sql-database-threat-detection-get-started.md).




<!--HONumber=Dec16_HO4-->


