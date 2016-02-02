<properties
   pageTitle="SQL Data Warehouse Transact-SQL 言語要素 | Microsoft Azure"
   description="SQL Data Warehouse に使用される Transact-SQL 言語要素のリファレンス コンテンツへのリンクのリスト。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/22/2015"
   ms.author="barbkess"/>


# 言語要素

## コア要素

- [構文表記規則](https://msdn.microsoft.com/library/ms177563.aspx)
- [オブジェクトの名前付け規則](https://msdn.microsoft.com/library/ms175874.aspx)
- [予約済みキーワード](https://msdn.microsoft.com/library/ms189822.aspx)
- [照合順序](https://msdn.microsoft.com/library/ff848763.aspx)
- [コメント](https://msdn.microsoft.com/library/ms181627.aspx)
- [定数](https://msdn.microsoft.com/library/ms179899.aspx)
- [データ型](https://msdn.microsoft.com/library/ms187752.aspx)
- [実行します。](https://msdn.microsoft.com/library/ms188332.aspx)
- [式](https://msdn.microsoft.com/library/ms190286.aspx)
- [強制終了します。](https://msdn.microsoft.com/library/ms173730.aspx)
- [IDENTITY プロパティの代替手段](https://msdn.microsoft.com/library/ms186775.aspx)
- [印刷](https://msdn.microsoft.com/library/ms176047.aspx)
- [使用します。](https://msdn.microsoft.com/library/ms188366.aspx)

## バッチ、フロー制御、および変数

- [開始します.終了](https://msdn.microsoft.com/library/ms190487.aspx)
- [中断](https://msdn.microsoft.com/library/ms181271.aspx)
- [@Local_variable を宣言します。](https://msdn.microsoft.com/library/ms188927.aspx)
- [場合に.その他](https://msdn.microsoft.com/library/ms182717.aspx)
- [RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx)
- [SET@local_variable](https://msdn.microsoft.com/library/ms189484.aspx)
- [スローします。](https://msdn.microsoft.com/library/ee677615.aspx)
- [しようとしてください.CATCH](https://msdn.microsoft.com/library/ms175976.aspx)
- [WHILE](https://msdn.microsoft.com/library/ms178642.aspx)

## 演算子

- [+ (加算)](https://msdn.microsoft.com/library/ms178565.aspx)
- [+ (文字列連結)](https://msdn.microsoft.com/library/ms177561.aspx)
- [-(負号)](https://msdn.microsoft.com/library/ms189480.aspx)
- [-(減算)](https://msdn.microsoft.com/library/ms189518.aspx)
- [* (乗算)](https://msdn.microsoft.com/library/ms176019.aspx)
- [/(除算)](https://msdn.microsoft.com/library/ms175009.aspx)
- [剰余](https://msdn.microsoft.com/library/ms190279.aspx)

## 一致するワイルドカード文字

- [= (等しい)](https://msdn.microsoft.com/library/ms175118.aspx)
- [> (より大きい)](https://msdn.microsoft.com/library/ms178590.aspx)
- [< (より小さい)](https://msdn.microsoft.com/library/ms179873.aspx)
- [> = (以上に)](https://msdn.microsoft.com/library/ms181567.aspx)
- [< = (以下を)](https://msdn.microsoft.com/library/ms174978.aspx)
- [<> (等しくない)](https://msdn.microsoft.com/library/ms176020.aspx)
- [! = (等しくない)](https://msdn.microsoft.com/library/vstudio/bb399278.aspx)
- [と](https://msdn.microsoft.com/library/ms188372.aspx)
- [間](https://msdn.microsoft.com/library/ms187922.aspx)
- [存在します。](https://msdn.microsoft.com/library/ms188336.aspx)
- [IN](https://msdn.microsoft.com/library/ms177682.aspx)
- [は、[NOT] NULL](https://msdn.microsoft.com/library/ms188795.aspx)
- [ように](https://msdn.microsoft.com/library/ms179859.aspx)
- [NOT](https://msdn.microsoft.com/library/ms189455.aspx)
- [OR](https://msdn.microsoft.com/library/ms188361.aspx)

### ビット演算子

- [& (ビット演算 AND)](https://msdn.microsoft.com/library/ms174965.aspx)
- [|(ビット演算 OR)](https://msdn.microsoft.com/library/ms186714.aspx)
- [^ (ビットごとの排他的 OR)](https://msdn.microsoft.com/library/ms190277.aspx)
- [~ (ビット演算子 NOT)](https://msdn.microsoft.com/library/ms173468.aspx)
- [^ = (ビットごとの排他的 OR 代入)](https://msdn.microsoft.com/library/cc627413.aspx)
- [| = (ビットごとの OR 代入)](https://msdn.microsoft.com/library/cc627409.aspx)
- [& = (ビットごとの AND 代入)](https://msdn.microsoft.com/library/cc627427.aspx)

## 関数

- [@@DATEFIRST](https://msdn.microsoft.com/library/ms187766.aspx)
- [@@ERROR](https://msdn.microsoft.com/library/ms188790.aspx)
- [@@LANGUAGE](https://msdn.microsoft.com/library/ms177557.aspx)
- [@@SPID](https://msdn.microsoft.com/library/ms189535.aspx)
- [@@TRANCOUNT](https://msdn.microsoft.com/library/ms187967.aspx)
- [@@VERSION](https://msdn.microsoft.com/library/ms177512.aspx)
- [ABS](https://msdn.microsoft.com/library/ms189800.aspx)
- [ACOS](https://msdn.microsoft.com/library/ms178627.aspx)
- [ASCII](https://msdn.microsoft.com/library/ms177545.aspx)
- [ASIN](https://msdn.microsoft.com/library/ms181581.aspx)
- [ATAN](https://msdn.microsoft.com/library/ms181746.aspx)
- [ATN2](https://msdn.microsoft.com/library/ms173854.aspx)
- [場合](https://msdn.microsoft.com/library/ms181765.aspx)
- [CAST および CONVERT](https://msdn.microsoft.com/library/ms187928.aspx)
- [上限](https://msdn.microsoft.com/library/ms189818.aspx)
- [CHAR 型](https://msdn.microsoft.com/library/ms187323.aspx)
- [CHARINDEX 関数](https://msdn.microsoft.com/library/ms186323.aspx)
- [結合](https://msdn.microsoft.com/library/ms190349.aspx)
- [COL_NAME](https://msdn.microsoft.com/library/ms174974.aspx)
- [COLLATIONPROPERTY](https://msdn.microsoft.com/library/ms190305.aspx)
- [CONCAT](https://msdn.microsoft.com/library/hh231515.aspx)
- [COS](https://msdn.microsoft.com/library/ms188919.aspx)
- [COT](https://msdn.microsoft.com/library/ms188921.aspx)
- [カウント](https://msdn.microsoft.com/library/ms175997.aspx)
- [COUNT_BIG](https://msdn.microsoft.com/library/ms190317.aspx)
- [CURRENT_TIMESTAMP](https://msdn.microsoft.com/library/ms188751.aspx)
- [現在のユーザー](https://msdn.microsoft.com/library/ms176050.aspx)
- [DATABASEPROPERTYEX](https://msdn.microsoft.com/library/ms186823.aspx)
- [DATALENGTH](https://msdn.microsoft.com/library/ms173486.aspx)
- [DATEADD 関数](https://msdn.microsoft.com/library/ms186819.aspx)
- [DATEDIFF 関数](https://msdn.microsoft.com/library/ms189794.aspx)
- [DATEFROMPARTS](https://msdn.microsoft.com/library/hh213228.aspx)
- [DATENAME](https://msdn.microsoft.com/library/ms174395.aspx)
- [日付構成要素](https://msdn.microsoft.com/library/ms174420.aspx)
- [DATETIME2FROmPARTS](https://msdn.microsoft.com/library/hh213312.aspx)
- [DATETIMEFROMPARTS](https://msdn.microsoft.com/library/hh213233.aspx)
- [DATETIMEOFFSETFROMPARTS](https://msdn.microsoft.com/library/hh231077.aspx)
- [1 日](https://msdn.microsoft.com/library/ms176052.aspx)
- [DB_ID 関数](https://msdn.microsoft.com/library/ms186274.aspx)
- [DB_NAME](https://msdn.microsoft.com/library/ms189753.aspx)
- [度](https://msdn.microsoft.com/library/ms178566.aspx)
- [DENSE_RANK](https://msdn.microsoft.com/library/ms173825.aspx)
- [相違点](https://msdn.microsoft.com/library/ms188753.aspx)
- [EOMONTH](https://msdn.microsoft.com/library/hh213020.aspx)
- [ERROR_MESSAGE](https://msdn.microsoft.com/library/ms190358.aspx)
- [ERROR_NUMBER](https://msdn.microsoft.com/library/ms175069.aspx)
- [ERROR_PROCEDURE](https://msdn.microsoft.com/library/ms188398.aspx)
- [ERROR_SEVERITY](https://msdn.microsoft.com/library/ms178567.aspx)
- [ERROR_STATE](https://msdn.microsoft.com/library/ms180031.aspx)
- [EXP 関数](https://msdn.microsoft.com/library/ms179857.aspx)
- [フロア](https://msdn.microsoft.com/library/ms178531.aspx)
- [GETDATE](https://msdn.microsoft.com/library/ms188383.aspx)
- [GETUTCDATE](https://msdn.microsoft.com/library/ms178635.aspx)
- [HAS_DBACCESS](https://msdn.microsoft.com/library/ms187718.aspx)
- [HASHBYTES](https://msdn.microsoft.com/library/ms174415.aspx)
- [INDEXPROPERTY](https://msdn.microsoft.com/library/ms187729.aspx)
- [ISDATE 関数](https://msdn.microsoft.com/library/ms187347.aspx)
- [ISNULL](https://msdn.microsoft.com/library/ms184325.aspx)
- [ISNUMERIC](https://msdn.microsoft.com/library/ms186272.aspx)
- [遅延](https://msdn.microsoft.com/library/hh231256.aspx)
- [潜在顧客](https://msdn.microsoft.com/library/hh213125.aspx)
- [左](https://msdn.microsoft.com/library/ms177601.aspx)
- [LEN 関数](https://msdn.microsoft.com/library/ms190329.aspx)
- [ログ](https://msdn.microsoft.com/library/ms190319.aspx)
- [LOG10](https://msdn.microsoft.com/library/ms175121.aspx)
- [低い](https://msdn.microsoft.com/library/ms174400.aspx)
- [LTRIM](https://msdn.microsoft.com/library/ms177827.aspx)
- [最大値](https://msdn.microsoft.com/library/ms187751.aspx)
- [最小値](https://msdn.microsoft.com/library/ms179916.aspx)
- [1 か月](https://msdn.microsoft.com/library/ms187813.aspx)
- [NCHAR](https://msdn.microsoft.com/library/ms182673.aspx)
- [NTILE](https://msdn.microsoft.com/library/ms175126.aspx)
- [NULLIF](https://msdn.microsoft.com/library/ms177562.aspx)
- [OBJECT_ID](https://msdn.microsoft.com/library/ms190328.aspx)
- [OBJECT_NAME](https://msdn.microsoft.com/library/ms186301.aspx)
- [OBJECTPROPERTY](https://msdn.microsoft.com/library/ms176105.aspx)
- [OIBJECTPROPERTYEX](https://msdn.microsoft.com/library/ms188390.aspx)
- [ODBCS スカラ関数](https://msdn.microsoft.com/library/bb630290.aspx)
- [OVER 句](https://msdn.microsoft.com/library/ms189461.aspx)
- [PARSENAME](https://msdn.microsoft.com/library/ms188006.aspx)
- [PATINDEX](https://msdn.microsoft.com/library/ms188395.aspx)
- [PERCENTILE_CONT](https://msdn.microsoft.com/library/hh231473.aspx)
- [PERCENTILE_DISC](https://msdn.microsoft.com/library/hh231327.aspx)
- [PI](https://msdn.microsoft.com/library/ms189512.aspx)
- [電源](https://msdn.microsoft.com/library/ms174276.aspx)
- [QUOTENAME](https://msdn.microsoft.com/library/ms176114.aspx)
- [ラジアン](https://msdn.microsoft.com/library/ms189742.aspx)
- [ランク](https://msdn.microsoft.com/library/ms176102.aspx)
- [置換](https://msdn.microsoft.com/library/ms186862.aspx)
- [レプリケート](https://msdn.microsoft.com/library/ms174383.aspx)
- [反転](https://msdn.microsoft.com/library/ms180040.aspx)
- [右](https://msdn.microsoft.com/library/ms177532.aspx)
- [丸める](https://msdn.microsoft.com/library/ms175003.aspx)
- [ROW_NUMBER](https://msdn.microsoft.com/library/ms186734.aspx)
- [RTRIM](https://msdn.microsoft.com/library/ms178660.aspx)
- [SCHEMA_ID](https://msdn.microsoft.com/library/ms188797.aspx)
- [SCHEMA_NAME](https://msdn.microsoft.com/library/ms175068.aspx)
- [SERVERPROPERTY](https://msdn.microsoft.com/library/ms174396.aspx)
- [SESSION_USER](https://msdn.microsoft.com/library/ms177587.aspx)
- [サインイン](https://msdn.microsoft.com/library/ms188420.aspx)
- [SIN 関数](https://msdn.microsoft.com/library/ms188377.aspx)
- [SMALLDATETIMEFROMPARTS](https://msdn.microsoft.com/library/hh213396.aspx)
- [SOUNDEX](https://msdn.microsoft.com/library/ms187384.aspx)
- [領域](https://msdn.microsoft.com/library/ms187950.aspx)
- [SQL_VARIANT_PROPERTY](https://msdn.microsoft.com/library/ms178550.aspx)
- [SQRT](https://msdn.microsoft.com/library/ms176108.aspx)
- [正方形](https://msdn.microsoft.com/library/ms173569.aspx)
- [STATS_DATE](https://msdn.microsoft.com/library/ms190330.aspx)
- [STDEV](https://msdn.microsoft.com/library/ms190474.aspx)
- [STDEVP](https://msdn.microsoft.com/library/ms176080.aspx)
- [STR](https://msdn.microsoft.com/library/ms189527.aspx)
- [内容](https://msdn.microsoft.com/library/ms188043.aspx)
- [部分文字列](https://msdn.microsoft.com/library/ms187748.aspx)
- [合計](https://msdn.microsoft.com/library/ms187810.aspx)
- [SUSER_SNAME](https://msdn.microsoft.com/library/ms174427.aspx)
- [SWITCHOFFSET](https://msdn.microsoft.com/library/bb677244.aspx)
- [SYSDATETIME](https://msdn.microsoft.com/library/bb630353.aspx)
- [SYSDATETIMEOFFSET](https://msdn.microsoft.com/library/bb677334.aspx)
- [SYSTEM_USER](https://msdn.microsoft.com/library/ms179930.aspx)
- [SYSUTCDATETIME](https://msdn.microsoft.com/library/bb630387.aspx)
- [TAN 関数](https://msdn.microsoft.com/library/ms190338.aspx)
- [TERTIARY_WEIGHTS](https://msdn.microsoft.com/library/ms186881.aspx)
- [TIMEFROMPARTS](https://msdn.microsoft.com/library/hh213398.aspx)
- [TODATETIMEOFFSET](https://msdn.microsoft.com/library/bb630335.aspx)
- [TYPE_ID](https://msdn.microsoft.com/library/ms181628.aspx)
- [TYPE_NAME](https://msdn.microsoft.com/library/ms189750.aspx)
- [TYPEPROPERTY](https://msdn.microsoft.com/library/ms188419.aspx)
- [UNICODE](https://msdn.microsoft.com/library/ms180059.aspx)
- [上限](https://msdn.microsoft.com/library/ms180055.aspx)
- [ユーザー](https://msdn.microsoft.com/library/ms186738.aspx)
- [ユーザー名](https://msdn.microsoft.com/library/ms188014.aspx)
- [VAR](https://msdn.microsoft.com/library/ms186290.aspx)
- [VARP 関数](https://msdn.microsoft.com/library/ms188735.aspx)
- [年](https://msdn.microsoft.com/library/ms186313.aspx)
- [XACT_STATE](https://msdn.microsoft.com/library/ms189797.aspx)

## トランザクション

- トランザクション

## 診断セッション

- CREATE DIAGNOSTICS SESSION

## プロシージャ

- [sp_addrolemember](https://msdn.microsoft.com/library/ms187750.aspx)
- [sp_columns](https://msdn.microsoft.com/library/ms176077.aspx)
- [sp_configure](https://msdn.microsoft.com/library/ms188787.aspx)
- sp_datatype_info_90
- [sp_droprolemember](https://msdn.microsoft.com/library/ms188369.aspx)
- [sp_execute](https://msdn.microsoft.com/library/ff848746.aspx)
- [sp_executesql](https://msdn.microsoft.com/library/ms188001.aspx)
- [sp_fkeys](https://msdn.microsoft.com/library/ms175090.aspx)
- sp_pdw_add_network_credentials
- sp_pdw_database_encryption
- sp_pdw_database_encryption_regenerate_system_keys
- sp_pdw_log_user_data_masking
- sp_pdw_remove_network_credentials
- [sp_pkeys](https://msdn.microsoft.com/library/ms189813.aspx)
- [sp_prepare](https://msdn.microsoft.com/library/ff848808.aspx)
- sp_special_columns_100
- [sp_sproc_columns](https://msdn.microsoft.com/library/ms182705.aspx)
- [sp_statistics](https://msdn.microsoft.com/library/ms173842.aspx)
- [sp_tables](https://msdn.microsoft.com/library/ms186250.aspx)
- [sp_unprepare](https://msdn.microsoft.com/library/ff848735.aspx)



## SET ステートメント

- [[SET ANSI_DEFAULTS]](https://msdn.microsoft.com/library/ms188340.aspx)
- [セット ANSI_NULL_DFLT_OFF](https://msdn.microsoft.com/library/ms187356.aspx)
- SET ANSI_NULL_DFLOT_ON
- [[SET ANSI_NULLS]](https://msdn.microsoft.com/library/ms188048.aspx)
- [[SET ANSI_PADDING]](https://msdn.microsoft.com/library/ms187403.aspx)
- [[SET ANSI_WARNINGS]](https://msdn.microsoft.com/library/ms190368.aspx)
- [[SET ARITHABORT]](https://msdn.microsoft.com/library/ms190306.aspx)
- [SET ARITHIGNORE](https://msdn.microsoft.com/library/ms184341.aspx)
- [セット CONCAT_NULL_YIELDS_NULL](https://msdn.microsoft.com/library/ms176056.aspx)
- [SET DATEFIRST](https://msdn.microsoft.com/library/ms181598.aspx)
- [SET DATEFORMAT](https://msdn.microsoft.com/library/ms189491.aspx)
- [SET FMTONLY](https://msdn.microsoft.com/library/ms173839.aspx)
- [セット IMPLICIT_TRANSACITONS](https://msdn.microsoft.com/library/ms187807.aspx)
- [[SET LOCK_TIMEOUT]](https://msdn.microsoft.com/library/ms189470.aspx)
- [セット NUMBERIC_ROUNDABORT](https://msdn.microsoft.com/library/ms188791.aspx)
- [[SET QUOTED_IDENTIFIER]](https://msdn.microsoft.com/library/ms174393.aspx)
- [[SET ROWCOUNT]](https://msdn.microsoft.com/library/ms188774.aspx)
- [[SET TEXTSIZE]](https://msdn.microsoft.com/library/ms186238.aspx)
- [SET TRANSACTION ISOLATION LEVEL](https://msdn.microsoft.com/library/ms173763.aspx)
- [SET XACT_ABORT](https://msdn.microsoft.com/library/ms188792.aspx)


## 次のステップ

参照の詳細については、次を参照してください。 [SQL Data Warehouse の概要 [] を参照する][]します。








[sql data warehouse development overview]: sql-data-warehouse-overview-reference.md 

