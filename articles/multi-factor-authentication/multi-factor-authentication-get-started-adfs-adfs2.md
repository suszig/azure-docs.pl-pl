---
title: "Używanie serwera usługi Azure MFA z usługami AD FS 2.0 | Microsoft Docs"
description: "Ta strona dotyczy usługi Azure Multi-Factor Authentication i zawiera informacje umożliwiające rozpoczęcie korzystania z usługi Azure MFA i usług AD FS 2.0."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 96168849-241a-4499-a224-d829913caa7e
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/14/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 7ad06e9086d6532b3de2ba060b37424051f022c2


---
# <a name="secure-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-20"></a>Zabezpieczanie zasobów w chmurze i zasobów lokalnych przy użyciu serwera usługi Azure Multi-Factor Authentication i usług AD FS 2.0
Ten artykuł jest przeznaczony dla organizacji, które są sfederowane z usługą Azure Active Directory i chcą zabezpieczyć zasoby przechowywane lokalnie i w chmurze. Zasoby można chronić, używając serwera usługi Azure Multi-Factor Authentication i konfigurując go do pracy z usługami AD FS w taki sposób, aby weryfikacja dwuetapowa była wymagana dla ważnych punktów końcowych.

Ta dokumentacja dotyczy korzystania z serwera usługi Azure Multi-Factor Authentication z usługami AD FS 2.0.  Dowiedz się więcej na temat [zabezpieczania zasobów w chmurze i lokalnych przy użyciu serwera usługi Azure Multi-Factor Authentication i usług AD FS systemu Windows Server 2012 R2](multi-factor-authentication-get-started-adfs-w2k12.md).

## <a name="secure-ad-fs-20-with-a-proxy"></a>Zabezpieczanie usługi AD FS 2.0 przy użyciu serwera proxy
Aby zabezpieczyć usługi AD FS 2.0 za pomocą serwera proxy, zainstaluj serwer usługi Azure Multi-Factor Authentication na serwerze proxy usług AD FS i skonfiguruj go.

