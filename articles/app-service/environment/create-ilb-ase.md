---
title: "Tworzenie i używanie wewnętrznego modułu równoważenia obciążenia z środowiska usługi aplikacji Azure"
description: "Więcej informacji na temat tworzenia i używania środowiska usługi aplikacji Azure izolowane internet"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 0f4c1fa4-e344-46e7-8d24-a25e247ae138
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: cc7bdd7860506c20187dc913b72111824d1737ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-use-an-internal-load-balancer-with-an-app-service-environment"></a>Tworzenie i używanie wewnętrznego modułu równoważenia obciążenia z środowiska usługi aplikacji #

 Środowiska usługi aplikacji Azure to wdrożenie usługi Azure App Service w podsieci sieci wirtualnej platformy Azure (VNet). Istnieją dwa sposoby wdrożenia środowiska usługi App Service (ASE): 

- Z adresów VIP na zewnętrzny adres IP często nazywane ASE zewnętrznych.
- Z adresów VIP na wewnętrzny adres IP często nazywane ASE ILB, ponieważ wewnętrzny punkt końcowy jest wewnętrzny moduł równoważenia obciążenia (ILB). 

W tym artykule przedstawiono sposób tworzenia ASE ILB. Omówienie ASE, zobacz [wprowadzenie do środowiska usługi aplikacji][Intro]. Aby dowiedzieć się, jak utworzyć ASE zewnętrznych, zobacz [Tworzenie zewnętrznych ASE][MakeExternalASE].

## <a name="overview"></a>Omówienie ##

Można wdrożyć ASE z punktem końcowym dostęp do Internetu lub adres IP w sieci wirtualnej. Aby ustawić adres IP adresów sieci wirtualnej, należy wdrożyć ASE z ILB. Podczas wdrażania sieci ASE z ILB, należy podać:

-   Własnej domeny, który jest używany podczas tworzenia aplikacji.
-   Certyfikat używany do obsługi protokołu HTTPS.
-   Zarządzanie systemem DNS dla domeny.

W zamian można wykonać czynności takich jak:

-   Host aplikacji intranetowych bezpiecznie w chmurze, które dostępne za pośrednictwem sieci VPN platformy Azure ExpressRoute lub lokacja lokacja.
-   Aplikacje hosta w chmurze, które nie są wymienione w publicznych serwerów DNS.
-   Utwórz izolowaną internet zaplecza aplikacji, które frontonu aplikacji bezpiecznego można zintegrować z.

### <a name="disabled-functionality"></a>Funkcje wyłączone ###

Istnieje kilka kwestii, które nie można wykonać, gdy używasz ASE ILB:

-   Użyj opartych na protokole SSL.
-   Przypisz adresy IP do określonych aplikacji.
-   Kup i korzystać z certyfikatu z aplikacji za pośrednictwem portalu Azure. Można uzyskać certyfikatów bezpośrednio od urzędu certyfikacji i używać ich z aplikacjami. Nie można je uzyskać za pośrednictwem portalu Azure.

## <a name="create-an-ilb-ase"></a>Utwórz ILB ASE ##

Aby utworzyć ASE ILB:

1. W portalu Azure wybierz **nowy** > **sieci Web i mobilność** > **środowiska usługi aplikacji**.

2. Wybierz subskrypcję.

3. Wybierz lub utwórz grupę zasobów.

4. Wybierz lub Utwórz sieć wirtualną.

5. W przypadku wybrania istniejącej sieci wirtualnej, należy utworzyć podsieci do przechowywania ASE. Upewnij się, że ustawiony rozmiar podsieci, wystarczająco duże, aby pomieścić przyszłego rozwoju Twojej ASE. Firma Microsoft zaleca o rozmiarze `/25`, który 128 adresów i może obsłużyć ASE rozmiar maksymalny. Możesz wybrać rozmiar minimalny to `/28`. Po wymaga infrastruktury, ten rozmiar mogą być skalowane do maksymalnie 11 wystąpień.

    * Przekraczają maksymalną liczbą wystąpień 100 domyślną w planów usługi aplikacji.

    * Skalowanie bliskie 100, ale także z bardziej szybkie skalowanie frontonu.

