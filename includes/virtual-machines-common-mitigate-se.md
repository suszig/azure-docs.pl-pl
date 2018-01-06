
 
Ostatnie ujawnienie [nowa klasa luk w zabezpieczeniach Procesora](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) znany jako rozważana wykonywania ataków kanału po stronie spowodowało pytania, korzystając z wyszukiwania jasności więcej klientów. 

 
## <a name="azure-infrastructure"></a>Infrastruktury platformy Azure

Problemy opisane ujawnieniem luk w zabezpieczeniach może służyć do obejścia granicy funkcji hypervisor i spowodować ujawnienie pamięci między dwoma współhostowanym maszyn wirtualnych. Zgłoszonej we wcześniejszej [wpis w blogu](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/), Azure została zastosowana środki zaradcze ochrony klientów przed tę lukę w zabezpieczeniach.  Firma Microsoft zaleca zawsze, że klienci stosować najlepsze rozwiązania dotyczące obrazy maszyny Wirtualnej, takie jak instalowanie wszystkich aktualizacji zabezpieczeń od swojego dostawcy systemu operacyjnego.

## <a name="paas-services-on-azure"></a>PaaS usług Azure
Azure PaaS oferty mają środki zaradcze wdrażać domyślnie. Klienci są wymagane żadne działania. Poniżej znajduje się wyjątek usługi w chmurze Azure.  


## <a name="azure-cloud-services"></a>usług Azure Cloud Services

[Usługi w chmurze Azure](https://azure.microsoft.com/services/cloud-services/) z automatycznej aktualizacji włączane automatycznie otrzymywać wersji systemu operacyjnego gościa, który zawiera środki zaradcze te luki w zabezpieczeniach. 

Następujące wersje systemu operacyjnego gościa obejmują aktualizacje z zabezpieczenia przed lukami kanału po stronie rozważana wykonywania:

* WA-GOŚCIA-OS-5.15_201801-01
* WA-GOŚCIA-OS-4.50_201801-01


Niewielkiej liczby klientów korzystających z usługi w chmurze Azure do hostowania kodzie niezaufanym należy również włączyć [jądra wirtualny adres przesłanianie](#enabling-kernel-virtual-address-shadowing-on-windows-server) oprócz aktualizacji systemu operacyjnego gościa. Zapewnia dodatkową ochronę przed rozważana wykonywania kanału po stronie luk w zabezpieczeniach. Można to zrobić za pomocą zadania uruchamiania. Więcej informacji o tym, które klientów i scenariusze użycia wymagają tej funkcji i włącz ją, znajduje się poniżej.


## <a name="azure-virtual-machines-windows--linux"></a>Maszyny wirtualne platformy Azure (z systemem Windows i Linux)

Firma Microsoft zaleca zawsze klientom zainstalowanie wszystkich aktualizacji zabezpieczeń. 

Pakietu zabezpieczeń 2018 stycznia rozwiązuje te luki w zabezpieczeniach i inne. Sprawdź, czy używasz obsługiwanej aplikacji oprogramowania antywirusowego przed zainstalowaniem aktualizacji systemu operacyjnego. Skontaktuj się z dostawcą oprogramowania antywirusowego, aby uzyskać informacje o zgodności. 

Na luki w zabezpieczeniach rozważana wykonywania adres, aktualizacje Linux jądra będzie wymagane i można uzyskać od dostawcy dystrybucji, jeśli jest dostępna. 

Niewielka liczba klientów przy użyciu maszyn wirtualnych platformy Azure (system Windows) do obsługi w kodzie niezaufanym należy również włączyć [jądra wirtualny adres przesłanianie](#enabling-kernel-virtual-address-shadowing-on-windows-server) zapewniające dodatkową ochronę przed kanału po stronie rozważana wykonywania luki w zabezpieczeniach.  Więcej informacji na temat których klientów i ich użycia scenariusze wymagają tej funkcji i włącz ją podano poniżej.


## <a name="enabling-kernel-virtual-address-shadowing-on-windows-server"></a>Włączanie wirtualnego adresu jądra przesłanianie w systemie Windows Server

Klienci korzystający z systemu Windows Server można uruchomić kod z niezaufanej należy włączyć funkcję jądra wirtualny adres przesłanianie ochronę systemów gdzie kodzie niezaufanym jest wykonywany z uprawnieniami użytkownika niski.

Ta dodatkowa ochrona mogą wpływać na wydajność i jest domyślnie wyłączona. Jądra wirtualny adres przesłanianie chroni przed ujawnieniem informacji do proces i procesu jądra.

Sieci maszyn wirtualnych, usługi w chmurze lub serwerów lokalnych są narażeni na ataki, jeśli ich można podzielić na następujące kategorie:

* Azure zagnieżdżone hosty funkcji Hyper-V wirtualizacji
* Hosty (RDSH) usług pulpitu zdalnego
* Maszyny wirtualne i usługi w chmurze systemem kodzie niezaufanym, takich jak kontenery lub niezaufanych rozszerzeń dla bazy danych zawartości z niezaufanych sieci web lub obciążeń które Uruchom kod, który jest dostępne ze źródeł zewnętrznych.

Przykładowy scenariusz, gdy jest konieczne jądra wirtualny adres przesłanianie: 

|     |
|-----|
|Maszyna wirtualna platformy Azure jest uruchomiona usługa którego niezaufanym użytkownikom można przesłać kod JavaScript, który jest wykonywany z ograniczonymi uprawnieniami. Na tej samej maszyny Wirtualnej istnieje wysoko uprzywilejowane procesu, który zawiera dane poufne, które nie powinny być dostępne dla tych użytkowników. W takiej sytuacji wirtualnego przesłanianie adresów jądra jest niezbędne w celu zapewnienia ochrony przed ujawnieniem między nimi.|
|     | 

Szczegółowe instrukcje na temat włączania jądra wirtualny adres przesłanianie są dostępne za pośrednictwem [systemu Windows Server KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution).


> [!NOTE]
> W momencie publikacji wirtualnego przesłanianie adresów jądra jest dostępna tylko w systemie Windows Server 2016, Windows Server 2012 R2 i Windows Server 2008 R2.  
>
>

Jeśli używasz serwera systemu Linux, skontaktuj się z dostawcą systemów operacyjnych, aktualizacji i instrukcje.

## <a name="branch-target-injection-mitigation-support-microcode"></a>Gałęzi docelowych środki zaradcze iniekcji pomocy technicznej (mikrokodu)

Klienci korzystający z narzędzia, które wykryć obecność na podstawie mikrokodu środki zaradcze raport, że platforma Azure jest atakowany. Są one nieprawidłowe. W momencie publikacji gałęzi docelowych iniekcji ograniczenia obsługi nie jest uwidaczniana od funkcji Hypervisor Azure na maszynach wirtualnych platformy Azure lub usługi w chmurze Azure. Oznacza to, że maszyny wirtualne znają istnienia mikrokodu i nie można użyć jej zestaw zwiększonej instrukcji. Oznacza to, że platforma Azure jest narażony na ataki kanału po stronie rozważana wykonywania cross-VM.
 
Firma Microsoft pracy z głównym partnerami z branży dodatkowe aktualizacje mogą stać się dostępne.

## <a name="next-steps"></a>Kolejne kroki

Aby dowiedzieć się więcej, zobacz [Zabezpieczanie klientów systemu Azure z usterka Procesora](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).