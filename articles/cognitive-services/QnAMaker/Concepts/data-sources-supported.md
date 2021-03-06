---
title: サポートされているデータ ソース - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker は、Web ページ、PDF ファイル、または MS Word doc ファイルとして保存された FAQ、製品マニュアル、ガイドライン、サポート ドキュメント、ポリシーなどの半構造化コンテンツから質問と回答のペアを自動的に抽出します。 また、構造化 QnA コンテンツ ファイルのコンテンツをナレッジ ベースに追加することもできます。
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/25/2018
ms.author: tulasim
ms.openlocfilehash: 29e894b0666b37d32f36b016603fda408e9d2746
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2018
ms.locfileid: "47161055"
---
# <a name="data-sources-for-qna-maker-content"></a>QnA Maker コンテンツのデータ ソース

QnA Maker は、Web ページ、PDF ファイル、または MS Word doc ファイルとして保存された FAQ、製品マニュアル、ガイドライン、サポート ドキュメント、ポリシーなどの半構造化コンテンツから質問と回答のペアを自動的に抽出します。 また、構造化 QnA コンテンツ ファイルのコンテンツをナレッジ ベースに追加することもできます。 

次の表は、QnA Maker でサポートされているコンテンツの種類とファイル形式をまとめたものです。

|ソースの種類|コンテンツの種類| 例|
|--|--|--|
|URL|FAQ (基本、セクション付き、またはトピックのホーム ページ付き)|[基本的な FAQ](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs)、[リンク付き FAQ](https://www.microsoft.com/software-download/faq)、[トピックのホーム ページ付き FAQ](https://support.microsoft.com/products/windows?os=windows-10)|
|PDF / DOC|FAQ、製品マニュアル、パンフレット、論文、チラシ、ポリシー、サポート ガイド、構造化 QnA など|[Structured QnA.doc](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Bot%20Service%20Sample%20FAQ.docx)、[Sample Product Manual.pdf](http://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf)、[Sample semi-structured.doc](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx)、[Sample white paper.pdf](https://azure.microsoft.com/mediahandler/files/resourcefiles/azure-stack-wortmann-bring-the-power-of-the-public-cloud-into-your-data-center/Azure_Stack_Wortmann_Bring_the_Power_of_the_Public_Cloud_into_Your_Data_Center.pdf)|
|Excel|構造化 QnA ファイル (RTF、HTML サポートを含む)|[Sample QnA FAQ.xls](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/QnA%20Maker%20Sample%20FAQ.xlsx)|
|TXT/TSV|構造化 QnA ファイル|[Sample chit-chat.tsv](https://raw.githubusercontent.com/Microsoft/BotBuilder-PersonalityChat/master/CSharp/Datasets/Queries_Responses_Friendly_QnAMaker.tsv)|

## <a name="faq-urls"></a>FAQ URL

QnA Maker がサポートできる FAQ ページの形式は、基本的な FAQ ページ、リンク付き FAQ ページ、トピックのホーム ページ付き FAQ ページの 3 種類です。

### <a name="plain-faq-pages"></a>基本的な FAQ ページ

これは最も一般的なタイプの FAQ ページです。質問と回答が同じページにあり、質問のすぐ下に回答が記載されます。 

次に示すのは、基本的な FAQ ページの例です。

![基本的な FAQ ページ](../media/qnamaker-concepts-datasources/plain-faq.png) 

 
### <a name="faq-pages-with-links"></a>リンク付き FAQ ページ 

このタイプの FAQ ページでは、質問が 1 か所に集約され、同じページ上の別のセクションまたは別のページのどちらかにある回答にリンクされます。

次に示すのは、同じページ上のセクションへのリンクがある FAQ ページの例です。

 ![セクション リンク FAQ ページ](../media/qnamaker-concepts-datasources/sectionlink-faq.png) 


### <a name="faq-pages-with-a-topics-homepage"></a>トピックのホーム ページ付き FAQ ページ

このタイプの FAQ にはトピックのホーム ページがあり、それぞれのトピックは別のページにある関連 QnA へのリンクです。 ここでは、QnA Maker は、リンクされているすべてのページをクロールして、対応する質問と回答を抽出します。

次に示すのは、別のページ内の FAQ セクションへのリンクがトピックのホーム ページにある FAQ ページの例です。 

 ![ディープ リンク FAQ ページ](../media/qnamaker-concepts-datasources/topics-faq.png) 


## <a name="pdf-doc-files"></a>PDF/ DOC ファイル

QnA Maker では、PDF または DOC ファイルの半構造化コンテンツを処理し、QnA に変換することができます。 正しく抽出できる適切なファイルは、コンテンツが何らかの構造化形式で整理され、適切に定義されたセクションで表現されているファイルです。 セクションはさらにサブセクションまたはサブトピックに分けることができます。 抽出が最もうまくいくのは、階層的な見出しのある明確な構造を持ったドキュメントです。

QnA Maker は、フォント サイズ、フォント スタイル、ナンバリング、色などの視覚的な手がかりに基づいて、ファイル内のセクション、サブセクション、関係を識別します。半構造化 PDF または DOC ファイルには、マニュアル、FAQ、ガイドライン、ポリシー、パンフレット、チラシ、その他多くの種類のファイルがあります。 次に示すのは、これらのファイルの種類の例です。

### <a name="product-manuals"></a>製品マニュアル

一般にマニュアルは、製品に付属するガイダンス資料です。 ユーザーにとっては、製品のセットアップ、使用、保守、トラブルシューティングにあたっての貴重な情報源となります。 QnA Maker は、マニュアルを処理する際、見出しと小見出しを質問として抽出し、質問に続くコンテンツを回答として抽出します。 [こちら](http://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf)の例を参照してください。

次に示すのは、索引ページと階層的なコンテンツを持ったマニュアルの例です

 ![製品マニュアルの例](../media/qnamaker-concepts-datasources/product-manual.png) 

> [!NOTE]
> 抽出の対象として最も適しているのは、目次やインデックス ページがあり、階層化された見出しから成る明確な構造を持ったマニュアルです。

### <a name="brochures-guidelines-papers-and-other-files"></a>パンフレット、ガイドライン、論文、およびその他のファイル

明確な構造とレイアウトがあれば、他の多くの種類のドキュメントも処理して QA ペアを生成することができます。 これには、パンフレット、ガイドライン、レポート、ホワイト ペーパー、科学論文、ポリシー、書籍などが含まれます。[こちら](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx)の例を参照してください。

次に示すのは、索引のない半構造化ドキュメントの例です。

 ![Azure BLOB ストレージ半構造化ドキュメント](../media/qnamaker-concepts-datasources/semi-structured-doc.png) 

### <a name="structured-qna-document"></a>構造化 QnA ドキュメント

次に示すように、質問と回答の構造化 DOC ファイルの形式は、質問と回答を 1 行おきに並べる、つまり、1 つの質問を 1 行に記述し、次の行にその回答を記述する形式です。 

```
Question1

Answer1

Question2

Answer2
```

次に示すのは、構造化 QnA Word 文書の例です。

 ![構造化 QnA ドキュメント](../media/qnamaker-concepts-datasources/structured-qna-doc.png) 

## <a name="structured-txt-tsv-and-xls-files"></a>構造化 *TXT*、*TSV*、および *XLS* ファイル

構造化 *.txt*、*.tsv*、または *.xls* ファイル形式の QnA を QnA Maker にアップロードして、ナレッジ ベースを作成または拡張することもできます。  これらはプレーン テキストでも、RTF または HTML のコンテンツが含まれていても構いません。 

| 質問  | Answer  | Metadata                |
|-----------|---------|-------------------------|
| 質問 1 | 回答 1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| 質問 2 | 回答 2 |      `Key:Value`           |

これより後の列は、ソース ファイルに含まれていても無視されます。

次に示すのは、HTML コンテンツを含む構造化 QnA *.xls* ファイルの例です。

 ![構造化 QnA Excel](../media/qnamaker-concepts-datasources/structured-qna-xls.png)

## <a name="structured-data-format-through-import"></a>インポートでの構造化データ形式

ナレッジ ベースをインポートすると、既にあるナレッジ ベースの内容が置き換えられます。 インポートでは、データ ソース情報を含んだ .tsv 形式の構造化ファイルが必要となります。 この情報によって、QnA Maker は質問とその回答のペアをグループ化し、その帰属先となるデータ ソースを特定することができます。

| 質問  | Answer  | ソース| Metadata                |
|-----------|---------|----|---------------------|
| 質問 1 | 回答 1 | URL 1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| 質問 2 | 回答 2 | 編集|    `Key:Value`       |

## <a name="editorially-add-to-knowledge-base"></a>編集という形でナレッジ ベースに追加する

ナレッジ ベースの内容とする既存のコンテンツがない場合は、QnA Maker のナレッジ ベースに編集という形で QnA を追加することができます。 ナレッジ ベースの更新方法については、[こちら](../How-To/edit-knowledge-base.md)を参照してください。

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [QnA Maker サービスの設定](../How-To/set-up-qnamaker-service-azure.md)

## <a name="see-also"></a>関連項目 

[QnA Maker の概要](../Overview/overview.md)