### <a name="configure-iis-authentication"></a>Konfigurowanie uwierzytelniania usług IIS
1. Na serwerze usługi Azure Multi-Factor Authentication kliknij ikonę **Uwierzytelnianie usług IIS** w menu po lewej stronie.
2. Kliknij kartę **Oparte na formularzach**.
3. Kliknij przycisk **Dodaj...** Dodaj...
   <center>![Konfiguracja](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>
4. Aby automatycznie wykryć zmienne nazwy użytkownika, hasła i domeny, wprowadź adres URL logowania (taki jak https://sso.contoso.com/adfs/ls) w oknie dialogowym Konfigurowana automatycznie witryna sieci Web oparta na formularzach i kliknij przycisk OK.
5. Zaznacz pole **Wymagaj dopasowania użytkownika usługi Azure Multi-Factor Authentication**, jeśli wszyscy użytkownicy zostali lub zostaną zaimportowani na serwer i będą podlegać weryfikacji dwuetapowej. Jeśli znaczna liczba użytkowników nie została jeszcze zaimportowana na serwer lub będzie wykluczona z weryfikacji dwuetapowej, należy pozostawić to pole puste. Aby uzyskać dodatkowe informacje na temat tej funkcji, zapoznaj się z plikiem pomocy.
6. Jeśli nie można automatycznie wykryć zmiennych strony, kliknij przycisk **Określ ręcznie...** w oknie dialogowym Automatyczna konfiguracja witryny sieci Web opartej na formularzach.
7. W oknie dialogowym Dodawanie witryny sieci Web opartej na formularzach wprowadź adres URL strony logowania usług AD FS (taki jak https://sso.contoso.com/adfs/ls) w polu Adres URL przesyłania, a następnie wprowadź nazwę aplikacji (opcjonalnie). Nazwa aplikacji jest widoczna w raportach usługi Azure Multi-Factor Authentication i może być wyświetlana w uwierzytelniających wiadomościach SMS lub wiadomościach aplikacji mobilnej. Więcej informacji dotyczących adresu URL przesyłania można znaleźć w pliku pomocy.
8. Ustaw format żądania na wartość „POST” lub „GET”.
9. Wprowadź wartość zmiennej nazwy użytkownika (ctl00$ContentPlaceHolder1$UsernameTextBox) i wartość zmiennej hasła (ctl00$ContentPlaceHolder1$PasswordTextBox). Jeśli na stronie logowania opartej na formularzach jest wyświetlane pole tekstowe domeny, wprowadź również wartość zmiennej domeny. W celu znalezienia nazw pól wejściowych na stronie logowania może być konieczne przejście na tę stronę w przeglądarce sieci Web, kliknięcie prawym przyciskiem myszy i wybranie polecenia **Wyświetl źródło**.
10. Zaznacz pole **Wymagaj dopasowania użytkownika usługi Azure Multi-Factor Authentication**, jeśli wszyscy użytkownicy zostali lub zostaną zaimportowani na serwer i będą podlegać weryfikacji dwuetapowej. Jeśli znaczna liczba użytkowników nie została jeszcze zaimportowana na serwer lub będzie wykluczona z weryfikacji dwuetapowej, należy pozostawić to pole puste.
    <center>![Konfiguracja](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Kliknij przycisk **Zaawansowane...**, aby przejrzeć ustawienia zaawansowane. Możesz skonfigurować ustawienia obejmujące możliwość wybierania niestandardowego pliku strony odmowy, buforowanie pomyślnych uwierzytelnień w witrynie sieci Web za pomocą plików cookie oraz sposób uwierzytelniania przy użyciu podstawowych poświadczeń.
12. Ponieważ serwer proxy usług AD FS przypuszczalnie nie zostanie przyłączony do domeny, do nawiązywania połączenia z kontrolerem domeny w celu importowania i wstępnego uwierzytelniania użytkowników możesz użyć protokołu LDAP. W oknie dialogowym Zaawansowana witryna sieci Web oparta na formularzach kliknij kartę **Uwierzytelnianie podstawowe** i wybierz opcję **Powiązanie z protokołem LDAP** jako typ uwierzytelniania wstępnego.
13. Po wykonaniu tej czynności kliknij przycisk **OK**, aby wrócić do okna dialogowego Dodawanie witryny sieci Web opartej na formularzach. Więcej informacji dotyczących ustawień zaawansowanych można znaleźć w pliku pomocy.
14. Kliknij przycisk **OK**, aby zamknąć okno dialogowe.
15. Po wykryciu lub wprowadzeniu adresu URL i zmiennych na stronie dane witryny sieci Web wyświetlane są w panelu Oparte na formularzach.
16. Aby skonfigurować odpowiedni poziom wtyczki IIS, kliknij kartę **Moduł macierzysty** i wybierz serwer, witrynę sieci Web, w ramach której działa serwer proxy usług AD FS (taką jak „Domyślna witryna sieci Web”), lub aplikację serwera proxy usług AD FS (taką jak „ls” w obszarze „adfs”).
17. Kliknij pole **Włącz uwierzytelnianie usług IIS** w górnej części ekranu.
18. Uwierzytelnianie usług IIS zostało włączone.

### <a name="configure-directory-integration"></a>Konfigurowanie integracji katalogu
Uwierzytelnianie usług IIS zostało włączone, ale aby przeprowadzić uwierzytelnianie wstępne w usłudze Active Directory (AD) za pośrednictwem protokołu LDAP, musisz skonfigurować połączenie protokołu LDAP z kontrolerem domeny.

1. Kliknij ikonę **Integracja katalogu**.
2. Na karcie Ustawienia wybierz przycisk radiowy **Użyj określonej konfiguracji LDAP**.
   <center>![Konfiguracja](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>
3. Kliknij przycisk **Edytuj...** Dodaj...
4. W oknie dialogowym Edycja konfiguracji LDAP wypełnij pola informacjami wymaganymi do nawiązania połączenia z kontrolerem domeny usługi AD. W poniższej tabeli znajdują się opisy pól. Te informacje znajdują się również w pliku pomocy usługi Serwer Azure Multi-Factor Authentication.
5. Przetestuj połączenie LDAP, klikając przycisk **Testuj**.
   <center>![Konfiguracja](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>
6. Jeśli test połączenia LDAP zakończył się pomyślnie, kliknij przycisk **OK**.

### <a name="configure-company-settings"></a>Konfigurowanie ustawień firmy
1. Następnie kliknij ikonę **Ustawienia firmy** i wybierz kartę **Rozpoznawanie nazwy użytkownika**.
2. Wybierz przycisk radiowy **Użyj atrybutu unikatowego identyfikatora LDAP w celu dopasowania nazw użytkowników**.
3. Jeśli użytkownicy wprowadzają swoje nazwy użytkowników w formacie „domena\nazwa_użytkownika”, serwer musi być w stanie oddzielić domenę od nazwy użytkownika podczas tworzenia zapytania LDAP. Można to skonfigurować za pomocą ustawienia rejestru.
4. W przypadku serwera 64-bitowego otwórz Edytor rejestru i przejdź do klucza HKEY_LOCAL_MACHINE/SOFTWARE/Wow6432Node/Positive Networks/PhoneFactor. W przypadku serwera 32-bitowego klucz nie zawiera elementu „Wow6432Node”. Utwórz klucz rejestru typu DWORD o nazwie „UsernameCxz_stripPrefixDomain” i ustaw jego wartość na 1. Po wykonaniu tych czynności serwer proxy usług AD FS jest chroniony przez usługę Azure Multi-Factor Authentication.

Upewnij się, że zaimportowano użytkowników z usługi Active Directory na serwer. Jeśli chcesz umieścić wewnętrzne adresy IP na liście dozwolonych adresów, tak aby weryfikacja dwuetapowa nie była wymagana podczas logowania się do witryny sieci Web z tych lokalizacji, zobacz sekcję [Zaufane adresy IP](#trusted-ips).

<center>![Konfiguracja](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## <a name="ad-fs-20-direct-without-a-proxy"></a>Bezpośrednie używanie usług AD FS 2.0 bez serwera proxy
Jeśli serwer proxy usług AD FS nie jest używany, można zabezpieczyć usługi AD FS. Zainstaluj serwer usługi Azure Multi-Factor Authentication na serwerze usług AD FS i skonfiguruj go, wykonując następujące kroki:

1. Na serwerze usługi Azure Multi-Factor Authentication kliknij ikonę **Uwierzytelnianie usług IIS** w menu po lewej stronie.
2. Kliknij kartę **HTTP**.
3. Kliknij przycisk **Dodaj...** Dodaj...
4. W oknie dialogowym Dodawanie podstawowego adresu URL w polu Podstawowy adres URL wprowadź adres URL witryny sieci Web usług AD FS, w której jest przeprowadzane uwierzytelnianie HTTP (taki jak https://sso.domain.com/adfs/ls/auth/integrated). Następnie wprowadź nazwę aplikacji (opcjonalnie). Nazwa aplikacji jest widoczna w raportach usługi Azure Multi-Factor Authentication i może być wyświetlana w uwierzytelniających wiadomościach SMS lub wiadomościach aplikacji mobilnej.
5. W razie potrzeby dostosuj wartości w polach Limit czasu bezczynności i Maksymalna długość sesji.
6. Zaznacz pole **Wymagaj dopasowania użytkownika usługi Azure Multi-Factor Authentication**, jeśli wszyscy użytkownicy zostali lub zostaną zaimportowani na serwer i będą podlegać weryfikacji dwuetapowej. Jeśli znaczna liczba użytkowników nie została jeszcze zaimportowana na serwer lub będzie wykluczona z weryfikacji dwuetapowej, należy pozostawić to pole puste. Aby uzyskać dodatkowe informacje na temat tej funkcji, zapoznaj się z plikiem pomocy.
7. W razie potrzeby zaznacz pole użycia plików cookie do buforowania.
   <center>![Konfiguracja](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>
8. Kliknij przycisk **OK**.
9. Aby włączyć wtyczkę usług IIS na odpowiednim poziomie, kliknij kartę **Moduł macierzysty** i wybierz serwer, witrynę sieci Web (taką jak „Domyślna witryna sieci Web”) lub aplikację usług AD FS (taką jak „ls” w obszarze „adfs”).
10. Kliknij pole **Włącz uwierzytelnianie usług IIS** w górnej części ekranu. Po wykonaniu tych czynności usługi AD FS są chronione przez usługę Azure Multi-Factor Authentication.

Upewnij się, że zaimportowano użytkowników z usługi Active Directory na serwer. Jeśli chcesz umieścić wewnętrzne adresy IP na liście dozwolonych adresów, tak aby weryfikacja dwuetapowa nie była wymagana podczas logowania się do witryny sieci Web z tych lokalizacji, zobacz sekcję Zaufane adresy IP.

## <a name="trusted-ips"></a>Zaufane adresy IP
Zaufane adresy IP pozwalają użytkownikom pominąć uwierzytelnianie za pomocą usługi Azure Multi-Factor Authentication w przypadku żądań witryn sieci Web pochodzących z określonych adresów IP lub podsieci. Można na przykład chcieć zwolnić użytkowników z konieczności przechodzenia weryfikacji dwuetapowej, jeśli logują się z biura. W tym celu należy określić podsieć biura jako wpis z zaufanymi adresami IP.

### <a name="to-configure-trusted-ips"></a>Aby skonfigurować zaufane adresy IP
1. W sekcji Uwierzytelnianie usług IIS kliknij kartę **Zaufane adresy IP**.
2. Kliknij przycisk **Dodaj...** Dodaj...
3. Gdy pojawi się okno dialogowe Dodawanie zaufanych adresów IP, wybierz jeden z przycisków radiowych: **Pojedynczy adres IP**, **Zakres adresów IP** albo **Podsieć**.
4. Wprowadź adres IP, zakres adresów IP lub podsieć, którą chcesz dodać do listy dozwolonych adresów. Jeśli wprowadzasz podsieć, wybierz odpowiednią maskę sieci i kliknij przycisk **OK**. Po wykonaniu tych czynności został dodany zaufany adres IP.

<center>![Konfiguracja](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>



<!--HONumber=Dec16_HO1-->


