---
title: "Magazyn plików Azure instalacji z maszyny Wirtualnej systemu Windows Azure | Dokumentacja firmy Microsoft"
description: "Przechowywanie plików w chmurze za pomocą usługi Azure file storage i instalowanie udziału plików w chmurze z maszyny wirtualnej platformy Azure (VM)."
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: 6ffb2d2da1e2439df6f5da543411e3c2c68d3435
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-file-shares-with-windows-vms"></a>Udziały plików platformy Azure za pomocą maszyn wirtualnych systemu Windows 

Korzystać z udziałów plików na platformę Azure, jako sposób przechowywania i uzyskać dostęp do plików z maszyny Wirtualnej. Na przykład można przechowywać skryptu lub pliku konfiguracji aplikacji, które wszystkich maszyn wirtualnych do udziału. W tym temacie zostanie przedstawiony zostanie sposób tworzenia i instalowanie udziału plików na platformę Azure oraz sposób przekazywania i pobierania plików.

## <a name="connect-to-a-file-share-from-a-vm"></a>Połączenia z udziałem plików z maszyny Wirtualnej

W tej sekcji założono, że masz już udział plików, który chcesz nawiązać połączenie. Jeśli potrzebujesz go utworzyć, zobacz [utworzyć udział plików](#create-a-file-share) dalszej części tego tematu.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W menu po lewej stronie kliknij **kont magazynu**.
3. Wybierz konto magazynu.
4. W **omówienie** w obszarze **usług**, wybierz pozycję **pliki**.
5. Wybierz udział plików.
6. Kliknij przycisk **Connect** aby otworzyć stronę przedstawiono składnię wiersza polecenia instalowanie udziału plików z systemem Windows lub Linux.
7. Wyróżnij składnię polecenia i wklej go w Notatniku lub w innym miejscu gdzie możesz łatwo do niego dostęp. 
8. Edytuj składni, aby usunąć wiodące ** > ** i Zastąp *[litera dysku]* z literą dysku (na przykład **/Y:**) gdzie chcesz zainstalować udział plików.
8. Połącz się z maszyną Wirtualną i otwórz wiersz polecenia.
9. Wklej w składni edytowanych połączenia i trafień **Enter**.
10. Po utworzeniu połączenia jest wyświetlany komunikat **polecenie zostało wykonane pomyślnie.**
11. Sprawdzanie połączenia, wpisując w literę dysku, aby przełączyć się do tego dysku, a następnie wpisz **dir** Aby wyświetlić zawartość udziału plików.



## <a name="create-a-file-share"></a>Tworzenie udziału plików 
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W menu po lewej stronie kliknij **kont magazynu**.
3. Wybierz konto magazynu.
4. W **omówienie** w obszarze **usług**, wybierz pozycję **pliki**.
5. Na stronie usługi plików, kliknij przycisk **+ udział pliku** do utworzenia pierwszego udziału plików. \
6. Wprowadź nazwę udziału plików. Nazwy udziałów plików można użyć, małe litery, cyfry i łączniki pojedynczego. Nazwa nie może rozpoczynać się od łącznika i nie można używać wielu łączników obok siebie. 
7. Wprowadź limit na rozmiar, jaki może to być plik, do 5120 GB.
8. Kliknij przycisk **OK** wdrażania udziału plików.
   
## <a name="upload-files"></a>Przekazywanie plików
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W menu po lewej stronie kliknij **kont magazynu**.
3. Wybierz konto magazynu.
4. W **omówienie** w obszarze **usług**, wybierz pozycję **pliki**.
5. Wybierz udział plików.
6. Kliknij przycisk **przekazać** otworzyć **przekazać pliki** strony.
7. Kliknij ikonę do przeglądania lokalnego systemu plików dla pliku do przekazania.   
8. Kliknij przycisk **przekazać** można przekazać pliku do udziału plików.

## <a name="download-files"></a>Pobieranie plików
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W menu po lewej stronie kliknij **kont magazynu**.
3. Wybierz konto magazynu.
4. W **omówienie** w obszarze **usług**, wybierz pozycję **pliki**.
5. Wybierz udział plików.
6. Kliknij prawym przyciskiem myszy plik i wybierz polecenie **Pobierz** można pobrać go na komputerze lokalnym.
   

## <a name="next-steps"></a>Następne kroki

Można również tworzyć i Zarządzanie udziałami plików przy użyciu programu PowerShell. Aby uzyskać więcej informacji, zobacz [Rozpoczynanie pracy z magazynem plików Azure w systemie Windows](../../storage/files/storage-dotnet-how-to-use-files.md).
