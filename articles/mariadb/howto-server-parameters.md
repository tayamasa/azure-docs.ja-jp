---
title: Azure Database for MariaDB のサーバー パラメーターの構成方法
description: この記事では、Azure portal を使用して Azure Database for MariaDB で使用できる MariaDB サーバー パラメータを構成する方法について説明します。
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: c992783db3b63c73feb18c8c493ca1b43f9ea370
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2018
ms.locfileid: "46975209"
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mariadb-by-using-the-azure-portal"></a>Azure portal を使用して Azure Database for MariaDB のサーバー パラメーターを構成する方法

Azure Database for MariaDB では、いくつかのサーバー パラメーターの構成がサポートされています｡ この記事では、Azure Portal を使用してこれらのパラメーターを構成する方法について説明します。 すべてのサーバー パラメーターを調整できるわけではありません。

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Azure ポータルの [サーバー パラメーター] に移動する

1. Azure portal にサインインし、お使いの Azure Database for MariaDB サーバーを探します。
2. **[設定]** セクションの **[サーバー パラメーター]** をクリックして、Azure Database for MariaDB サーバーの [サーバー パラメーター] ページを開きます。
![Azure Portal の [サーバー パラメーター] ページ](./media/howto-server-parameters/azure-portal-server-parameters.png)
3. 調整が必要な設定を探します。 **[説明]** 列を確認して、目的と許可される値を理解します。
![列挙ドロップ ダウン](./media/howto-server-parameters/3-toggle_parameter.png)
4. **[保存]** をクリックして変更を保存します。
![変更の保存または破棄](./media/howto-server-parameters/4-save_parameters.png)
5. パラメーターの新しい値を保存した場合は、**[すべて既定値にリセット]** を選択していつでもすべてを既定値に戻すことができます。
![すべて既定値にリセット](./media/howto-server-parameters/5-reset_parameters.png)

## <a name="list-of-configurable-server-parameters"></a>構成可能なサーバー パラメーターの一覧

サポートされるサーバー パラメーターの一覧は、拡大を続けています。 Azure ポータルのサーバー パラメーターのタブを使用して定義を取得し、お使いのアプリケーションの要件に基づいてサーバー パラメーターを構成します。

## <a name="non-configurable-server-parameters"></a>構成不可能なサーバー パラメーター

InnoDB バッファー プールと最大接続数は構成できず、[価格レベル](concepts-pricing-tiers.md)に関連付けられています。

|**価格レベル**| **仮想コア数**|**InnoDB バッファー プール (MB)**| **最大接続数**|
|---|---|---|---|--|
|Basic| 1| 1024| 50|
|Basic| 2| 2560| 100|
|汎用| 2| 3584| 300|
|汎用| 4| 7680| 625|
|汎用| 8| 15360| 1250|
|汎用| 16| 31232| 2500|
|汎用| 32| 62976| 5000|
|メモリ最適化| 2| 7168| 600|
|メモリ最適化| 4| 15360| 1250|
|メモリ最適化| 8| 30720| 2500|
|メモリ最適化| 16| 62464| 5000|

<!-- 
|**Pricing Tier**| **Compute Generation**|**vCore(s)**|**InnoDB Buffer Pool (MB)**| **Max Connections**|
|---|---|---|---|--|
|Basic| Gen 4| 1| 1024| 50|
|Basic| Gen 4| 2| 2560| 100|
|Basic| Gen 5| 1| 1024| 50|
|Basic| Gen 5| 2| 2560| 100|
|General Purpose| Gen 4| 2| 3584| 300|
|General Purpose| Gen 4| 4| 7680| 625|
|General Purpose| Gen 4| 8| 15360| 1250|
|General Purpose| Gen 4| 16| 31232| 2500|
|General Purpose| Gen 4| 32| 62976| 5000|
|General Purpose| Gen 5| 2| 3584| 300|
|General Purpose| Gen 5| 4| 7680| 625|
|General Purpose| Gen 5| 8| 15360| 1250|
|General Purpose| Gen 5| 16| 31232| 2500|
|General Purpose| Gen 5| 32| 62976| 5000|
|Memory Optimized| Gen 5| 2| 7168| 600|
|Memory Optimized| Gen 5| 4| 15360| 1250|
|Memory Optimized| Gen 5| 8| 30720| 2500|
|Memory Optimized| Gen 5| 16| 62464| 5000|
-->

次に示す追加のサーバー パラメーターは、システム内で構成できません。

|**パラメーター**|**固定値**|
| :------------------------ | :-------- |
|Basic レベルの innodb_file_per_table|OFF|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|512 MB|

ここに記載されていないその他のサーバー パラメーターはすべて、[MariaDB](https://mariadb.com/kb/en/library/xtradbinnodb-server-system-variables/) に対する MariaDB の初期既定値に設定されます。

## <a name="working-with-the-time-zone-parameter"></a>タイム ゾーン パラメーターを使用する

### <a name="populating-the-time-zone-tables"></a>タイム ゾーン テーブルに入力する

サーバーのタイム ゾーン テーブルには、MySQL コマンド ラインや MySQL Workbench などのツールから `az_load_timezone` ストアド プロシージャを呼び出すことでデータを入力できます。

> [!NOTE]
> MySQL Workbench から `az_load_timezone` コマンドを実行するとき、場合によっては、`SET SQL_SAFE_UPDATES=0;` を利用し、最初にセーフ アップデート モードをオフにする必要があります。

```sql
CALL mysql.az_load_timezone();
```

利用可能なタイム ゾーン値を表示するには、次のコマンドを実行します。

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>グローバル レベルのタイム ゾーンを設定する

Azure Portal の **[サーバー パラメーター]** ページからグローバル レベルのタイム ゾーンを設定できます。 下の画像では、グローバル タイム ゾーンが値 "US/Pacific" に設定されています。

![タイム ゾーン パラメーターを設定する](./media/howto-server-parameters/timezone.png)

### <a name="setting-the-session-level-time-zone"></a>セッション レベルのタイム ゾーンを設定する

セッション レベルのタイム ゾーンは、MySQL コマンド ラインや MySQL Workbench などのツールから `SET time_zone` コマンドを実行することで設定できます。 下の例では、タイム ゾーンが **US/Pacific** タイム ゾーンに設定されます。

```sql
SET time_zone = 'US/Pacific';
```

[日付と時刻関数](https://mariadb.com/kb/en/library/convert_tz/)については MariaDB ドキュメントを参照してください。

<!--
## Next steps

- [Connection libraries for Azure Database for MariaDB](concepts-connection-libraries.md).
-->