6. Wybierz **wirtualnego lokalizacji** > **konfiguracji sieci wirtualnej**. Ustaw **typu VIP** do **wewnętrzny**.

7. Wprowadź nazwę domeny. Ta domena jest używany w przypadku aplikacji utworzone w tym ASE. Istnieją pewne ograniczenia. Nie można go:

    * NET   

    * azurewebsites.NET

    * p.azurewebsites.NET

    * &lt;asename&gt;. p.azurewebsites.net

   Niestandardowa nazwa domeny używane do aplikacji i nazwy domeny używane przez użytkownika ASE nie mogą się nakładać. Dla ASE ILB z nazwą domeny _contoso.com_, nie można użyć niestandardowych nazw domen dla twojej aplikacji, takich jak:

    * www.contoso.com

    * Abcd.def.contoso.com

    * Abcd.contoso.com

   Jeśli znasz nazwy domeny niestandardowej dla aplikacji, należy wybrać domenę dla ASE ILB, który nie występuje konflikt z tymi nazwami domen niestandardowych. W tym przykładzie, można użyć przypominać *contoso internal.com* dla domeny Twojej ASE ponieważ nie będących w konflikcie z nazwami domen niestandardowych, które kończą się *. contoso.com*.

8. Wybierz **OK**, a następnie wybierz **Utwórz**.

    ![Tworzenie środowiska ASE][1]

Na **sieci wirtualnej** bloku jest **konfiguracja sieci wirtualnej** opcji. Służy on do wybierania zewnętrznego adresu VIP lub wewnętrznego adresu VIP. Wartość domyślna to **zewnętrznych**. W przypadku wybrania **zewnętrznych**, Twoje ASE używa VIP dostępny z Internetu. W przypadku wybrania **wewnętrzne**, Twoje ASE skonfigurowano ILB na adres IP w sieci wirtualnej.

Po wybraniu **wewnętrzne**, możliwość dodawania więcej adresów IP do sieci ASE zostanie usunięta. Zamiast tego należy podać domenę ASE. W elemencie ASE z zewnętrznego wirtualnego adresu IP nazwę ASE służy w domenie aplikacji utworzone w tym ASE.

Jeśli ustawisz **typu VIP** do **wewnętrzne**, nazwę ASE nie jest używany w domenie dla ASE. Jawnie określ domenę. Jeśli Twoja domena to *contoso.corp.net* i utworzyć aplikację, w tym o nazwie ASE *timereporting*, adres URL dla tej aplikacji jest timereporting.contoso.corp.net.


## <a name="create-an-app-in-an-ilb-ase"></a>Utwórz aplikację w elemencie ASE ILB ##

Tworzenie aplikacji w elemencie ASE ILB odbywa się w taki sam sposób tworzenia aplikacji w elemencie ASE normalnie.

1. W portalu Azure wybierz **nowy** > **sieci Web i mobilność** > **Web** lub **Mobile** lub **interfejsu API Aplikacja**.

2. Wprowadź nazwę aplikacji.

3. Wybierz subskrypcję.

4. Wybierz lub utwórz grupę zasobów.

5. Wybierz lub Utwórz plan usługi aplikacji. Jeśli chcesz utworzyć nowy plan usługi aplikacji, wybierz użytkownika ASE jako lokalizację. Wybierz puli procesów roboczych, w którym ma swój plan usługi aplikacji ma zostać utworzony. Po utworzeniu planu usługi aplikacji, wybierz Twoje ASE jako lokalizację i puli procesów roboczych. Po określeniu nazwy aplikacji domeny pod nazwą aplikacji zostało zastąpione przez domenę dla Twojego ASE.

