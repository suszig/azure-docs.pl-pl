---
title: "Ogólny Łącznik usług SQL krok po kroku | Dokumentacja firmy Microsoft"
description: "W tym artykule jest Instruktaż prosty system HR krok po kroku przy użyciu ogólny łącznik SQL."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 28c1cc60-24fd-4d0d-a36d-b4aba6de86e7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 3fdc1b405b95180d031aa4ad45b406f7fc149d8f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="generic-sql-connector-step-by-step"></a>Instrukcja krok po kroku dotycząca ogólnego łącznika SQL
Ten temat jest przewodnik krok po kroku. Tworzy HR proste przykładową bazę danych i używać go do importowania niektórym użytkownikom i ich członkostwa w grupie.

## <a name="prepare-the-sample-database"></a>Przygotowanie przykładowej bazy danych
Na serwerze z uruchomionym programem SQL Server, uruchom skrypt SQL znaleziono [dodatek a.](#appendix-a). Ten skrypt tworzy przykładowa baza danych o nazwie GSQLDEMO. Model obiektów dla bazy danych utworzonej wygląda tego obrazu:  
![Model obiektu](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/objectmodel.png)

Również utworzyć użytkownika, którego chcesz używać do łączenia z bazą danych. W tym przewodniku użytkownik jest nazywany FABRIKAM\SQLUser i znajduje się w domenie.

## <a name="create-the-odbc-connection-file"></a>Tworzenie pliku połączenia ODBC
Ogólny łącznik SQL używa ODBC do łączenia się z serwerem zdalnym. Najpierw należy utworzyć plik z informacjami o połączenia ODBC.

1. Uruchom narzędzie zarządzania ODBC na serwerze:  
   ![ODBC](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc.png)
2. Wybierz kartę **pliku DSN**. Kliknij przycisk **Dodaj...** .  
   ![ODBC1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc1.png)
3. Działania sterownika out-of-box drobne, więc zaznacz go i kliknij **Dalej >**.  
   ![ODBC2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc2.png)
4. Określ plik nazwę, taką jak **GenericSQL**.  
   ![ODBC3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc3.png)
5. Kliknij przycisk **Zakończ**.  
   ![ODBC4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc4.png)
6. Czas, aby skonfigurować połączenie. Nadaj źródła danych czytelny opis, a następnie podaj nazwę serwera z uruchomionym programem SQL Server.  
   ![ODBC5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc5.png)
7. Wybierz sposób uwierzytelniania SQL. W takim przypadku stosujemy uwierzytelniania systemu Windows.  
   ![ODBC6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc6.png)
8. Podaj nazwę przykładowej bazy danych, **GSQLDEMO**.  
   ![ODBC7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc7.png)
9. Zachowaj domyślne wszystko na tym ekranie. Kliknij przycisk **Zakończ**.  
   ![ODBC8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc8.png)
10. Aby sprawdzić, wszystko działa zgodnie z oczekiwaniami, kliknij przycisk **Testuj źródło danych**.  
    ![ODBC9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc9.png)
11. Upewnij się, że test zakończy się pomyślnie.  
    ![ODBC10](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc10.png)
12. Powinno być teraz widoczne w pliku DSN ODBC pliku konfiguracji.  
    ![ODBC11](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc11.png)

Mamy teraz plik możemy muszą i może rozpocząć tworzenie łącznika.

## <a name="create-the-generic-sql-connector"></a>Tworzenie łącznika ogólnego SQL
1. W Interfejsie użytkownika Menedżera usługi synchronizacji, wybierz **łączniki** i **Utwórz**. Wybierz **ogólnego SQL (Microsoft)** i nadaj mu nazwę opisową.  
   ![Connector1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector1.png)
2. Znajdź plik DSN, który został utworzony w poprzedniej sekcji, a następnie przekaż go do serwera. Podaj poświadczenia do połączenia z bazą danych.  
   ![Connector2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector2.png)
3. W tym przewodniku, to aby ułatwić firmie Microsoft firma Microsoft i powiedzieć, że istnieją dwa typy obiektów, **użytkownika** i **grupy**.
   ![Connector3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector3.png)
4. W celu znalezienia atrybutów, chcemy łącznik, aby wykryć te atrybuty analizując samej tabeli. Ponieważ **użytkowników** jest słowem zastrzeżonym SQL, należy podać go w nawiasy kwadratowe [].  
   ![Connector4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector4.png)
5. Czas do definiowania atrybutu zakotwiczenia i atrybut nazwy domeny. Aby uzyskać **użytkowników**, używamy kombinacja nazwy użytkownika dwa atrybuty i identyfikator pracownika. Dla **grupy**, używamy, Nazwa_grupy (nie realistyczne w rzeczywistych, ale w ramach tego przewodnika działa).
   ![Connector5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector5.png)
6. Nie wszystkie typy atrybutów, można wykryć w bazie danych SQL. W szczególności nie odwołania typu atrybutu. Dla typu obiektu grupy należy zmienić OwnerID i MemberID ma dotyczyć odwołanie.  
   ![Connector6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector6.png)
7. Atrybuty, które jako typ obiektu wymagają atrybutów odwołania w poprzednim kroku wybrano te wartości są odwołania do. W tym przypadku typ obiektu użytkownika.  
   ![Connector7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector7.png)
8. Na stronie parametry globalne wybierz **znaku wodnego** jako strategii delta. Także wpisać w formacie daty/godziny **RRRR MM-dd gg**.
   ![Connector8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector8.png)
9. Na **Konfiguruj partycje i hierarchie** wybierz oba typy obiektów.
   ![Connector9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector9.png)
10. Na **wybierz typy obiektów** i **wybierz atrybuty**, zaznacz typy obiektów i wszystkich atrybutów. Na **Konfiguruj zakotwiczenia** kliknij przycisk **Zakończ**.

## <a name="create-run-profiles"></a>Tworzenie profilów uruchamiania
1. W Interfejsie użytkownika Menedżera usługi synchronizacji, wybierz **łączniki**, i **Konfigurowanie profilów uruchamiania**. Kliknij przycisk **nowy profil**. Możemy zaczynać **pełny Import**.  
   ![Runprofile1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile1.png)
2. Wybierz typ **pełny Import (tylko etap)**.  
   ![Runprofile2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile2.png)
3. Wybierz partycję **obiektu = użytkownik**.  
   ![Runprofile3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile3.png)
4. Wybierz **tabeli** i typ **[użytkownicy]**. Przewiń w dół do sekcji Typ obiektu wielowartościowe, a następnie wprowadź dane, tak jak na poniższej ilustracji. Wybierz **Zakończ** pominąć krok.  
   ![Runprofile4a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4a.png)  
   ![Runprofile4b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4b.png)  
5. Wybierz **nowy krok**. Teraz, wybierz opcję **obiektu = grupy**. Na ostatniej stronie należy użyć konfiguracji, tak jak na poniższej ilustracji. Kliknij przycisk **Zakończ**.  
   ![Runprofile5a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5a.png)  
   ![Runprofile5b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5b.png)  
6. Opcjonalnie: Jeśli chcesz, można skonfigurować dodatkowe profile uruchamiania. W ramach tego przewodnika jest używany tylko pełny Import.
7. Kliknij przycisk **OK** aby zakończyć zmianę profile uruchamiania.

## <a name="add-some-test-data-and-test-the-import"></a>Dodaj dane testowe i testowania importu
Wprowadź dane testowe w przykładowej bazie danych. Gdy wszystko będzie gotowe, wybierz **Uruchom** i **pełny import**.

Oto użytkownika z dwóch numery telefonów i grupy z niektóre elementy członkowskie.  
![cs1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs1.png)  
![CS2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs2.png)  

## <a name="appendix-a"></a>Dodatek A
**Skrypt SQL w celu utworzenia przykładowej bazy danych**

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
