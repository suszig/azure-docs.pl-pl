
**Dokument ostatniej aktualizacji**: 6 stycznia 6:30 strefy PST.

Ostatnie ujawnienie [nowa klasa luk w zabezpieczeniach Procesora](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) znany jako rozważana wykonywania ataków kanału po stronie spowodowało pytania, korzystając z wyszukiwania jasności więcej klientów.  

Infrastrukturę, która uruchamia Azure i izoluje obciążeń klientów od siebie jest chroniony.  Oznacza to, że innych klientów działających na platformie Azure nie ataki aplikację za pomocą te luki w zabezpieczeniach.

## <a name="keeping-your-operating-systems-up-to-date"></a>Aktualizowanie przez systemy operacyjne

Podczas aktualizacji systemu operacyjnego nie jest wymagany do izolowania aplikacji działających na platformie Azure z innych klientów działających na platformie Azure, zawsze jest najlepszym rozwiązaniem w celu zapewnienia aktualności Twojej wersji systemu operacyjnego. 

W poniższych ofertach Oto nasze zalecane działania, aby zaktualizować System operacyjny: 

<table>
<tr>
<th>Oferty</th> <th>Zalecana akcja </th>
</tr>
<tr>
<td>usług Azure Cloud Services </td>  <td>Włącz Aktualizacje automatyczne lub upewnij się, że używasz najnowszej systemu operacyjnego gościa.</td>
</tr>
<tr>
<td>Maszyny wirtualne systemu Linux platformy Azure</td> <td>Zainstaluj aktualizacje z dostawcą systemu operacyjnego, jeśli jest dostępna. </td>
</tr>
<tr>
<td>Maszyny wirtualne systemu Windows Azure </td> <td><ul><li>Sprawdź, czy używasz obsługiwanej aplikacji oprogramowania antywirusowego przed zainstalowaniem aktualizacji systemu operacyjnego. Skontaktuj się z dostawcą oprogramowania antywirusowego, aby uzyskać informacje o zgodności. </li> <li> Zainstaluj [zbiorczy zabezpieczeń stycznia](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002). </li></ul></td>
</tr>
<tr>
<td>Inne usługi Azure PaaS</td> <td>Brak akcji wymagane dla klientów korzystających z tych usług. Azure automatycznie aktualizuje Twojej wersji systemu operacyjnego aktualne. </td>
</tr>
</table>

## <a name="additional-guidance-if-you-are-running-untrusted-code"></a>Dodatkowe wskazówki, jeśli używasz kodzie niezaufanym 

Uruchomić kod z niezaufanej nie jest potrzebne nie działanie dodatkowe klienta. Jeśli zezwolisz na kod, który nie masz zaufania (na przykład można zezwolić jednemu z klientami w celu przekazania pliku binarnego lub fragment kodu, który następnie wykonaj w chmurze w aplikacji), a następnie należy wykonać następujące dodatkowe czynności.  


### <a name="windows"></a>Windows 
Jeśli używasz systemu Windows i obsługujący kodzie niezaufanym, należy również włączyć funkcję Windows polu Przesłanianie wirtualnych adresów jądra (KVA), co zapewnia dodatkową ochronę przed lukami kanału po stronie rozważana wykonywania. Ta funkcja jest domyślnie wyłączona i może wpłynąć na wydajność, jeśli jest włączona. Postępuj zgodnie z [systemu Windows Server KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) instrukcje dotyczące włączania ochrony na serwerze. Jeśli korzystasz z usług Azure Cloud Services, sprawdź, czy korzystasz z WA-GOŚCIA — systemu operacyjnego — 5.15_201801-01 lub WA-GOŚCIA — systemu operacyjnego — 4.50_201801-01 (dostępne rozpoczyna się w styczniu 10) i Włącz rejestru klucza za pomocą zadania uruchamiania.


### <a name="linux"></a>Linux
Jeśli korzysta z systemu Linux i hosting kodzie niezaufanym, należy również zaktualizować Linux do nowszej wersji, która implementuje izolacja tabeli stron jądra (KPTI), która oddziela tabele stron używane przez jądro z elementów należących do przestrzeni. Czynniki te wymagają aktualizacji systemu operacyjnego Linux oraz można uzyskać od dostawcy dystrybucji, jeśli jest dostępna. Dostawca systemu operacyjnego można stwierdzić, czy zabezpieczenia są włączone lub wyłączone domyślnie.








## <a name="next-steps"></a>Kolejne kroki

Aby dowiedzieć się więcej, zobacz [Zabezpieczanie klientów systemu Azure z usterka Procesora](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).