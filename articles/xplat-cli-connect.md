---
title: Logowanie do platformy Azure z poziomu interfejsu wiersza polecenia | Dokumentacja firmy Microsoft
description: "Połącz z subskrypcją platformy Azure z interfejsu wiersza polecenia platformy Azure (Azure CLI) dla komputerów Mac, Linux i Windows"
editor: tysonn
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: ed856527-d75e-4e16-93fb-253dafad209d
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: rasquill
"\"/": 
ms.openlocfilehash: 31efab60690b54faf7992251fcd01e307c4464f2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="log-in-to-azure-from-the-azure-cli"></a>Logowanie do platformy Azure z interfejsu wiersza polecenia platformy Azure
Interfejsu wiersza polecenia Azure to zestaw poleceń open source, obsługujący wiele platform do pracy z zasobów platformy Azure. W tym artykule opisano różne sposoby poświadczenia konta Azure interfejsu wiersza polecenia Azure nawiązać połączenia z subskrypcją platformy Azure:

* Uruchom `azure login` polecenia interfejsu wiersza polecenia do uwierzytelniania za pomocą usługi Azure Active Directory. Ta metoda umożliwia dostęp do poleceń interfejsu wiersza polecenia w obu [polecenia tryby](#cli-command-modes). Po uruchomieniu polecenia bez dodatkowych opcji `azure login` wyświetli monit o kontynuowanie interakcyjnie zalogować się za pośrednictwem portalu sieci web. Aby uzyskać dodatkowe `azure login` polecenia opcji, zobacz scenariusze w tym artykule, lub typ `azure login --help`.
* Jeśli musisz używać polecenia interfejsu wiersza polecenia w trybie zarządzania usługami Azure (nie zalecane dla większości nowych wdrożeń), można pobrać i zainstalować plik ustawień publikowania na tym komputerze.

Jeśli nie została jeszcze zainstalowana interfejsu wiersza polecenia, zobacz [instalowanie interfejsu wiersza polecenia Azure](cli-install-nodejs.md). Jeśli nie masz subskrypcji Azure, możesz utworzyć [bezpłatne konto](http://azure.microsoft.com/free/) w zaledwie kilka minut.

Aby uzyskać ogólne informacje o tożsamości z innego konta i subskrypcji platformy Azure, zobacz [jak subskrypcje platformy Azure są kojarzone z usługi Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="scenario-1-azure-login-with-interactive-login"></a>Scenariusz 1: azure logowania przy użyciu logowania interakcyjnego
Z określonymi kontami interfejsu wiersza polecenia wymaga uruchomienia `azure login` , a następnie kontynuuj proces logowania przy użyciu przeglądarki sieci web za pośrednictwem portalu sieci web, w procesie nazywanym *logowania interakcyjnego*. Typową przyczyną jest, jeśli masz konto służbowe (nazywane również *konta organizacyjnego*) skonfigurowane wymaganie uwierzytelniania wieloskładnikowego. Za pomocą logowania interaktywnego Twojego konta Microsoft, należy użyć polecenia w trybie Menedżera zasobów.

Interakcyjne logowanie jest łatwe: typ `azure login` — bez żadnych opcji--jak pokazano w poniższym przykładzie:

```
azure login
```                                                                                             

Dane wyjściowe zostanie wyświetlony ekran podobny do następującego:

```         
info:    Executing command login
info:    To sign in, use a web browser to open the page http://aka.ms/devicelogin. Enter the code XXXXXXXXX to authenticate.
```
Skopiuj kod oferowanych w danych wyjściowych polecenia, a następnie otwórz przeglądarkę do http://aka.ms/devicelogin lub inne strony, jeśli określony. (Aby otworzyć przeglądarkę na tym samym komputerze lub na innym komputerze lub urządzeniu). Wprowadź kod, a następnie zostanie wyświetlony monit o wprowadzenie nazwy użytkownika i hasła tożsamości, którego chcesz użyć. Po zakończeniu tego procesu powłoki poleceń kończy nazwy logowania. Może wyglądać mniej więcej tak:

    info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Added subscription Azure Free Trial
    info:    Setting subscription "Visual Studio Ultimate with MSDN" as default
    +
    info:    login command OK

> [!NOTE]
> Z logowania interakcyjnego uwierzytelnianie i autoryzacja są wykonywane przy użyciu usługi Azure Active Directory. Jeśli używasz tożsamość konta Microsoft, proces logowania uzyskuje dostęp do domeny domyślnej usługi Azure Active Directory. (Jeśli zostało zarejestrowane w bezpłatne konto platformy Azure, Azure Active Directory tworzone domyślną domenę dla konta.)
>
>

## <a name="scenario-2-azure-login-with-a-username-and-password"></a>Scenariusz 2: azure Zaloguj się za pomocą nazwy użytkownika i hasła
Użyj `azure login` polecenia nazwy użytkownika (`-u`) parametru do uwierzytelniania, gdy chcesz użyć służbowego lub konto służbowe, które nie wymaga uwierzytelniania wieloskładnikowego. Zostanie wyświetlony monit o hasło w wierszu polecenia (lub hasło można przekazać opcjonalnie jako parametr dodatkowe `azure login` polecenia). Poniższy przykład przekazuje nazwę użytkownika konta organizacyjnego:

    azure login -u myUserName@contoso.onmicrosoft.com

Następnie zostanie wyświetlony monit o wprowadzenie hasła:

    info:    Executing command login
    Password: *********

Następnie kończy proces logowania.

    info:    Added subscription Visual Studio Ultimate with MSDN
    +
    info:    login command OK

Jeśli jest to Twoje pierwsze rejestrowania w czasie przy użyciu tych poświadczeń, należy sprawdzić, które chcesz buforować tokenu uwierzytelniania. Ten monit występuje także jeśli poprzednio korzystano `azure logout` polecenia (opisane w dalszej części tego artykułu). Aby pominąć ten wiersz w scenariuszach automatyzacji, uruchom `azure login` z `-q` parametru.

## <a name="scenario-3-azure-login-with-a-service-principal"></a>Scenariusz 3: azure logowania przy użyciu nazwy głównej usługi
Jeśli tworzenie nazwy głównej usługi dla aplikacji usługi Active Directory, a nazwy głównej usługi ma uprawnienia do subskrypcji, możesz użyć `azure login` polecenie, aby uwierzytelnić nazwy głównej usługi. W zależności od scenariusza, można podać poświadczenia nazwy głównej usługi jako jawne parametry `azure login` polecenia. Na przykład następujące polecenie przekazuje główną nazwę usługi i identyfikator dzierżawy usługi Active Directory:

    azure login -u https://www.contoso.org/example --service-principal --tenant myTenantID

Następnie monit o podanie hasła. Można również podać poświadczenia, za pomocą interfejsu wiersza polecenia kodu skryptu lub aplikacji lub Użyj certyfikatu do uwierzytelnienia nazwy głównej usługi nieinteraktywnie w scenariuszach automatyzacji. Aby uzyskać szczegółowe informacje i przykłady, zobacz [uwierzytelniania nazwy głównej usługi z usługą Azure Resource Manager](resource-group-authenticate-service-principal-cli.md).

## <a name="scenario-4-use-a-publish-settings-file"></a>Scenariusz 4: Korzystanie z pliku ustawień publikowania
Jeśli musisz używać poleceń usługi Azure Service Management tryb interfejsu wiersza polecenia (na przykład w celu wdrożenia maszyn wirtualnych platformy Azure w klasycznym modelu wdrażania), możesz połączyć za pomocą pliku ustawień publikowania. Ta metoda instaluje certyfikat na komputerze lokalnym, który umożliwia wykonywanie zadań zarządzania dla tak długo, jak subskrypcja i certyfikat są prawidłowe.

* **Aby pobrać plik ustawień publikowania** dla Twojego konta, upewnij się, że interfejsu wiersza polecenia jest w trybie zarządzania usługami, wpisując `azure config mode asm`. Następnie uruchom następujące polecenie:

        azure account download

To uruchomi domyślną przeglądarkę i wyświetli monit o Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com). Po zalogowaniu, `.publishsettings` pliku pliki do pobrania. Zanotuj miejsce, w którym plik został zapisany.

> [!NOTE]
> Jeśli Twoje konto jest skojarzone z wieloma dzierżawcami usługi Azure Active Directory, może być monit o wybranie które chcesz pobrać plik ustawień publikowania dla usługi Active Directory.
>
>

Po wybraniu za pomocą strony pobierania lub poprzez odwiedzenie klasycznego portalu Azure, wybrane usługi Active Directory staje się domyślne używane przez stronę klasycznego portalu i pobierania. Po ustanowieniu domyślny, zostanie wyświetlony tekst "**kliknij tutaj, aby powrócić do strony wyboru**" w górnej części strony pobierania. Użyj podane łącze, aby powrócić do strony wyboru.

* **Aby zaimportować plik ustawień publikowania**, uruchom następujące polecenie:

        azure account import <path to your .publishsettings file>

> [!IMPORTANT]
> Po zaimportowaniu Twoje ustawienia publikowania, należy usunąć `.publishsettings` pliku. Nie jest już wymagana przez wiersza polecenia platformy Azure, a stanowi zagrożenie bezpieczeństwa, ponieważ może służyć do uzyskania dostępu do Twojej subskrypcji.
>
>

## <a name="cli-command-modes"></a>Tryby polecenia interfejsu wiersza polecenia
Azure CLI dostępne są dwa tryby polecenia do pracy z zasobami Azure z zestawami inne polecenie:

* **Tryb usługi Resource Manager** — w przypadku pracy z zasobami platformy Azure w modelu wdrażania usługi Resource Manager. Aby ustawić ten tryb, uruchom `azure config mode arm`.
* **Tryb zarządzania usługami** — w przypadku pracy z zasobami platformy Azure w klasycznym modelu wdrażania. Aby ustawić ten tryb, uruchom `azure config mode asm`.

Podczas pierwszej instalacji bieżącej wersji interfejsu wiersza polecenia jest w trybie Menedżera zasobów.

> [!NOTE]
> Tryb usługi Resource Manager i tryb usługi zarządzania wzajemnie się wykluczają. Oznacza to, że zasoby utworzone w trybie jednego nie można zarządzać z innych trybu.
>
>

## <a name="multiple-subscriptions"></a>Wiele subskrypcji
Jeśli masz wiele subskrypcji Azure, łączenie Azure udziela dostępu do wszystkich subskrypcji skojarzonych z poświadczeniami użytkownika. Jedna subskrypcja jest wybrana jako domyślna i używane przez interfejs wiersza polecenia Azure podczas wykonywania operacji. Można wyświetlić subskrypcje, łącznie z bieżącej subskrypcji domyślną, używając `azure account list` polecenia. To polecenie zwraca informacje podobne do następujących:

    info:    Executing command account list
    data:    Name              Id                                    Current
    data:    ----------------  ------------------------------------  -------
    data:    Azure-sub-1       ####################################  true
    data:    Azure-sub-2       ####################################  false

Na powyższej liście **bieżącego** bieżącej subskrypcji domyślne jako Azure-sub-1 wskazuje kolumny. Aby zmienić domyślne subskrypcji, użyj `azure account set` polecenia, a następnie określ subskrypcję, która ma być domyślnie. Na przykład:

    azure account set Azure-sub-2

Spowoduje to zmianę domyślnego subskrypcji na Azure-sub-2.

> [!NOTE]
> Zmiana domyślnego subskrypcji aktywne natychmiast i zmiana globalnych; nowe polecenia interfejsu wiersza polecenia Azure, czy uruchamiać z tego samego wystąpienia wiersza polecenia lub inne wystąpienie, użyj nowej subskrypcji domyślne.
>
>

Jeśli chcesz użyć innych niż domyślne subskrypcji z wiersza polecenia platformy Azure, ale nie chcesz zmienić bieżące ustawienie domyślne, możesz użyć `--subscription` opcji dla polecenia i podać nazwę subskrypcji chcesz użyć dla tej operacji.

Po nawiązaniu połączenia z subskrypcją platformy Azure, możesz rozpocząć używać polecenia interfejsu wiersza polecenia Azure do pracy z zasobami platformy Azure.

## <a name="storage-of-cli-settings"></a>Magazyn ustawień interfejsu wiersza polecenia
Czy możesz zalogować się przy użyciu `azure login` polecenia lub importowania ustawień publikowania, profilu interfejsu wiersza polecenia i dzienniki są przechowywane w `.azure` katalog znajduje się w sieci `user` katalogu. Twoje `user` katalogu jest chroniony przez system operacyjny. Jednak zaleca się wykonanie dodatkowych czynności w celu zaszyfrowania Twojej `user` katalogu. Można to zrobić na następujące sposoby:

* W systemie Windows należy zmodyfikować właściwości katalogu lub używać funkcji BitLocker.
* Dla komputerów Mac należy włączyć funkcję FileVault dla katalogu.
* W systemie Ubuntu skorzystaj z funkcji zaszyfrowanego katalogu głównego. Inne dystrybucje systemu Linux oferuje podobne funkcje.

## <a name="logging-out"></a>Wylogowywanie
Się wylogować, użyj następującego polecenia:

    azure logout -u <username>

Jeśli subskrypcji skojarzone z kontem, są uwierzytelniani tylko z usługą Active Directory, wylogowaniu usunięcia informacji o subskrypcji z profilu lokalnego. Jednak także zaimportowano plik ustawień publikowania dla subskrypcji, rejestrowanie tylko do usunięcia usługi Active Directory powiązane informacje z profilu lokalnego.

## <a name="next-steps"></a>Następne kroki
* Użycie poleceń interfejsu wiersza polecenia Azure, zobacz [polecenia wiersza polecenia platformy Azure w trybie Menedżera zasobów](virtual-machines/azure-cli-arm-commands.md) i [polecenia wiersza polecenia platformy Azure w trybie zarządzania usługami](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* Aby dowiedzieć się więcej o Azure CLI, pobierania kodu źródłowego, zgłaszanie problemów lub przyczyniają się do projektu, odwiedź stronę [repozytorium GitHub dla interfejsu wiersza polecenia Azure](https://github.com/azure/azure-xplat-cli).
* Jeśli wystąpią problemy przy użyciu wiersza polecenia platformy Azure lub usługi Azure, odwiedź stronę [fora Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).
