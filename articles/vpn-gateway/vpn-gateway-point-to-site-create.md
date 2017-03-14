---
title: "Konfigurowanie połączenia bramy sieci VPN typu punkt-lokacja z siecią wirtualną platformy Azure przy użyciu klasycznej witryny Portal | Microsoft Docs"
description: "Istnieje możliwość bezpiecznego połączenia się z siecią wirtualną platformy Azure poprzez utworzenie połączenia bramy sieci VPN typu punkt-lokacja."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 4f5668a5-9b3d-4d60-88bb-5d16524068e0
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/02/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: cea53acc33347b9e6178645f225770936788f807
ms.openlocfilehash: 3ff2dcba568ed7ff83154cb6e1f1861ffb32a0d2
ms.lasthandoff: 03/03/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-classic-portal"></a>Konfigurowanie połączenia sieci VPN typu punkt-lokacja z siecią wirtualną przy użyciu portalu klasycznego
> [!div class="op_single_selector"]
> * [Resource Manager — witryna Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager — program PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klasyczny — witryna Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> * [Model klasyczny — klasyczny portal](vpn-gateway-point-to-site-create.md)
> 
> 

Konfiguracja typu punkt-lokacja pozwala utworzyć indywidualne bezpieczne połączenie z siecią wirtualną z poziomu komputera klienckiego. Przedstawione rozwiązanie przydaje się w przypadku, gdy celem użytkownika jest połączenie się z siecią wirtualną z lokalizacji zdalnej, np. z domu lub z konferencji, lub gdy konieczne jest połączenie z siecią wirtualną jedynie niewielkiej liczby klientów.

Połączenia typu punkt-lokacja nie wymagają do prawidłowego działania urządzenia sieci VPN ani publicznego adresu IP. Połączenie sieci VPN jest nawiązywane poprzez zainicjowanie połączenia z komputera klienckiego. Więcej informacji na temat połączeń typu punkt-lokacja znajduje się w sekcji [Często zadawane pytania dotyczące połączeń typu punkt-lokacja](#faq) na końcu tego artykułu.

W tym artykule opisano proces tworzenia sieci wirtualnej z połączeniem typu punkt-lokacja w ramach klasycznego modelu wdrażania przy użyciu portalu klasycznego.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modele wdrażania i metody nawiązywania połączeń typu punkt-lokacja
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

W poniższej tabeli przedstawiono dwa modele wdrażania oraz dostępne metody wdrażania dla konfiguracji typu punkt-lokacja. Jeśli dostępny jest artykuł zawierający kroki konfiguracji, można do niego przejść bezpośrednio z tabeli.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Podstawowy przepływ pracy
![Diagram: połączenie typu punkt-lokacja](./media/vpn-gateway-point-to-site-create/p2sclassic.png "punkt-lokacja")

Poniższe kroki opisują czynności niezbędne w celu utworzenia bezpiecznego połączenia typu punkt-lokacja z siecią wirtualną. 

Proces konfigurowania połączenia typu punkt-lokacja został podzielony na&4; sekcje. Ważne jest zachowanie kolejności konfigurowania poszczególnych sekcji. Nie pomijać żadnych kroków ani przeskakiwać do przodu.

* **Sekcja 1** — tworzenie sieci wirtualnej i bramy sieci VPN.
* **Sekcja 2** — tworzenie certyfikatów używanych do uwierzytelniania oraz ich przekazywanie.
* **Sekcja 3** — eksportowanie i instalowanie certyfikatów klienta.
* **Sekcja 4** — konfigurowanie klienta VPN.

## <a name="vnetvpn"></a>Sekcja 1 — tworzenie sieci wirtualnej i bramy sieci VPN
### <a name="part-1-create-a-virtual-network"></a>Część 1. Tworzenie sieci wirtualnej
1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com). Zwróć uwagę, że te kroki odnoszą się do portalu klasycznego, a nie do witryny Azure Portal. Obecnie nie można utworzyć połączenia punkt-lokacja za pomocą witryny Azure Portal.
2. W lewym dolnym rogu ekranu kliknij opcję **Nowe**. W okienku nawigacji kliknij opcję **Usługi sieciowe**, a następnie opcję **Sieć wirtualna**. Kliknij przycisk **Tworzenie niestandardowe**, aby rozpocząć korzystanie z kreatora konfiguracji.
3. Na stronie **Szczegóły sieci wirtualnej** wprowadź następujące informacje, a następnie kliknij strzałkę w prawym dolnym rogu, aby przejść dalej.
   
   * **Nazwa**: nadaj nazwę sieci wirtualnej. Na przykład: „VNet1”. Tej nazwy będziesz używać, odwołując się do sieci podczas wdrażania maszyn wirtualnych w ramach danej sieci wirtualnej.
   * **Lokalizacja**: omawiana lokalizacja jest bezpośrednio związana z lokalizacją fizyczną (regionem), w której mają się znajdować zasoby (maszyny wirtualne). Jeśli na przykład maszyny wirtualne wdrażane w ramach tej sieci wirtualnej mają znajdować się we wschodnim regionie USA, należy wybrać tę właśnie lokalizację. Po utworzeniu sieci wirtualnej nie można zmienić powiązanego z nią regionu.
4. Na stronie **DNS Servers and VPN Connectivity** (Serwery DNS i połączenia sieci VPN) wprowadź następujące informacje, a następnie kliknij strzałkę w prawym dolnym rogu, aby przejść dalej.
   
   * **Serwery DNS**: wprowadź nazwę serwera DNS i jego adres IP lub wybierz zarejestrowany wcześniej serwer DNS z menu skrótów. To ustawienie nie powoduje jednak utworzenia serwera DNS. Umożliwia natomiast określenie serwerów DNS, które mają być używane do rozpoznawania nazw dla tej sieci wirtualnej. Chcąc skorzystać z domyślnej usługi rozpoznawania nazw platformy Azure, należy pozostawić to pole puste.
   * **Konfiguracja połączenia sieci VPN typu punkt-lokacja**: zaznacz to pole wyboru.
5. Na stronie **Łączność typu punkt-lokacja** określ zakres adresów IP, z których klienci sieci VPN będą otrzymywać adresy IP po nawiązaniu połączenia. Istnieje kilka zasad dotyczących zakresów adresów, które można określić. Należy koniecznie sprawdzić, czy określony zakres nie pokrywa się z żadnym z zakresów należących do sieci lokalnej.
6. Wprowadź następujące informacje, a następnie kliknij strzałkę, aby przejść dalej.
   
   * **Przestrzeń adresowa**: należy uwzględnić początkowy adres IP oraz wartość CIDR (liczbę adresów).
   * **Dodaj przestrzeń adresową**: przestrzeń adresową należy dodawać tylko wtedy, gdy jest to wymagane z racji struktury sieci.
7. Na stronie **Przestrzenie adresowe sieci wirtualnej** określ zakres adresów, który ma być używany w ramach sieci wirtualnej. Są to dynamiczne adresy IP (adresy DIP), które zostaną przypisane maszynom wirtualnym i innym wystąpieniom roli wdrażanym w ramach tej sieci wirtualnej.<br><br>Szczególnie istotne jest, aby wybrać zakres, który nie nakłada się na żaden z zakresów używanych w sieci lokalnej. Należy skontaktować się z administratorem sieci — niezbędne może okazać się wydzielenie przez niego z przestrzeni adresowej sieci lokalnej zakresu adresów IP, które zostaną użyte w ramach sieci wirtualnej.
8. Wprowadź następujące informacje, a następnie kliknij znacznik wyboru, aby rozpocząć tworzenie sieci wirtualnej.
   
   * **Przestrzeń adresowa**: dodaj wewnętrzny zakres adresów IP, którego chcesz używać w ramach tej sieci wirtualnej, uwzględniając początkowy adres IP i liczbę adresów. Istotne jest, aby wybrać zakres, który nie nakłada się na żaden z zakresów używanych w sieci lokalnej. 
   * **Dodaj podsieć**: dodatkowe podsieci nie są wymagane, można jednak utworzyć osobne podsieci dla maszyn wirtualnych, które będą miały statyczne adresy DIP. Maszyny wirtualne mogą także tworzyć podsieć niezależną od innych wystąpień roli.
   * **Dodaj podsieć bramy**: podsieć bramy jest wymagana w przypadku połączeń sieci VPN typu punkt-lokacja. Kliknij, aby dodać podsieć bramy. Podsieć bramy jest używana tylko w odniesieniu do bramy sieci wirtualnej.
9. Po utworzeniu sieci wirtualnej w obszarze **Stan** na stronie sieci w klasycznej witrynie Azure Portal pojawi się wartość **Utworzona**. Po utworzeniu sieci wirtualnej można utworzyć bramę o dynamicznym routingu.

### <a name="part-2-create-a-dynamic-routing-gateway"></a>Część 2: Tworzenie bramy o dynamicznym routingu
Podczas konfiguracji należy wybrać dynamiczny typ bramy. Bramy o statycznym routingu nie będą działać z tą funkcją.

1. Na stronie **Sieci** w klasycznej witrynie Azure Portal kliknij sieć wirtualną, która została utworzona, a następnie przejdź na stronę **Pulpit nawigacyjny**.
2. W dolnej części strony **Pulpit nawigacyjny** kliknij przycisk **Utwórz bramę**. Zostanie wyświetlony komunikat z pytaniem: **Czy chcesz utworzyć bramę sieci wirtualnej „VNet1”?**. Kliknij opcję **Tak**, aby rozpocząć tworzenie bramy. Proces tworzenia bramy może potrwać do 45 minut.

## <a name="generate"></a>Sekcja 2 — generowanie i przekazywanie certyfikatów
Certyfikaty są używane do uwierzytelniania klientów sieci VPN w obrębie sieci VPN typu punkt-lokacja. Można stosować certyfikat z podpisem własnym lub certyfikat główny wygenerowany przez rozwiązanie do obsługi certyfikatów przedsiębiorstwa. Na platformę Azure można przekazać do 20 certyfikatów głównych. Po przekazaniu pliku cer platforma Azure może użyć zawartych w nim informacji do uwierzytelniania klientów, którzy mają zainstalowany certyfikat klienta. Certyfikat klienta musi zostać wygenerowany na podstawie tego samego certyfikatu, który jest reprezentowany przez plik cer.

W tej sekcji wykonasz poniższe czynności:

* Uzyskaj plik cer dla certyfikatu głównego. Może to być certyfikat z podpisem własnym lub można użyć systemu certyfikatów przedsiębiorstwa.
* Przekaż plik cer do platformy Azure.
* Wygeneruj certyfikaty klienta.

### <a name="root"></a>Część 1: Uzyskiwanie pliku cer dla certyfikatu głównego
Jeśli korzystasz z rozwiązania dla przedsiębiorstwa, możesz użyć istniejącego łańcucha certyfikatów. Uzyskaj plik cer dla certyfikatu głównego, którego chcesz użyć.

Jeśli nie używasz rozwiązania z certyfikatem przedsiębiorstwa, musisz utworzyć certyfikat główny z podpisem własnym. Aby utworzyć certyfikat z podpisem własnym, który zawiera pola niezbędne do uwierzytelniania punkt-lokacja, użyj narzędzia makecert. Temat [Tworzenie certyfikatu głównego z podpisem własnym dla połączeń punkt-lokacja](vpn-gateway-certificates-point-to-site.md) zawiera opis kroków tworzenia certyfikatu głównego z podpisem własnym. Zdajemy sobie sprawę, że narzędzie makecert jest przestarzałe, ale tym razem jest to wspierane rozwiązanie.

>[!NOTE]
>Chociaż tworzenie certyfikatów z podpisem własnym przy użyciu programu PowerShell jest możliwe, certyfikaty wygenerowane w ten sposób nie zawierają pól niezbędnych do uwierzytelniania punkt-lokacja.
>


#### <a name="to-obtain-the-cer-file-from-a-self-signed-root-certificate"></a>Aby uzyskać plik cer z certyfikatu głównego z podpisem własnym

1. Aby uzyskać plik cer z certyfikatu głównego z podpisem własnym, otwórz plik **certmgr.msc** i zlokalizuj utworzony przez Ciebie certyfikat główny. Certyfikat znajduje się zwykle w lokalizacji „Certificates-Current User/ Personal/Certificates” i ma nazwę nadaną mu podczas tworzenia. Kliknij prawym przyciskiem myszy certyfikat główny z podpisem własnym, kliknij pozycję **wszystkie zadania**, a następnie kliknij pozycję **eksport**. Spowoduje to otwarcie **Kreatora eksportu certyfikatów**.
2. W kreatorze kliknij pozycję **Dalej**, wybierz opcję **Nie eksportuj klucza prywatnego**, a następnie kliknij przycisk **Dalej**.
3. Na stronie **Format pliku eksportu** wybierz pozycję **Certyfikat X.509 szyfrowany algorytmem Base-64 (.CER)**. Następnie kliknij przycisk **Dalej**.
4. W obszarze **Eksport pliku** wybierz pozycję **Przeglądaj**, aby przejść do lokalizacji, do której chcesz wyeksportować certyfikat. Do pola **Nazwa pliku** wprowadź nazwę pliku certyfikatu. Następnie kliknij przycisk **Next** (Dalej).
5. Kliknij przycisk **Zakończ**, aby wyeksportować certyfikat.

### <a name="upload"></a>Część 2: Przekazywanie pliku cer certyfikatu głównego do klasycznej witryny Azure Portal
Dodaj zaufany certyfikat do platformy Azure. Po dodaniu do platformy Azure pliku x.509 szyfrowanego algorytmem Base64 (.cer) otrzymuje ona informację, że może zaufać certyfikatowi głównemu reprezentowanemu przez ten plik.

1. W klasycznej witrynie Azure Portal na stronie **Certyfikaty** odnoszącej się do danej sieci wirtualnej kliknij pozycję **Przekaż certyfikat główny**.
2. Na stronie **Przekaż certyfikat** wybierz lokalizację pliku .cer certyfikatu głównego, a następnie kliknij znacznik wyboru.

### <a name="createclientcert"></a>Część 3: Generowanie certyfikatu klienta
Kolejny krok to generowanie certyfikatów klienta. Można wygenerować unikatowy certyfikat dla każdego klienta, dzięki któremu będzie można się łączyć, ale można też użyć tego samego certyfikatu na wielu klientach. Zaletą generowania unikatowych certyfikatów klienta jest możliwość odwoływania pojedynczego certyfikatu, jeśli to konieczne. W przeciwnym razie, jeśli wszyscy użytkownicy korzystają z tego samego certyfikatu klienta i konieczne jest odwołanie certyfikatu dla jednego klienta, należy wygenerować i zainstalować nowe certyfikaty dla wszystkich klientów, którzy używają certyfikatu do uwierzytelniania.

####<a name="enterprise-certificate"></a>Certyfikat przedsiębiorstwa
- Jeśli używasz rozwiązania z certyfikatem przedsiębiorstwa, wygeneruj certyfikat klienta przy użyciu formatu wartości nazwy pospolitej „name@yourdomain.com”, a nie formatu „nazwa_domeny\nazwa_użytkownika”.
- Upewnij się, że wydawany certyfikat klienta jest oparty na szablonie certyfikatu „Użytkownik”, którego pierwszym elementem na liście użycia jest „Uwierzytelnienie klienta”, a nie Logowanie karty inteligentnej itp. Certyfikat można sprawdzić przez dwukrotne kliknięcie certyfikatu klienta i wyświetlenie pozycji **Szczegóły > Ulepszone użycie klucza**.

####<a name="self-signed-certificate"></a>Certyfikat z podpisem własnym 
Jeśli używasz certyfikatu z podpisem własnym i chcesz wygenerować certyfikat klienta, zobacz temat [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Praca z certyfikatami głównymi z podpisem własnym dla konfiguracji połączenia typu punkt-lokacja).

## <a name="installclientcert"></a>Sekcja 3 — eksportowanie i instalowanie certyfikatu klienta
Certyfikat klienta należy zainstalować na każdym komputerze, który zostanie połączony z siecią wirtualną. Certyfikat klienta jest wymagany do uwierzytelniania. Można zautomatyzować instalowanie certyfikatu klienta, ale możliwe jest też przeprowadzenie instalacji ręcznie. Kroki przedstawione poniżej opisują sposób postępowania w przypadku eksportu i ręcznej instalacji certyfikatu klienta.

1. Aby wyeksportować certyfikat klienta, można użyć narzędzia *certmgr.msc*. Kliknij prawym przyciskiem myszy certyfikat klienta, który chcesz wyeksportować, a następnie kliknij przycisk **wszystkie zadania** i opcję **eksportuj**.
2. Wyeksportuj certyfikat klienta z kluczem prywatnym. Klucz znajduje się w pliku *pfx*. Zapisz lub zapamiętaj hasło (klucz) ustawione dla tego certyfikatu.
3. Skopiuj plik *.pfx* na komputer kliencki. Na komputerze klienckim kliknij dwukrotnie plik *pfx*, aby go zainstalować. Gdy zostanie wyświetlony stosowny monit, wprowadź hasło. Nie zmieniaj lokalizacji instalacji.

## <a name="vpnclientconfig"></a>Sekcja 4 — konfigurowanie klienta VPN
Aby nawiązać połączenie z siecią wirtualną, należy skonfigurować także klienta sieci VPN. W celu nawiązania połączenia klient wymaga zarówno certyfikatu klienta, jak i odpowiedniej konfiguracji klienta sieci VPN. Aby skonfigurować klienta sieci VPN, wykonaj po kolei następujące czynności.

### <a name="part-1-create-the-vpn-client-configuration-package"></a>Część 1: Tworzenie pakietu konfiguracyjnego klienta VPN
1. W klasycznej witrynie Azure Portal na stronie **Pulpit nawigacyjny** sieci wirtualnej przejdź do menu szybkiego dostępu w prawym górnym rogu. Pakiet klienta VPN zawiera informacje konfiguracyjne umożliwiające skonfigurowanie oprogramowania klienta VPN wbudowanego w system Windows. Instalacja pakietu nie powoduje instalacji dodatkowego oprogramowania. Ustawienia są specyficzne dla sieci wirtualnej, z którą chcesz nawiązać połączenie. Lista obsługiwanych systemów operacyjnych klienta znajduje się w sekcji [Często zadawane pytania dotyczące połączeń typu punkt-lokacja](#faq) na końcu tego artykułu.<br><br>Wybierz pakiet pobierania, który odpowiada systemowi operacyjnemu klienta, na którym zostanie zainstalowany:
   
   * W przypadku klientów 32-bitowych należy wybrać opcję **Pobierz pakiet dla 32-bitowego klienta VPN**.
   * W przypadku klientów 64-bitowych należy wybrać opcję **Pobierz pakiet dla 64-bitowego klienta VPN**.
2. Utworzenie pakietu klienta może potrwać kilka minut. Po zakończeniu procesu tworzenia pakietu można pobrać plik. Pobrany plik *.exe* można bezpiecznie przechowywać na komputerze lokalnym.
3. Po wygenerowaniu pakietu klienta VPN i pobraniu go z klasycznej witryny Azure Portal można go zainstalować na komputerze klienckim, z którego ma zostać nawiązane połączenie z siecią wirtualną. Chcąc zainstalować pakiet klienta VPN na wielu komputerach klienckich, należy się upewnić, że na każdym z komputerów jest także zainstalowany certyfikat klienta.

### <a name="part-2-install-the-vpn-configuration-package-on-the-client"></a>Część 2: Instalacja pakietu konfiguracyjnego sieci VPN na komputerze klienckim
1. Skopiuj lokalnie plik konfiguracji na komputer, który chcesz połączyć z siecią wirtualną, a następnie kliknij dwukrotnie plik z rozszerzeniem exe. 
2. Gdy pakiet zostanie zainstalowany, możesz uruchomić połączenie sieci VPN. Pakiet konfiguracyjny nie jest podpisany przez firmę Microsoft. Możesz podpisać pakiet, korzystając z urządzenia używanego w tym celu w Twojej organizacji, lub samodzielnie, przy użyciu narzędzia [SignTool](http://go.microsoft.com/fwlink/p/?LinkId=699327). Z pakietu można korzystać nawet, jeśli nie jest podpisany. Jeśli jednak pakiet nie jest podpisany, podczas instalowania go zostanie wyświetlone ostrzeżenie.
3. Na komputerze klienckim przejdź do obszaru **Ustawienia sieci** i kliknij pozycję **Sieć VPN**. Połączenie zostanie wyświetlone. Widoczna będzie nazwa sieci wirtualnej, z którą zostanie nawiązane połączenie. Powinno to wyglądać następująco: 
   
    ![Klient sieci VPN](./media/vpn-gateway-point-to-site-create/vpn.png "Klient sieci VPN")

### <a name="part-3-connect-to-azure"></a>Część 3: Nawiązywanie połączenia z platformą Azure
1. Aby nawiązać połączenie z siecią wirtualną na komputerze klienckim, przejdź do połączeń sieci VPN i wyszukaj wcześniej utworzone połączenie sieci VPN. Połączenie będzie miało taką samą nazwę jak sieć wirtualna. Kliknij przycisk **Połącz**. Może pojawić się komunikat podręczny, który odwołuje się do użycia certyfikatu. Jeśli tak się stanie, kliknij przycisk **Kontynuuj**, aby skorzystać z podniesionych uprawnień. 
2. Na stronie stanu **Połączenie** kliknij przycisk **Połącz**, aby rozpocząć połączenie. Jeśli widzisz ekran **Wybierz certyfikat**, sprawdź, czy wyświetlany certyfikat klienta to ten, który ma zostać użyty do nawiązania połączenia. Jeśli nie, kliknij strzałkę na liście rozwijanej, aby wybrać właściwy certyfikat, a następnie kliknij przycisk **OK**.
   
    ![Klient sieci VPN 2](./media/vpn-gateway-point-to-site-create/clientconnect.png "Połączenie klienta sieci VPN")
3. Połączenie powinno zostać nawiązane.
   
    ![Klient sieci VPN 3](./media/vpn-gateway-point-to-site-create/connected.png "Połączenie klienta sieci VPN 2")

> [!NOTE]
> Jeśli używasz certyfikatu wydanego przy użyciu rozwiązania dla przedsiębiorstwa proponowanego przez urząd certyfikacji i występują problemy z uwierzytelnianiem, sprawdź kolejność uwierzytelniania w certyfikacie klienta. Kolejność elementów na liście uwierzytelniania można sprawdzić przez dwukrotne kliknięcie certyfikatu klienta i przejście do pozycji **Szczegóły > Ulepszone użycie klucza**. Upewnij się, że pierwszym elementem na liście jest „Uwierzytelnienie klienta”. Jeśli tak nie jest, certyfikat klienta należy wydać na podstawie szablonu Użytkownik mającego Uwierzytelnienie klienta jako pierwszy element na liście. 
>
>

### <a name="part-4-verify-the-vpn-connection"></a>Część 4: Sprawdzenie połączenia sieci VPN
1. Aby sprawdzić, czy połączenie sieci VPN jest aktywne, otwórz wiersz polecenia z podwyższonym poziomem uprawnień, a następnie uruchom polecenie *ipconfig/all*.
2. Przejrzyj wyniki. Zwróć uwagę na fakt, że otrzymany adres IP jest jednym z adresów z zakresu dla połączenia typu punkt-lokacja określonego podczas tworzenia sieci wirtualnej. Wyniki powinny być podobne do poniższego kodu:

Przykład:

    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## <a name="faq"></a>Często zadawane pytania dotyczące połączeń typu punkt-lokacja

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Następne kroki

Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Aby uzyskać więcej informacji, zobacz [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) (Maszyny wirtualne).

Aby uzyskać więcej informacji na temat sieci wirtualnych, zobacz artykuł [Virtual Network Documentation](/azure/virtual-network) (Dokumentacja sieci wirtualnych).


