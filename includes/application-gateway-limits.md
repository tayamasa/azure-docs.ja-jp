| リソース | 既定の制限 | 注 |
| --- | --- | --- |
| Application Gateway |サブスクリプションあたり 50 | 最大 100 |
| フロントエンド IP の構成の数 |2 |パブリック 1、プライベート 1 |
| フロントエンド ポートの数 |20 | |
| バックエンド アドレス プールの数 |20 | |
| プールあたりのバックエンド サーバーの数 |100 | |
| HTTP リスナーの数 |20 | |
| HTTP の負荷分散規則 |200 |HTTP リスナーの数 * n。n = 10 (既定値) |
| バックエンドの HTTP 設定の数 |20 |バックエンド アドレス プールあたり 1 |
| ゲートウェイあたりのインスタンスの数 |10 | それ以上のインスタンス数については、サポート チケットを開いてください。 |
| SSL 証明書の数 |20 |HTTP リスナーあたり 1 |
| 認証証明書 |5 | 最大 10 |
| 要求のタイムアウト (最小) |1 秒 | |
| 要求のタイムアウト (最大) |24 時間 | |
| サイトの数 |20 |HTTP リスナーあたり 1 |
| 各リスナーあたりの URL のマップの数 |1 | |
|URL の最大長|8000|
| 最大ファイル アップロード サイズ (標準) |2 GB | |
| 最大ファイル アップロード サイズ (WAF) |100 MB| |
|WAF の本文サイズの制限 (ファイルがない場合)|128 KB|

