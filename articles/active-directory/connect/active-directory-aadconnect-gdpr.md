---
title: "Azure AD Connect i rozporządzenia ochrony danych ogólne | Dokumentacja firmy Microsoft"
description: "Ten dokument zawiera opis sposobu uzyskiwania GDPR zgodności z programem Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.author: billmath
ms.openlocfilehash: c3956dd379961b119f65bdebe1f5a8038c4fa8f0
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="gdpr-compliance-and-azure-ad-connect"></a>GDPR zgodności i Azure AD Connect 

W 2018 maja, prawa Europejskiego prywatności, [rozporządzenia ogólne ochrony danych (GDPR)](http://ec.europa.eu/justice/data-protection/reform/index_en.htm), przypada zaczęły obowiązywać. GDPR nakłada nowe zasady dotyczące firmy, agencji rządowych z systemem innym niż zysków i innymi organizacjami, że oferta towarów i usług do osób w Unii Europejskiej (UE), lub że zbieranie i analizowanie danych powiązane mieszkańców Unii Europejskiej. Stosuje GDPR niezależnie od tego, w którym znajduje się. 

Usług i produktów firmy Microsoft dostępnych dzisiaj ułatwić spełniają wymagania GDPR. Dowiedz się więcej o zasady Privacy firmy Microsoft pod adresem [Centrum zaufania](https://www.microsoft.com/trustcenter)

>[!NOTE] 
>Ten artykuł dotyczy usługi Azure AD Connect i GDPR zgodności.  Aby uzyskać informacje dotyczące usługi Azure AD Connect Health i GDPR zgodności, zobacz artykuł [tutaj](../../active-directory/connect-health/active-directory-aadconnect-health-gdpr.md).

Ogólne dane zgodności rozporządzenia ochrony w przypadku instalacji usługi Azure AD Connect można połączyć się z dwóch sposobów:

1.  Na żądanie wyodrębniania danych osoby i usuwanie danych z tej osoby z instalacji
2.  Upewnij się, że żadne dane nie są przechowywane poza 48 godzin.

Zespół usługi Azure AD Connect zaleca druga opcja, ponieważ jest znacznie łatwiejsze do wdrożenia i konserwacji.

Następujące dane, który znajduje się w zakresie zgodności GDPR przechowywane na serwerze synchronizacji Azure AD Connect:
1.  Dane dotyczące osoby w **bazy danych Azure AD Connect**
2.  Dane w **dziennika zdarzeń systemu Windows** pliki zawierające informacje dotyczące osoby.
3.  Dane w **pliki dziennika instalacji Azure AD Connect** zawierających temat osoby

GDPR zgodne, klientów Azure AD Connect powinny używać następujących wytycznych:
1.  Usuń zawartość folderu, który zawiera pliki dziennika instalacji Azure AD Connect na bieżąco — co najmniej raz 48 godzin.
2.  Ten produkt może także utworzyć dzienników zdarzeń.  Aby dowiedzieć się więcej o dziennikach dzienników zdarzeń, zobacz [dokumentacji tutaj](https://msdn.microsoft.com/library/windows/desktop/aa385780.aspx).

Dane dotyczące osoby zostanie automatycznie usunięta z bazy danych programu Azure AD Connect po usunięciu tej osoby danych z którego pochodzi z systemu źródłowego. Żadna konkretna akcja ze strony administratorów musi być zgodny z GDPR.  Jednak wymaga, aby danych Azure AD Connect jest zsynchronizowany z źródła danych, co co najmniej dwa dni.

## <a name="delete-the-azure-ad-connect-installation-log-file-folder-contents"></a>Usuń zawartość folderu pliku dziennika instalacji Azure AD Connect
Regularnie Sprawdź i usunąć zawartość **c:\programdata\aadconnect** folderu — z wyjątkiem **PersistedState.Xml** pliku. Ten plik zachowuje swój stan poprzedniej instalacji programu Azure A Connect i jest używany podczas instalacji uaktualnienia. Ten plik nie zawiera żadnych danych dotyczących osoby i nie należy go usunąć.

>[!IMPORTANT]
>Nie należy usuwać pliku PersistedState.xml.  Ten plik nie zawiera żadnych informacji użytkownika i przechowuje informacje o stanie poprzedniej instalacji.

Możesz przejrzeć i usunąć te pliki przy użyciu Eksploratora Windows lub można wykonać niezbędne czynności skryptu, takiego jak następujące:


```
$Files = ((Get-childitem -Path "$env:programdata\aadconnect" -Recurse).VersionInfo).FileName
Foreach ($file in $files) {
If ($File.ToUpper() -ne "$env:programdata\aadconnect\PERSISTEDSTATE.XML".toupper()) # Do not delete this file
    {Remove-Item -Path $File -Force}
    } 
```

### <a name="schedule-this-script-to-run-every-48-hours"></a>Planowanie tego skryptu do uruchomienia co 48 godzin
Wykonaj następujące kroki, aby zaplanować uruchomienie skryptu co 48 godzin.

1.  Zapisz skrypt w pliku z rozszerzeniem **&46; PS1**, otwórz Panel sterowania i kliknij pozycję **systemów i zabezpieczeń**.
    ![System](media\active-directory-aadconnect-gdpr\gdpr2.png)

2.  W pozycji Narzędzia administracyjne kliknij **harmonogram zadań**.
    ![Zadanie podrzędne](media\active-directory-aadconnect-gdpr\gdpr3.png)
3.  W harmonogramie zadań, kliknij prawym przyciskiem myszy **Biblioteka Harmonogramu zadań** i wybierz polecenie **Tworzenie podstawowych zadań...**
4.  Wprowadź nazwę dla nowego zadania, a następnie kliknij przycisk **dalej**.
5.  Wybierz **codzienne** wyzwalacz zadania i kliknij na **dalej**.
6.  Wartość cyklu **2 dni** i kliknij przycisk **dalej**.
7.  Wybierz **uruchomić program** jako akcji, a następnie kliknij polecenie **dalej**.
8.  Typ **PowerShell** pole dla skryptu lub programu, a w polu z etykietą **Dodaj argumenty (opcjonalnie)**, wprowadź pełną ścieżkę do skryptu, który został utworzony wcześniej, a następnie kliknij przycisk **dalej**.
9.  Następnym ekranie zawiera podsumowanie informacji o zadaniu, które zostały utworzone. Sprawdź wartości, a następnie kliknij przycisk **Zakończ** do utworzenia zadania.



## <a name="next-steps"></a>Kolejne kroki
- [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).
- [Azure AD Connect Health i GDPR](../../active-directory/connect-health/active-directory-aadconnect-health-gdpr.md)
