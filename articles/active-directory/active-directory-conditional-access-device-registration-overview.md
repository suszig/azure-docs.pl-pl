---
title: Omówienie usługi Rejestracja urządzeń w usłudze Azure Active Directory | Microsoft Docs
description: stanowi podstawę dla scenariuszy dostępu warunkowego opartego na urządzeniach. Po zarejestrowaniu urządzenia usługa Rejestracja urządzeń w usłudze Azure Active Directory aprowizuje dla urządzenia tożsamość, która jest używana do uwierzytelniania urządzenia podczas logowania użytkownika.
services: active-directory
keywords: rejestracja urządzenia, włączanie rejestracji urządzenia, rejestracja urządzenia i MDM
documentationcenter: ''
author: femila
manager: swadhwa
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/27/2016
ms.author: Markvi

---
# Wprowadzenie do usługi Rejestracja urządzeń w usłudze Azure Active Directory
Rejestracja urządzeń w usłudze Azure Active Directory jest podstawą dla scenariuszy dostępu warunkowego opartego na urządzeniach. Po zarejestrowaniu urządzenia usługa Rejestracja urządzeń w usłudze Azure Active Directory inicjuje obsługę urządzenia przy użyciu tożsamości, która jest używana do uwierzytelniania urządzenia podczas logowania użytkownika. Uwierzytelnionego urządzenia i atrybutów urządzenia można następnie użyć do wymuszania zasad dostępu warunkowego dla aplikacji hostowanych w chmurze i lokalnych.

W przypadku połączenia z rozwiązaniem do zarządzania urządzeniami przenośnymi (MDM, mobile device management), takim jak usługa Microsoft Intune, atrybuty urządzenia w usłudze Azure Active Directory są aktualizowane przy użyciu dodatkowych informacji o urządzeniu. Umożliwia to tworzenie reguł dostępu warunkowego, które wymuszają dostęp z urządzeń spełniających określone standardy zabezpieczeń i zgodności. Aby uzyskać więcej informacji na temat rejestrowania urządzeń w usłudze Microsoft Intune, zobacz artykuł [Enroll devices for management in Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune) (Rejestrowanie urządzeń w usłudze Intune w celu zarządzania nimi).

## Scenariusze obsługiwane przez usługę Rejestracja urządzeń w usłudze Azure Active Directory
Usługa Rejestracja urządzeń w usłudze Azure Active Directory obejmuje obsługę urządzeń z systemami iOS, Android i Windows. Poszczególne scenariusze wykorzystujące usługę Rejestracja urządzeń w usłudze Azure Active Directory mogą mieć bardziej specyficzne wymagania i obsługę platform. Te scenariusze są następujące:

* **Dostęp warunkowy do aplikacji, które są hostowane lokalnie**: można używać zarejestrowanych urządzeń z zasadami dostępu dla aplikacji, które są skonfigurowane do korzystania z usług AD FS w systemie Windows Server 2012 R2. Aby uzyskać więcej informacji na temat konfigurowania lokalnego dostępu warunkowego, zobacz [Setting up On-premises Conditional Access using Azure Active Directory Device Registration](active-directory-conditional-access-on-premises-setup.md) (Konfigurowanie lokalnego dostępu warunkowego przy użyciu usługi Rejestracja urządzeń w usłudze Azure Active Directory).
* **Dostęp warunkowy dla aplikacji usługi Office 365 w usłudze Microsoft Intune**: administratorzy IT mogą aprowizować zasady dostępu warunkowego urządzeń, aby zabezpieczyć zasoby firmowe, a jednocześnie zezwalać na dostęp do usług pracownikom przetwarzającym informacje na zgodnych urządzeniach. Aby uzyskać więcej informacji, zobacz artykuł [Conditional Access Device Policies for Office 365 services](active-directory-conditional-access-device-policies.md) (Zasady dostępu warunkowego urządzeń dla usług Office 365).

## Konfigurowanie usługi Rejestracja urządzeń w usłudze Azure Active Directory
Usługę Rejestracja urządzeń w usłudze Azure Active Directory należy włączyć w witrynie Azure Portal, aby urządzenia przenośne mogły odnajdywać usługę przez wyszukiwanie dobrze znanych rekordów systemu DNS. System DNS firmy należy skonfigurować tak, aby urządzenia z systemem Windows 10, Windows 8.1, Windows 7, Android i iOS mogły odnajdywać usługę i korzystać z niej.
Zarejestrowane urządzenia można wyświetlać i włączać/wyłączać za pomocą Portalu administratora w usłudze Azure Active Directory.

> [!NOTE]
> Aby uzyskać najnowsze instrukcje dotyczące konfigurowania automatycznej rejestracji urządzeń, zobacz artykuł [How to setup automatic registration of Windows domain joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md) (Konfigurowanie automatycznej rejestracji urządzeń przyłączonych do domeny systemu Windows w usłudze Azure Active Directory).
> 
> 

### Włączanie usługi Rejestracja urządzeń w usłudze Azure Active Directory
1. Zaloguj się w witrynie Microsoft Azure Portal jako administrator.
2. W lewym okienku wybierz pozycję **Active Directory**.
3. Na karcie **Katalog** wybierz swój katalog.
4. Wybierz kartę **Konfigurowanie**.
5. Przewiń do sekcji o nazwie **Urządzenia**.
6. Wybierz opcję **WSZYSTKIE** dla pozycji **UŻYTKOWNICY MOGĄ DOŁĄCZAĆ URZĄDZENIA W MIEJSCU PRACY**.
7. Wybierz maksymalną liczbę urządzeń do autoryzacji na użytkownika.

