---
title: Azure Machine Learning データ準備の実行環境とデータ環境のサポートされている組み合わせ | Microsoft Docs
description: このドキュメントでは、Azure Machine Learning データ準備用の各種ランタイムおよびデータ ソースのサポートされている組み合わせの全一覧を示します
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 9168ac1d26432ca3eee5a59b63aa0cec3ae72856
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989058"
---
# <a name="supported-matrix-for-this-release"></a>このリリースでサポートされているマトリックス 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


コードで Azure Machine Learning データ ソースを使用して、または Azure Machine Learning データ準備でデータを読み込み、Pandas または Spark データフレームのどちらかを取得するとき、実験的コンピューティング環境とデータの場所の組み合わせとして、次のものがサポートされています。

|     |ローカル ファイル  |Azure BLOB ストレージ  |SQL Server データベース***  |
|---------|---------|---------|---------|---------|
|ローカルの Python    |     サポートされています    |サポートされていません         | サポートされていません        |         |
|Docker (Linux VM) Python     |プロジェクト ファイルのみでサポート*         | サポートされていません        |        サポートされていません |         |
|Docker (Linux VM) PySpark     |プロジェクト ファイルのみでサポート*     |サポートされています         | サポートされています \*\*        |         |
|Azure Data Science Virtual Machine Python     |プロジェクト ファイルのみでサポート*         |サポートされていません         |サポートされていません         |         |
|Azure Data Science Virtual Machine PySPark     | プロジェクト ファイルのみでサポート*        |サポートされていません         |サポートされていません         |         |
|Azure HDInsight PySpark     | サポートされていません        |サポートされています         |サポートされています \*\*         |         |
|Azure HDInsight Python     | サポートされていません        | サポートされていません        | サポートされていません        |         |

Azure Data Lake Store は現在、どのコンピューティング ターゲット向けにもサポートされていません。

*ローカル ファイル パスが使用されるとき、プロジェクト内部のファイルはコンピューティング環境にコピーされた後、そこで読み取られます。 プロジェクトの外にあるファイルはコピーされず、コンピューティング環境ではパスが解決されなくなります。 ローカルで実行するときにコードがローカル ファイルを使用できるように、Data Source Substitution の使用を検討してください。 その後、別の実行構成用に Azure Storage BLOB に切り替えます。 データ ソースでサンプリング サポートを使用し、特定の実行構成においてのみ大規模データに対する実行を管理することもできます。

\*\* Maven JDBC SQL Server ドライバー 6.2.1 を使用します。 このパッケージ (または互換性のあるパッケージ) が、コンピューティング環境用の spark_dependencies.yml ファイルに含まれていることを確認する必要があります。

***コンピューティング環境からデータベースと通信できるという条件のもと、Azure SQL Database または SQL Server をサポートします。 
