

## <a name="what-is-happening"></a>Co się dzieje?

[3 stycznia ujawniono](https://googleprojectzero.blogspot.com/2018/01/reading-privileged-memory-with-side.html) sprzętową lukę w zabezpieczeniach obejmującą całą branżę. Utrzymywanie klientów bezpiecznego jest zawsze naszym głównym priorytetem i firma Microsoft biorą aktywnych czynności, aby upewnić się, że Azure klienta jest narażony na te luki w zabezpieczeniach.

Z publicznego ujawniania luki w zabezpieczeniach firma Microsoft [przyspieszony chronometrażu zaplanowanej konserwacji](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/) i rozpoczęła automatyczne ponowne uruchomienie maszyn wirtualnych, które nadal potrzebna aktualizacji.


## <a name="how-can-i-see-which-of-my-vms-are-already-updated"></a>Jak sprawdzić, które z maszyn wirtualnych są już zaktualizowany? 

Możesz zerknąć na stan maszyn wirtualnych. Jeśli ponowne uruchomienie zostało zakończone, będzie to widoczne na [liście maszyn wirtualnych w witrynie Azure Portal](https://aka.ms/T08tdc). Maszyny wirtualne są wymieniane jako „Już zaktualizowane”, jeśli aktualizacja została zastosowana, lub „Zaplanowane”, jeśli aktualizacja nadal jest wymagana. Jeśli chcesz zobaczyć tylko „Zaplanowane” maszyny wirtualne, zobacz usługę [Azure Service Health](https://portal.azure.com/).

## <a name="can-i-find-out-exactly-when-my-vms-will-be-rebooted"></a>Można sprawdzić dokładnie po maszyn wirtualnych zostanie uruchomiony ponownie?

Najlepszy sposób pozyskania alert o ponownym uruchomieniu komputera jest skonfigurowanie [zaplanowane zdarzenia](https://docs.microsoft.com/azure/virtual-machines/windows/scheduled-events). Zapewnia powiadomienie 15 minut maszyny wirtualnej, przechodząc w dół z powodu konserwacji.

## <a name="can-i-manually-redeploy-now-to-perform-the-required-maintenance"></a>Można I ręcznie wdrożyć ponownie teraz, aby wykonać wymagane działania konserwacyjne? 

Nie możemy zagwarantować, że na hoście zaktualizowane zostaną przydzielone ponownie wdrożonym maszyny Wirtualnej. W miarę możliwości sieci szkieletowej Azure spróbuje przydzielić maszyn wirtualnych do hostów, które już zostały zaktualizowane. Istnieje możliwość, że ponownego wdrażania maszyny Wirtualnej można trafić na hoście z systemem innym niż zaktualizowane, w którym to przypadku mogą zostać naliczone drugiego ponownego uruchomienia, wymuszone w ramach zaplanowanej konserwacji. W związku z tym wdraża ponownie ręcznie nie zaleca się uniknąć tego problemu.

## <a name="how-long-will-the-reboot-take"></a>Jak długo będzie trwać ponownego rozruchu? 

Ponowne uruchomienie większości trwa około **30 minut**.

## <a name="does-the-guest-os-need-to-be-updated"></a>Czy należy zaktualizować system operacyjny gościa? 

Ta aktualizacja infrastruktury platformy Azure adresów lukę w zabezpieczeniach na poziomie funkcji hypervisor i nie wymaga aktualizacji obrazów systemu Windows lub maszyny Wirtualnej systemu Linux. Jako zawsze należy kontynuować do zastosowania najlepsze rozwiązania dotyczące obrazów maszyny Wirtualnej. Należy zapoznać się z dostawcą systemów operacyjnych, aktualizacji i instrukcje, zgodnie z potrzebami. W przypadku klientów maszyn wirtualnych systemu Windows Server wytyczne zostały już opublikowane i są dostępne [tutaj](../articles/virtual-machines/windows/mitigate-se.md).

## <a name="will-there-be-a-performance-impact-as-a-result-of-resolving-this-update"></a>Będzie negatywny wpływ na wydajność wyniku rozpoznawania tej aktualizacji?

Większość klientów platformy Azure nie widzieli zauważalnego wpływu dzięki tej aktualizacji. Pracowaliśmy nad zoptymalizowaniem procesora CPU i ścieżki We/Wy dysku i nie dostrzegamy widocznego wpływu na wydajność po zastosowaniu poprawki. Niewielka część klientów może doświadczyć wpływu na wydajność sieci. Można temu zaradzić, za pomocą sieci Azure przyspieszony [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell) lub [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli), który jest bezpłatna możliwości dostępne dla wszystkich klientów platformy Azure.

## <a name="i-follow-your-recommendations-for-high-availability-will-my-environment-remain-highly-available-during-the-reboot"></a>I postępować zgodnie z zaleceniami wysokiej dostępności, Moje środowisko pozostanie wysokiej dostępności podczas ponownego uruchamiania?

Tak, ustaw maszyn wirtualnych wdrożonych w dostępności lub zestawy skalowania maszyny wirtualnej ma konstrukcja aktualizację domeny (UD). Podczas przeprowadzania konserwacji, Azure będzie honorować ograniczenia UD i nie zostanie wykonany ponowny rozruch maszyny wirtualne z różnych UD (w tym samym zestawie dostępności). Aby uzyskać więcej informacji o wysokiej dostępności, zapoznaj się [Zarządzaj dostępnością maszyn wirtualnych systemu Windows na platformie Azure](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) lub [Zarządzaj dostępnością maszyn wirtualnych systemu Linux na platformie Azure](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability).

## <a name="i-have-architected-my-business-continuitydisaster-recovery-plan-using-region-pairs-will-reboots-to-my-vms-occur-in-region-pairs-at-the-same-time"></a>Zostały zaprojektowane pod Mój planu odzyskiwania ciągłości/danych biznesowych przy użyciu pary regionu. Ponowne uruchomienie do moich maszyn wirtualnych nastąpi parami regionu w tym samym czasie?

Normalnie zdarzenia konserwacji planowanej platformy Azure są realizowane w sparowanych regionach w innym czasie, aby zminimalizować ryzyko zakłóceń w obu regionach. Jednak ze względu na pilny charakter aktualizację zabezpieczeń, firma Microsoft są zetknie aktualizacji do wszystkich regionów jednocześnie.

## <a name="what-about-paas-services-on-azure"></a>Co PaaS usług Azure?  

Usług platformy Azure, w tym sieci web i mobilnych, usług danych, IoT, niekorzystającą, itd. zostały rozwiązane lukę w zabezpieczeniach. Brak akcji wymagane dla klientów korzystających z tych usług.

## <a name="intel-released-additional-guidance-on-january-22-2018-related-to-the-security-vulnerabilities--will-this-guidance-cause-any-additional-maintenance-activities-by-azure"></a>Intel opublikowała dodatkowe wskazówki dotyczące 22 stycznia 2018 związane z luk w zabezpieczeniach.  W tych wskazówkach spowoduje żadnych działań obsługi dodatkowych przez platformę Azure?  

Nie ma wpływu na czynniki Azure wcześniej ogłaszane na 3 stycznia 2018 [zaktualizowano wskazówki](https://newsroom.intel.com/news/root-cause-of-reboot-issue-identified-updated-guidance-for-customers-and-partners/) z Intel. Nie będzie żadnej aktywności obsługi dodatkowych na klienta maszyn wirtualnych w wyniku tego nowych informacji.
 

## <a name="next-steps"></a>Kolejne kroki

Aby dowiedzieć się więcej, zobacz [Zabezpieczanie klientów systemu Azure z usterka Procesora](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).