6. Wybierz pozycję **Utwórz**. Aplikacji są wyświetlane na pulpicie nawigacyjnym, zaznacz **Przypnij do pulpitu nawigacyjnego** pole wyboru.

    ![Tworzenie planu usługi aplikacji][2]

    W obszarze **Nazwa aplikacji**, nazwa domeny jest zaktualizowana w celu odzwierciedlenia domeny Twojej ASE.

## <a name="post-ilb-ase-creation-validation"></a>Sprawdzanie poprawności tworzenia POST ILB ASE ##

ASE ILB są nieco inne niż z systemem innym niż - ILB ASE. Jak już zanotowane trzeba zarządzać własną DNS. Należy również podać swój własny certyfikat dla połączeń HTTPS.

Po utworzeniu sieci ASE, nazwa domeny zawiera określonej domeny. Nowy element jest wyświetlany w **ustawienie** menu o nazwie **certyfikatu ILB**. ASE jest tworzony przy użyciu certyfikatu, który nie określa domeny ILB ASE. Użycie ASE z tego certyfikatu, przeglądarki informuje, że jest on nieprawidłowy. Ten certyfikat ułatwia testowanie HTTPS, ale należy przekazać swój własny certyfikat, który jest powiązany z własną domeną ILB ASE. Ten krok jest niezbędny, niezależnie od tego, czy certyfikat z podpisem własnym, czy otrzymanego od urzędu certyfikacji.

![Nazwa domeny ILB ASE][3]

Twoje ASE ILB musi mieć prawidłowy certyfikat SSL. Użyj certyfikatu wewnętrznego urzędów, kupić certyfikat od zewnętrznego wystawcy lub Użyj certyfikatu z podpisem własnym. Niezależnie od źródła certyfikatu SSL, należy poprawnie skonfigurowane następujące atrybuty certyfikatu:

* **Temat**: ten atrybut musi mieć ustawioną *.your głównego domeny tutaj.
* **Alternatywna nazwa podmiotu**: ten atrybut musi zawierać zarówno **.your głównego domeny tutaj* i **.scm.your-głównego-domeny — w tym miejscu*. Połączenia SSL do witryny SCM/Kudu skojarzone z każdej aplikacji używać adresu w postaci *your-app-name.scm.your-root-domain-here*.

Konwertuj/Zapisz certyfikat jako plik pfx. Plik PFX musi obejmować wszystkie pośrednie i certyfikaty główne. Zabezpiecz ją przy użyciu hasła.

Jeśli chcesz utworzyć certyfikat z podpisem własnym, w tym miejscu można użyć poleceń programu PowerShell. Należy użyć nazwy domeny ILB ASE zamiast *internal.contoso.com*: 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "\*.internal-contoso.com","\*.scm.internal-contoso.com"
    
    $certThumbprint = "cert:\localMachine\my\" +$certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText
    
    $fileName = "exportedcert.pfx" 
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password

Certyfikat, który generowanie tych poleceń programu PowerShell oflagowane przez przeglądarki, ponieważ certyfikat nie został utworzony przez urząd certyfikacji, który znajduje się w łańcuch zaufania w przeglądarce. Aby uzyskać certyfikat, któremu ufa przeglądarki, należy przygotować od urzędu certyfikacji komercyjnych w łańcuchu zaufania w przeglądarce. 

![Zestaw ILB certyfikatu][4]

Aby przekazać własne certyfikaty i testowanie dostępu:

1. Po utworzeniu ASE, przejdź do ASE interfejsu użytkownika. Wybierz **ASE** > **ustawienia** > **certyfikatu ILB**.

2. Aby ustawić certyfikat ILB, wybierz plik certyfikatu PFX, a następnie wprowadź hasło. Ten krok zajmuje trochę czasu. Pojawi się komunikat z informacją, że operacja przekazywania jest w toku.

3. Pobierz adres ILB dla Twojego ASE. Wybierz **ASE** > **właściwości** > **wirtualny adres IP**.

4. Tworzenie aplikacji sieci web w sieci ASE po utworzeniu ASE.

