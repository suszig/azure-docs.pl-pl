<properties
    pageTitle="Usługi domenowe Azure AD: włączanie synchronizacji haseł | Microsoft Azure"
    description="Wprowadzenie do Usług domenowych Azure Active Directory"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/25/2016"
    ms.author="maheshu"/>

# Usługi domenowe Azure AD *(wersja zapoznawcza)* — włączanie synchronizacji haseł w Usługach domenowych Azure AD

## Zadanie 5. Włączanie synchronizacji haseł w Usługach domenowych AAD dla zsynchronizowanej dzierżawy Azure AD
Następnym zadaniem po włączeniu Usług domenowych Azure AD w katalogu usługi Azure AD jest włączenie synchronizacji haseł w Usługach domenowych Azure AD. Jeśli jest ona włączona, użytkownicy mogą logować się do domeny za pomocą poświadczeń firmowych.

Kroki do wykonania różnią się w zależności od tego, czy organizacja ma katalog usługi Azure AD tylko w chmurze, czy też wykonuje synchronizację z katalogiem lokalnym za pomocą programu Azure AD Connect.

<br>

> [AZURE.SELECTOR]
- [Katalog usługi Azure AD tylko w chmurze](active-directory-ds-getting-started-password-sync.md)
- [Zsynchronizowany katalog usługi Azure AD](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>

### Zsynchronizowane dzierżawy — włączanie synchronizacji skrótów poświadczeń NTLM i Kerberos w usłudze Azure AD
Jeśli dzierżawa usługi Azure AD w organizacji została ustawiona do synchronizacji z katalogiem lokalnym za pomocą programu Azure AD Connect, należy skonfigurować program Azure AD Connect do synchronizowania skrótów poświadczeń wymaganych do uwierzytelniania NTLM i Kerberos. Te skróty nie są domyślnie synchronizowane z usługą Azure AD. Wykonanie poniższych kroków umożliwi włączenie synchronizacji skrótów w dzierżawie usługi Azure AD.

#### Instalowanie lub aktualizowanie programu Azure AD Connect

Na komputerze przyłączonym do domeny trzeba zainstalować najnowszą zalecaną wersję programu Azure AD Connect. Jeśli masz istniejące wystąpienie instalatora programu Azure AD Connect, musisz zaktualizować je tak, aby można było korzystać z ogólnie dostępnej kompilacji programu Azure AD Connect. Upewnij się, że używasz najnowszej wersji programu Azure AD Connect, aby uniknąć znanych problemów i błędów, które mogły już zostać naprawione.

**[Pobieranie programu Azure AD Connect](http://www.microsoft.com/download/details.aspx?id=47594)**

Zalecana wersja: **1.1.130.0** — opublikowana 12 kwietnia 2016 r.

  > [AZURE.WARNING] Aby umożliwić synchronizowanie poświadczeń haseł w starszej wersji (wymaganych podczas uwierzytelniania NTLM i Kerberos) z dzierżawą usługi Azure AD, MUSISZ zainstalować najnowszą zalecaną wersję programu Azure AD Connect. Ta funkcja nie jest dostępna w poprzednich wersjach programu Azure AD Connect ani starszej wersji narzędzia DirSync.

Instrukcje dotyczące instalowania programu Azure AD Connect są dostępne w artykule [Wprowadzenie do programu Azure AD Connect](../active-directory/active-directory-aadconnect.md)


#### Wymuszanie pełnej synchronizacji haseł do usługi Azure AD

Aby wymusić pełną synchronizację haseł i umożliwić synchronizowanie skrótów haseł wszystkich użytkowników lokalnych (w tym skrótów poświadczeń wymaganych do uwierzytelniania NTLM/Kerberos) z dzierżawą usługi Azure AD, uruchom następujący skrypt programu PowerShell w każdym lesie usługi AD.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

Czas synchronizacji poświadczeń z usługą Azure AD zależy od rozmiaru katalogu (liczby użytkowników, grup itp.). Z haseł będzie można korzystać w domenie zarządzanej Usług domenowych Azure AD wkrótce po zsynchronizowaniu skrótów poświadczeń z usługą Azure AD.


<br>

## Powiązana zawartość

- [Włączanie synchronizacji haseł w Usługach domenowych AAD dla katalogu usługi Azure AD tylko w chmurze](active-directory-ds-getting-started-password-sync.md)

- [Administrowanie domeną zarządzaną Usług domenowych Azure AD](active-directory-ds-admin-guide-administer-domain.md)

- [Dołączanie maszyny wirtualnej z systemem Windows do domeny zarządzanej Usług domenowych Azure AD](active-directory-ds-admin-guide-join-windows-vm.md)

- [Dołączanie maszyny wirtualnej z systemem Red Hat Enterprise Linux do domeny zarządzanej Usług domenowych Azure AD](active-directory-ds-admin-guide-join-rhel-linux-vm.md)



<!--HONumber=Jun16_HO2-->


