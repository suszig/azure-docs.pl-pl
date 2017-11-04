# <a name="using-cdn-for-azure"></a>Za pomocą sieci CDN dla platformy Azure
Azure Content Delivery Network (CDN) oferuje deweloperom globalne rozwiązanie umożliwiające dostarczanie zawartości wysokiej przepustowości przez buforowanie obiektów blob i zawartości statycznej wystąpień obliczeń w węzłach fizycznych w Stanów Zjednoczonych, Europa, Azja, Australia i Azji. Aby uzyskać bieżącą listę lokalizacje węzłów CDN, zobacz [lokalizacje węzłów CDN Azure].

To zadanie obejmuje następujące kroki:

* [Krok 1: Utwórz konto magazynu](#Step1)
* [Krok 2: Utwórz nowy punkt końcowy CDN dla konta magazynu](#Step2)
* [Krok 3: Dostęp do zawartości w sieci CDN](#Step3)
* [Krok 4: Usuń zawartość usługi CDN](#Step4)

Korzyści wynikające ze stosowania sieci CDN do buforowania danych Azure obejmują:

* Lepszą wydajność i środowisko dla użytkowników końcowych, którzy są daleko od źródła zawartości i są używane aplikacje, gdzie wiele "internet podróży" są wymagane do załadowania zawartości
* Dużą skalę rozproszonych, aby lepiej obsługiwać natychmiastowe wysokie obciążenie, powiedzieć na początku zdarzenia, takie jak uruchamianie produktu

Istniejących klientów usługi CDN można teraz używać usługi Azure CDN w [klasycznego portalu Azure]. Usługa CDN jest funkcją dodatek do subskrypcji i ma oddzielnej [plan rozliczeniowy].

<a id="Step1"> </a>

<h2>Krok 1: Utwórz konto magazynu</h2>

Poniższa procedura umożliwia utworzenie nowego konta magazynu dla subskrypcji platformy Azure. Konto magazynu zapewnia dostęp do usług magazynu platformy Azure. Konto magazynu reprezentuje najwyższy poziom przestrzeni nazw do uzyskiwania dostępu do poszczególnych składników usługi Azure storage: obiektów Blob usługi, usługi kolejki i usługi tabeli. Aby uzyskać więcej informacji dotyczących usług magazynu Azure, zobacz [przy użyciu usług magazynu Azure](http://msdn.microsoft.com/library/azure/gg433040.aspx).

Aby utworzyć konto magazynu, musi być administratorem usługi albo współadministratorem subskrypcji skojarzone.

> [!NOTE]
> Informacje dotyczące wykonywania tej operacji przy użyciu interfejsu API zarządzania usługi Azure, zobacz [Utwórz konto magazynu](http://msdn.microsoft.com/library/windowsazure/hh264518.aspx) temat referencyjny.
> 
> 

**Aby utworzyć konto magazynu dla subskrypcji platformy Azure**

1. Zaloguj się do [klasycznego portalu Azure].
2. W lewym dolnym rogu kliknij **nowy**. W **nowy** zaznacz pozycję **usług danych**, następnie kliknij przycisk **magazynu**, następnie **szybkie tworzenie**.
   
   **Utwórz konto magazynu** zostanie wyświetlone okno dialogowe.
   
   ![Tworzenie konta magazynu][create-new-storage-account]
3. W **adres URL** wpisz nazwę domeny podrzędnej. Ten wpis może zawierać od 3 do 24 małych liter i cyfr.
   
    Ta wartość używana jako nazwa hosta w identyfikatorze URI, który jest wykorzystywana do adresowania obiektów Blob, kolejki lub tabeli zasobów dla subskrypcji. W celu adresu zasobu kontenera w usłudze obiektów Blob, należy użyć identyfikatora URI w następującym formacie, gdzie  *&lt;StorageAccountLabel&gt;*  odnosi się do wartości wpisanych w **wprowadź adres URL**:
   
    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mojkontener&gt;*
   
    **Ważne:** adresu URL etykiety formularze poddomeną identyfikatora URI konta magazynu i musi być unikatowa wśród wszystkich usług hostowanych na platformie Azure.
   
    Ta wartość jest także używana jako nazwa tego konta magazynu w portalu lub podczas uzyskiwania dostępu do tego konta programowo.
4. Z **grupy Region/koligacji** listy rozwijanej, wybierz region lub koligacji grupy dla konta magazynu. Wybierz grupę koligacji zamiast region, jeśli chcesz usług magazynu w tym samym centrum danych z innymi usługami systemu Windows Azure, które są używane. Może to zwiększyć wydajność, a nie jest obciążany za wyjście.  
   
   **Uwaga:** Aby utworzyć grupę koligacji, otwórz **ustawienia** obszarze portalu zarządzania, kliknij przycisk **grup koligacji**, a następnie kliknij przycisk **Dodaj grupę koligacji** lub **dodać**. Tworzyć i zarządzać przy użyciu interfejsu API systemu Windows Azure Service Management grup koligacji. Aby uzyskać więcej informacji zobacz [operacje na grupach koligacji].
5. Z **subskrypcji** listy rozwijanej wybierz subskrypcję, która zostanie użyte konto magazynu z.
6. Kliknij pozycję **Utwórz konto usługi Storage**. Proces tworzenia konta magazynu może potrwać kilka minut.
7. Aby zweryfikować, że pomyślnie utworzono konto magazynu, sprawdź, czy konto jest wyświetlany w elementach wymienionych dla **magazynu** ze stanem **Online**.

<a id="Step2"> </a>

<h2>Krok 2: Utwórz nowy punkt końcowy CDN dla konta magazynu</h2>

Po włączeniu sieci CDN w warstwie dostępu do konta magazynu lub usługi hostowanej, wszystkie obiekty publicznie dostępnych kwalifikują się do buforowania krawędzi CDN. Jeśli zmodyfikujesz obiekt, który jest aktualnie w pamięci podręcznej w sieci CDN nową zawartość nie będzie dostępne za pośrednictwem sieci CDN dopóki CDN odświeża jego zawartości, gdy pamięci podręcznej zawartości okres czasu wygaśnięcia.

**Aby utworzyć nowy punkt końcowy CDN dla konta magazynu**

1. W [klasycznego portalu Azure], w okienku nawigacji kliknij **CDN**.
2. Na wstążce kliknij **nowy**. W **nowy** okno dialogowe, wybierz opcję **usługi aplikacji**, następnie **CDN**, następnie **szybkie tworzenie**.
3. W **domeny pochodzenia** listy rozwijanej wybierz magazyn utworzonego konta w poprzedniej sekcji z listy kont dostępny magazyn. 
4. Kliknij przycisk **Utwórz** przycisk, aby utworzyć nowy punkt końcowy.
5. Po utworzeniu punktu końcowego, zostanie wyświetlony na liście punktów końcowych dla subskrypcji. Widok listy zawiera adres URL służący do uzyskiwania dostępu do zawartości buforowanej (w pamięci podręcznej), a także domeny źródła. 
   
    Domeny pochodzenia jest to lokalizacja, z którego CDN buforuje zawartość. Domeny pochodzenia może być konta magazynu lub usługi w chmurze; Konto magazynu jest używany na potrzeby tego przykładu. Magazyn zawartość jest buforowana na serwerach krawędzi zgodnie z do ustawienia nagłówka cache-control, który określisz, lub heurystyki domyślne buforowania sieci. 

    > [AZURE.NOTE]Konfiguracja utworzone dla punktu końcowego nie będzie natychmiast dostępna; może upłynąć do 60 minut rejestracji propagację za pośrednictwem sieci CDN. Użytkownicy, którzy spróbują użyć nazwy domeny usługi CDN natychmiast może zostać wyświetlony kod stanu 400 (nieprawidłowe żądanie), dopóki zawartość jest dostępna za pośrednictwem sieci CDN.

<a id="Step3"> </a>

<h2>Krok 3: Dostęp do zawartości w sieci CDN</h2> 

Aby uzyskać dostęp do zawartości w pamięci podręcznej w sieci CDN, użyj adresu URL CDN, w portalu. Adres pamięci podręcznej blob będzie podobny do następującego:

http://<*CDNNamespace*\>.vo.msecnd.net/ <*myPublicContainer*\>/<*element BlobName*\>

<a id="Step4"> </a>

<h2>Krok 4: Usuwanie zawartości z sieci CDN</h2>

Jeśli nie chcesz już w pamięci podręcznej obiektu w usłudze Azure sieci dostarczania zawartości (CDN), można wykonać jedną z następujących czynności:

* Dla obiektów blob platformy Azure można usunąć obiektu blob w kontenerze publicznego.
* Można utworzyć kontenera prywatnego zamiast publicznego. Zobacz [ograniczanie dostępu do kontenerów i obiektów blob](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/#restrict-access-to-containers-and-blobs) Aby uzyskać więcej informacji.
* Można wyłączyć lub usunąć punkt końcowy CDN za pomocą portalu zarządzania.
* Można zmodyfikować usługi hostowanej nie będzie odpowiadać na żądania dla obiekt.

Obiekt w sieci CDN już pamięci podręcznej pozostaje w pamięci podręcznej aż do zakończenia okresu czasu wygaśnięcia dla obiekt. Po wygaśnięciu okresu time-to-live CDN sprawdzi, czy punkt końcowy CDN jest nadal ważny i nadal anonimowo dostępny obiekt. Jeśli nie, obiekt już być buforowane.

Możliwość natychmiast przeczyścić zawartość nie jest obecnie obsługiwane w portalu zarządzania Azure. Skontaktuj się z [pomocy technicznej platformy Azure](https://azure.microsoft.com/support/options/) Jeśli chcesz natychmiast przeczyścić zawartości. 

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Jak utworzyć grupę koligacji w Azure]
* [Porady: Zarządzanie kontami magazynu dla subskrypcji platformy Azure]
* [Temat interfejsu API zarządzania usługami]
* [Jak zamapować zawartość usługi CDN na domenę niestandardową]

[Create Storage Account]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/
[lokalizacje węzłów CDN Azure]: http://msdn.microsoft.com/library/windowsazure/gg680302.aspx
[klasycznego portalu Azure]: https://manage.windowsazure.com/
[plan rozliczeniowy]: /pricing/calculator/?scenario=full
[Jak utworzyć grupę koligacji w Azure]: http://msdn.microsoft.com/library/azure/ee460798.aspx
[Overview of the Azure CDN]: http://msdn.microsoft.com/library/windowsazure/ff919703.aspx
[Temat interfejsu API zarządzania usługami]: http://msdn.microsoft.com/library/windowsazure/ee460807.aspx
[Jak zamapować zawartość usługi CDN na domenę niestandardową]: http://msdn.microsoft.com/library/windowsazure/gg680307.aspx


[create-new-storage-account]: ./media/cdn/CDN_CreateNewStorageAcct.png
[Previous Management Portal]: ../../Shared/Media/previous-portal.png