> [!NOTE]
> Rejestracja za pomocą usługi Microsoft Intune lub funkcji zarządzania urządzeniami przenośnymi dla usługi Office 365 wymaga dołączenia w miejscu pracy. Jeśli skonfigurowano którąś z tych usług, zaznaczona jest opcja WSZYSTKIE, a opcja BRAK jest wyłączona.
> 
> 

Domyślnie uwierzytelnianie dwuskładnikowe nie jest włączone dla usługi. Jednak uwierzytelnianie dwuskładnikowe jest zalecane podczas rejestrowania urządzenia.

* Przed wymaganiem uwierzytelniania dwuskładnikowego dla tej usługi należy skonfigurować dostawcę uwierzytelniania dwuskładnikowego w usłudze Azure Active Directory i skonfigurować konta użytkowników dla uwierzytelniania wieloskładnikowego — zobacz [Adding Multi-Factor Authentication to Azure Active Directory](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md) (Dodawanie usługi Multi-Factor Authentication w usłudze Azure Active Directory).
* Jeśli korzystasz z usług AD FS w systemie Windows Server 2012 R2, należy skonfigurować moduł uwierzytelniania dwuskładnikowego w usługach AD FS — zobacz [Using Multi-Factor Authentication with Active Directory Federation Services](../multi-factor-authentication/multi-factor-authentication-get-started-server.md) (Używanie usługi Multi-Factor Authentication w usługach Active Directory Federation Services).

## Konfigurowanie odnajdywania usługi Rejestracja urządzeń w usłudze Azure Active Directory
Urządzenia z systemami Windows 7 i Windows 8.1 będą odnajdywały usługę Rejestracja urządzeń, łącząc nazwę konta użytkownika z dobrze znaną nazwą serwera usługi rejestracji urządzeń.

Należy utworzyć rekord CNAME w systemie DNS wskazujący rekord A skojarzony z usługą Rejestracja urządzeń w usłudze Azure Active Directory. Rekord CNAME musi używać dobrze znanego prefiksu enterpriseregistration i następującego po nim sufiksu nazwy UPN używanego przez konta użytkowników w organizacji. Jeśli organizacja używa wielu sufiksów nazw UPN, należy utworzyć wiele rekordów CNAME w systemie DNS.

Na przykład jeśli w organizacji używane są dwa sufiksy nazw UPN, @contoso.com i @region.contoso.com, utworzysz następujące rekordy DNS.

| Wpis | Typ | Adres |
| --- | --- | --- |
| enterpriseregistration.contoso.com |CNAME |enterpriseregistration.windows.net |
| enterpriseregistration.region.contoso.com |CNAME |enterpriseregistration.windows.net |

## Wyświetlanie obiektów urządzeń i zarządzanie nimi w usłudze Azure Active Directory
1. Z poziomu Portalu administratora platformy Azure można wyświetlać, blokować i odblokowywać urządzenia. Zablokowane urządzenie nie będzie już miało dostępu do aplikacji, które są skonfigurowane w celu zezwalania na dostęp tylko zarejestrowanym urządzeniom.
2. Zaloguj się w witrynie Microsoft Azure Portal jako administrator.
3. W lewym okienku wybierz pozycję **Active Directory**.
4. Wybierz swój katalog.
5. Wybierz kartę **Użytkownicy**. Wybierz użytkownika, aby wyświetlić jego urządzenia.
6. Wybierz kartę **Urządzenia**.
7. Wybierz pozycję **Zarejestrowane urządzenia** z menu rozwijanego.
8. W tym miejscu można wyświetlić, zablokować lub odblokować zarejestrowane urządzenia użytkowników.

## Tematy dodatkowe
Za pomocą usługi Rejestracja urządzeń w usłudze Azure Active Directory można rejestrować urządzenia z systemem Windows 7 i Windows 8.1 przyłączone do domeny. Poniższe tematy zawierają więcej informacji na temat wymagań wstępnych oraz kroków wymaganych do skonfigurowania usługi rejestracji urządzeń na urządzeniach z systemami Windows 7 i Windows 8.1.

* [Automatic Device Registration with Azure Active Directory for Windows Domain-Joined Devices (Automatyczna rejestracja w usłudze Azure Active Directory urządzeń przyłączonych do domeny systemu Windows)](active-directory-conditional-access-automatic-device-registration.md)
* [Configure automatic device registration for Windows 7 domain joined devices (Konfigurowanie automatycznej rejestracji urządzeń z systemem Windows 7 przyłączonych do domeny)](active-directory-conditional-access-automatic-device-registration-windows7.md)
* [Configure automatic device registration for Windows 8.1 domain joined devices (Konfigurowanie automatycznej rejestracji urządzeń z systemem Windows 8.1 przyłączonych do domeny)](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
* [Automatic device registration with Azure Active Directory for Windows 10 domain-joined devices (Automatyczna rejestracja w usłudze Azure Active Directory urządzeń z systemem Windows 10 przyłączonych do domeny)](active-directory-azureadjoin-devices-group-policy.md)

<!--HONumber=Sep16_HO4-->


