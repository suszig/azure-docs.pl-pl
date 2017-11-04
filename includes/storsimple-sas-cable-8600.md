<!--author=alkohli last changed:02/22/16-->

#### <a name="to-attach-the-sas-cables"></a>Aby dołączyć kabli SAS
1. Zidentyfikuj podstawową i obudowy EBOD. Dwa załączniki można zidentyfikować analizując ich odpowiednich płaszczyzn Wstecz. Zobacz poniższy obraz, aby uzyskać wskazówki. 
   
    ![Utwórz kopię płaszczyzny podstawowej i obudowy EBOD](./media/storsimple-sas-cable-8600/HCSBackplaneofprimaryandEBODenclosure.png)
   
    **Widok podstawowych i EBOD obudowy do tyłu**
   
   | Etykieta | Opis |
   |:--- |:--- |
   | 1 |Obudowa podstawowego |
   | 2 |Obudowa EBOD |
2. Znajdź numery seryjne z podstawowej i obudowy EBOD. Do tyłu czyść z każdej obudowy umieszcza się na naklejce numeru seryjnego. Numery seryjne musi być taka sama w obu obudów. [Skontaktuj się z Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) natychmiast, jeżeli numery seryjne są niezgodne. Zobacz poniższy rysunek można znaleźć numery seryjne.
   
    ![Widok tyłu przedstawiający numer seryjny obudowy](./media/storsimple-sas-cable-8600/HCSRearviewofenclosureindicatinglocationofserialnumbersticker.png)
   
    **Lokalizacja naklejce numer seryjny**
   
   | Etykieta | Opis |
   |:--- |:--- |
   | 1 |Wyczyść obudowa |
3. Za pomocą podanego kabli SAS obudowa EBOD połączyć się z podstawowym obudowa w następujący sposób:
   
   1. Zidentyfikuj czterech portów SAS głównej obudowy i obudowy EBOD. Portów SAS są oznaczone jako EBOD na głównej obudowa i odpowiadają A portu na obudowę EBOD, jak pokazano w SAS okablowania ilustracji poniżej.
   2. Umożliwia łączenie A. porty EBOD podana kabli SAS
   3. Port EBOD kontrolera 0 powinny być podłączone do port A EBOD kontrolera 0. Port EBOD kontrolera 1 powinny być podłączone do port A EBOD kontrolera 1. Zobacz poniższy rysunek wskazówki. 
      
      ![Sygnatury dostępu Współdzielonego okablowania dla urządzenia](./media/storsimple-sas-cable-8600/HCSSAScablingforyourdevice.png)
      
      **Sygnatury dostępu Współdzielonego okablowania**
      
      | Etykieta | Opis |
      |:--- |:--- |
      | A |Obudowa podstawowego |
      | B |Obudowa EBOD |
      | 1 |Kontrolera 0 |
      | 2 |Kontrolera 1 |
      | 3 |EBOD kontrolera 0 |
      | 4 |EBOD kontrolera 1 |
      | 5, 6 |Portów SAS w głównej obudowy (etykietą EBOD) |
      | 7, 8 |Portów SAS w obudowie EBOD (Port A) |