5. Tworzenie maszyny Wirtualnej, jeśli nie istnieje w tej sieci wirtualnej.

    > [!NOTE] 
    > Nie należy próbować utworzyć tej maszyny Wirtualnej w tej samej podsieci co ASE, ponieważ będzie zakończyć się niepowodzeniem lub spowodować problemy.
    >
    >

6. Ustaw DNS dla domeny ASE. Za pomocą symbolu wieloznacznego i domeny w systemie DNS. Proste testy w celu edytowania pliku hosts na maszynie Wirtualnej można ustawić nazwy aplikacji sieci web do adresu VIP IP:

    a. Jeśli Twoje ASE ma nazwę domeny _. ilbase.com_ i utworzysz aplikację sieci web o nazwie _mytestapp_, jest skierowana na _mytestapp.ilbase.com_. Następnie ustaw _mytestapp.ilbase.com_ do rozpoznawania adresów ILB. (W systemie Windows, w pliku hosts wynosi _C:\Windows\System32\drivers\etc\_.)

    b. Aby przetestować wdrożenie publikowania w sieci web lub dostęp do zaawansowanych konsoli, należy utworzyć rekord dla _mytestapp.scm.ilbase.com_.

7. Korzystanie z przeglądarki na tej maszynie Wirtualnej, przejdź do http://mytestapp.ilbase.com. (Lub przejdź do niezależnie od Twoja nazwa aplikacji sieci web jest z domeną.)

8. Korzystanie z przeglądarki na tej maszynie Wirtualnej, przejdź do https://mytestapp.ilbase.com. Jeśli używasz certyfikatu z podpisem własnym, należy zaakceptować braku zabezpieczeń.

    Adres IP dla sieci ILB znajduje się w obszarze **adresów IP**. Ta lista zawiera również adresy IP używane przez zewnętrznych adresów VIP oraz dla ruchu przychodzącego zarządzania.

    ![Adres ILB IP][5]

## <a name="web-jobs-functions-and-the-ilb-ase"></a>Zadania Web Job, funkcje i ILB ASE ##

Zarówno funkcje i zadania sieci web są obsługiwane w elemencie ASE ILB, ale portalu pracować z nimi, musi mieć dostęp do sieci do lokacji SCM.  Oznacza to, że przeglądarka musi być na hoście, który jest albo podłączone do sieci wirtualnej.  

Gdy używasz usługi Azure Functions w elemencie ASE ILB, może pobrać komunikat o błędzie stwierdzający "nie możemy teraz pobrać funkcji. Spróbuj ponownie później." Ten błąd występuje, ponieważ interfejs użytkownika funkcji wykorzystuje lokacji SCM za pośrednictwem protokołu HTTPS i certyfikatu głównego nie jest w przeglądarce łańcuch zaufania. Zadania Web Job ma podobny problem. Aby uniknąć tego problemu, wykonaj jedną z następujących czynności:

- Dodaj certyfikat do magazynu zaufanych certyfikatów. Odblokowuje to Edge i przeglądarki Internet Explorer.
- Użyj przeglądarki Chrome i przejdź do witryny SCM najpierw, Zaakceptuj niezaufany certyfikat, a następnie przejdź do portalu.
- Użyj komercyjnych certyfikat w łańcuchu zaufania przeglądarki.  Jest to najlepsze rozwiązanie.  

## <a name="dns-configuration"></a>Konfiguracja DNS ##

Gdy używasz zewnętrznego adresu VIP DNS jest zarządzana przez platformę Azure. Wszystkich aplikacji utworzonych w Twojej ASE jest automatycznie dodawany do usługi Azure DNS, który jest publicznym systemie DNS. W elemencie ASE ILB możesz zarządzać własną DNS. Dla danej domeny, takie jak _contoso.net_, należy utworzyć rekordy A systemu DNS w systemie DNS, wskazujące na adres ILB dla:

- *. contoso.net
- *. scm.contoso.net

