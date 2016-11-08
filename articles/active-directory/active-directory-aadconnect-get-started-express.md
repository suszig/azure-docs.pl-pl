---
title: 'Azure AD Connect: rozpoczynanie pracy przy użyciu ustawień ekspresowych | Microsoft Docs'
description: Dowiedz się, jak pobrać, zainstalować i uruchomić kreatora instalacji programu Azure AD Connect.
services: active-directory
documentationcenter: ''
author: andkjell
manager: stevenpo
editor: curtand

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/27/2016
ms.author: billmath;andkjell

---
# Rozpoczynanie pracy z programem Azure AD Connect przy użyciu ustawień ekspresowych
Użyj **ustawień ekspresowych** programu Azure AD Connect, jeśli używasz topologii z jednym lasem oraz [synchronizacji haseł](active-directory-aadconnectsync-implement-password-synchronization.md) na potrzeby uwierzytelniania. **Ustawienia ekspresowe** to opcja domyślna używana w przypadku najbardziej typowych scenariuszy wdrożenia. Wystarczy kilka kliknięć, aby rozszerzyć katalog lokalny do chmury.

Przed rozpoczęciem instalacji należy [pobrać program Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) i wykonać czynności związane z wymaganiami wstępnymi opisane w temacie [Azure AD Connect: sprzęt i wymagania wstępne](active-directory-aadconnect-prerequisites.md).

Jeśli ustawienia ekspresowe nie są odpowiednie do Twojej topologii, zobacz [dokumentację pokrewną](#related-documentation), aby zapoznać się z innymi scenariuszami.

## Ekspresowa instalacja programu Azure AD Connect
Możesz zobaczyć te czynności w praktyce, korzystając z sekcji [Filmy wideo](#videos).

1. Zaloguj się jako administrator lokalny na serwerze, na którym chcesz zainstalować program Azure AD Connect. Powinien być to serwer, który ma być serwerem synchronizacji.
2. Przejdź do pozycji **AzureADConnect.msi** i kliknij ją dwukrotnie.
3. Na ekranie powitalnym zaznacz pole wyrażenia zgody na warunki licencji i kliknij pozycję **Kontynuuj**.  
4. Na ekranie Ustawienia ekspresowe kliknij polecenie **Użyj ustawień ekspresowych**.  
   ![Wprowadzenie do programu Azure AD Connect](./media/active-directory-aadconnect-get-started-express/express.png)
5. Na ekranie Łączenie z usługą Azure AD wprowadź nazwę użytkownika i hasło administratora globalnego usługi Azure AD. Kliknij przycisk **Dalej**.  
   ![Łączenie z usługą Azure AD](./media/active-directory-aadconnect-get-started-express/connectaad.png) Jeśli wystąpi błąd lub problem z łącznością, zobacz [Rozwiązywanie problemów z łącznością](active-directory-aadconnect-troubleshoot-connectivity.md).
6. Na ekranie Łączenie z usługami AD DS wprowadź nazwę użytkownika i hasło konta administratora przedsiębiorstwa. Możesz wprowadzić domenę w formacie NetBios lub FQDN, czyli FABRIKAM\administrator lub fabrikam.com\administrator. Kliknij przycisk **Dalej**.  
   ![Łączenie z usługami AD DS](./media/active-directory-aadconnect-get-started-express/connectad.png)
7. Strona [**Konfiguracja logowania w usłudze Azure AD**](active-directory-aadconnect-user-signin.md#azure-ad-sign-in-configuration) będzie wyświetlana tylko w przypadku, gdy nie ukończono [weryfikowania domen](active-directory-add-domain.md) w ramach [wymagań wstępnych](active-directory-aadconnect-prerequisites.md).
   ![Niezweryfikowane domeny](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png)  
   Jeśli zobaczysz tę stronę, sprawdź wszystkie domeny z oznaczeniem **Nie dodano** lub **Nie zweryfikowano**. Upewnij się, że używane domeny zostały zweryfikowane w usłudze Azure AD. Po zweryfikowaniu domen kliknij symbol Odśwież.
8. Na ekranie Wszystko gotowe do skonfigurowania kliknij pozycję **Zainstaluj**.
   * Opcjonalnie można na stronie Wszystko gotowe do skonfigurowania usunąć zaznaczenie pola wyboru **Uruchom proces synchronizacji, gdy tylko konfiguracja zostanie ukończona**. Usuń zaznaczenie tego pola wyboru, jeśli planujesz dodatkową konfigurację, na przykład konfigurację [filtrowania](active-directory-aadconnectsync-configure-filtering.md). Jeśli ta opcja nie będzie zaznaczona, kreator skonfiguruje funkcję synchronizacji, ale nie włączy harmonogramu. Synchronizacja nie zostanie uruchomiona do momentu włączenia jej ręcznie przez [ponowne uruchomienie kreatora instalacji](active-directory-aadconnectsync-installation-wizard.md).
   * Jeśli korzystasz z programu Exchange z lokalną usługą Active Directory, możesz również włączyć [**Wdrożenie hybrydowe programu Exchange**](https://technet.microsoft.com/library/jj200581.aspx). Włącz tę opcję, jeśli chcesz, aby skrzynki pocztowe programu Exchange były przechowywane jednocześnie zarówno w chmurze, jak i lokalnie.
     ![Wszystko gotowe do skonfigurowania programu Azure AD Connect](./media/active-directory-aadconnect-get-started-express/readytoconfigure.png)
9. Po zakończeniu instalacji kliknij przycisk **Zakończ**.
10. Po zakończeniu instalacji wyloguj się, a następnie zaloguj się ponownie przed użyciem narzędzia Synchronization Service Manager lub Synchronization Rule Editor.

## Filmy wideo
Aby obejrzeć film wideo na temat instalacji ekspresowej, zobacz:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Express-Settings/player]
> 
> 

## Następne kroki
Po zainstalowaniu programu Azure AD Connect możesz [zweryfikować instalację i przypisać licencje](active-directory-aadconnect-whats-next.md).

Dowiedz się więcej na temat funkcji włączonych w ramach instalacji: [Automatyczne uaktualnianie](active-directory-aadconnect-feature-automatic-upgrade.md), [Zapobieganie przypadkowemu usuwaniu](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) oraz [Azure AD Connect Health](active-directory-aadconnect-health-sync.md).

Dowiedz się więcej na te popularne tematy: [harmonogram i sposób włączania synchronizacji](active-directory-aadconnectsync-feature-scheduler.md).

Dowiedz się więcej na temat [integrowania tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).

## Dokumentacja pokrewna
| Temat |
| --- | --- |
| Omówienie programu Azure AD Connect |
| Instalowanie przy użyciu ustawień dostosowanych |
| Uaktualnianie przy użyciu narzędzia DirSync |
| Konta używane do instalacji |

<!--HONumber=sep16_HO1-->


