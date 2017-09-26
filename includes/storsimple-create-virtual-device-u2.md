#### <a name="to-create-a-virtual-device"></a>Aby utworzyć urządzenie wirtualne
1. W witrynie Azure Portal przejdź do usługi **StorSimple Manager**.
2. Przejdź do strony **Urządzenia**. Kliknij opcję **Utwórz urządzenie wirtualne** u dołu strony **Urządzenia**.
3. W **oknie dialogowym Tworzenie urządzenia wirtualnego** określ następujące informacje.
   
    ![Tworzenie urządzenia wirtualnego StorSimple](./media/storsimple-create-virtual-device-u2/CreatePremiumsva1.png)
   
   1. **Nazwa** — unikatowa nazwa urządzenia wirtualnego.
   2. **Model** — wybierz model urządzenia wirtualnego. To pole jest widoczne tylko, jeśli używasz wersji Update 2 lub nowszej. Model urządzenia 8010 oferuje 30 TB pamięci standardowej, a 8020 ma 64 TB magazynu w usłudze Premium Storage. Wybierz 8010,
   3. aby wdrożyć scenariusze pobierania z kopii zapasowych na poziomie elementów. Wybierz model 8020, aby wdrożyć obciążenia o wysokiej wydajności i małych opóźnieniach, lub użyj go jako urządzenia dodatkowego do odzyskiwania po awarii.
   4. **Wersja** — wybierz wersję urządzenia wirtualnego. W przypadku wybrania modelu urządzenia 8020 pole wersji nie zostanie wyświetlone. Ta opcja jest niedostępna, jeśli wszystkie fizyczne urządzenia zarejestrowane w tej usłudze mają uruchomioną wersję Update 1 (lub nowszą). To pole jest widoczne tylko wtedy, gdy w tej samej usłudze zarejestrowano równocześnie fizyczne urządzenia w wersji Update 1 i wersji starszej. Biorąc pod uwagę, że wersja urządzenia wirtualnego decyduje o tym, które fizyczne urządzenie będzie mogło przejść w tryb pracy awaryjnej lub z którego urządzenia będzie można klonować, ważne jest, by utworzyć odpowiednią wersję urządzenia wirtualnego. Wybierz pozycję:
      
      * Wersja Update 0.3, jeśli będziesz włączać tryb pracy awaryjnej lub odzyskiwanie po awarii z fizycznego urządzenia z uruchomioną wersją Update 0.3 lub starszą. 
      * Wersja Update 1, jeśli będziesz włączać tryb pracy awaryjnej lub klonować z fizycznego urządzenia z uruchomioną wersją Update 1 (lub nowszą). 
   5. **Sieć wirtualna** — określ sieć wirtualną, której chcesz użyć do tego urządzenia wirtualnego. W przypadku korzystania z usługi Premium Storage (w wersji Update 2 lub nowszej) musisz wybrać sieć wirtualną obsługiwaną za pomocą konta w usłudze Premium Storage. Nieobsługiwane sieci wirtualne będą wyszarzone na liście rozwijanej. W przypadku wybrania nieobsługiwanej sieci wirtualnej zostanie wyświetlone ostrzeżenie. 
   6. **Konto magazynu do tworzenia urządzenia wirtualnego** — wybierz konto magazynu do przechowywania obrazu urządzenia wirtualnego podczas inicjowania obsługi. Musi ono należeć do tego samego regionu co urządzenie wirtualne i sieć wirtualna. Nie powinno być używane do przechowywania danych przez urządzenie fizyczne ani wirtualne. Domyślnie do tego celu zostanie utworzone nowe konto magazynu. Jednak jeśli wiadomo, że masz już konto magazynu odpowiednie do tego celu, możesz wybrać je z listy. W przypadku tworzenia urządzeń wirtualnych premium lista rozwijana będzie zawierać tylko konta w usłudze Premium Storage. 
      
      > [!NOTE]
      > Urządzenie wirtualne może pracować tylko z kontami usługi Azure Storage. Inni dostawcy usługi w chmurze, np. Amazon, HP i OpenStack (obsługiwani przez urządzenie fizyczne) nie są obsługiwani przez urządzenia wirtualne StorSimple.
      > 
      > 
   7. Kliknij znacznik wyboru, aby wskazać, że wiesz, iż dane przechowywane na urządzeniu wirtualnym będą obsługiwane w centrum danych firmy Microsoft. Kiedy używasz tylko urządzenia fizycznego, klucz szyfrowania jest przechowywany z urządzeniem, więc firma Microsoft nie może go odszyfrować. 
      
       Kiedy używasz urządzenia wirtualnego, zarówno klucz szyfrowania, jak i klucz odszyfrowywania są przechowywane na platformie Microsoft Azure. Aby uzyskać więcej informacji, zobacz temat [Zagadnienia dotyczące zabezpieczeń podczas używania urządzenia wirtualnego](../articles/storsimple/storsimple-security.md).
   8. Kliknij ikonę wyboru, aby utworzyć urządzenie wirtualne. Zainicjowanie obsługi urządzenia może zająć około 30 minut.
      
      ![Etap tworzenia urządzenia wirtualnego StorSimple](./media/storsimple-create-virtual-device-u2/StorSimple_VirtualDeviceCreating1M.png)