Jeśli domenę ILB ASE jest używany dla wielu elementów poza tym ASE, może być konieczne zarządzanie DNS na podstawie ciągu app-name. Ta metoda może być trudne, ponieważ konieczne jest dodanie nowej nazwy aplikacji w systemie DNS, podczas jego tworzenia. Z tego powodu zaleca się użycie dedykowanych domeny.

## <a name="publish-with-an-ilb-ase"></a>Publikowanie za pomocą ILB ASE ##

Każda aplikacja, która zostanie utworzona są dwa punkty końcowe. W elemencie ASE ILB masz  *&lt;Nazwa aplikacji >.&lt; Domeny ASE ILB >* i  *&lt;Nazwa aplikacji > .scm.&lt; Domeny ASE ILB >*. 

Nazwa witryny SCM umożliwia przejście do konsoli Kudu, nazywanym **portal zaawansowane**, w portalu Azure. Konsola Kudu umożliwia wyświetlanie zmiennych środowiskowych, eksplorowania dysku, użyj konsoli i o wiele więcej. Aby uzyskać więcej informacji, zobacz [konsoli Kudu dla usługi Azure App Service][Kudu]. 

W wielodostępnej usługi aplikacji i zewnętrznego ASE Brak rejestracji jednokrotnej między konsoli Kudu i portalu Azure. Dla ASE ILB jednak należy do korzystania z poświadczeń publikowania do logowania się do konsoli Kudu.

Internetowych systemów elementu konfiguracji, takich jak GitHub i Visual Studio Team Services nie działają z ASE ILB, ponieważ punkt końcowy publikowania nie jest dostępny internet. Zamiast tego należy użyć systemu elementu konfiguracji, który wykorzystuje model ściągania, takich jak Dropbox.

Publikowanie punktów końcowych dla aplikacji w elemencie ASE ILB Użyj domeny, która została utworzona ILB ASE. Ta domena jest wyświetlany w profilu publikowania aplikacji i w bloku portalu aplikacji (**omówienie** > **Essentials** , a także **właściwości**). Jeśli masz ASE ILB z poddomeny *contoso.net* i aplikację o nazwie *test*, użyj *mytest.contoso.net* dla FTP i *mytest.scm.contoso.net* wdrożenia sieci web.

## <a name="couple-an-ilb-ase-with-a-waf-device"></a>Połączenie ASE ILB z urządzenia zapory aplikacji sieci Web ##

Usługa aplikacji Azure udostępnia wiele środki bezpieczeństwa, które zapewniają ochronę systemu. Ułatwiają również określić, czy zostało zaatakowane aplikacji. Ochrona najlepsze dla aplikacji sieci web jest połączenie Platforma macierzysta, takich jak usługi Azure App Service za pomocą zapory aplikacji sieci web (WAF). Ponieważ ILB ASE ma punkt końcowy aplikacji izolowane sieci, jest odpowiednie do takiego użycia.

Aby dowiedzieć się więcej na temat sposobu konfigurowania sieci ASE ILB z urządzenia zapory aplikacji sieci Web, zobacz [Konfigurowanie zapory aplikacji sieci web ze środowiskiem usługi aplikacji][ASEWAF]. W tym artykule przedstawiono sposób Barracuda urządzenia wirtualnego za pomocą programu ASE. Innym rozwiązaniem jest używać bramy aplikacji Azure. Brama aplikacji w używa reguł core OWASP do zabezpieczania aplikacji umieszczony za nią. Aby uzyskać więcej informacji na temat bramy aplikacji, zobacz [wprowadzenie do zapory aplikacji sieci web platformy Azure][AppGW].

## <a name="get-started"></a>Rozpoczynanie pracy ##

* Aby rozpocząć pracę z ASEs, zobacz [wprowadzenie do środowiska usługi aplikacji][Intro].
 
<!--Image references-->
[1]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-network.png
[2]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-webapp.png
[3]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate.png
[4]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate2.png
[5]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-ipaddresses.png

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[webapps]: ../app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
