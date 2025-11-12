# ReadSffFile

現在開発中...以下下書き

## 基本仕様
MUGENキャラ制作で使用されるSAEのSffファイルを読み取り、スプライトリスト等のデータを取得できます  
SFFv1のみで使用可能です　SFFv2/SFFv2.1は想定していません

## 想定環境
Windows11  
C++言語標準：ISO C++17標準  

上記の環境で動作することを確認しています  
上記以外の環境での動作は保証しません  

## 使用可能な関数一覧
##### デフォルトコンストラクタ

##### 任意ファイル読み込み

##### 指定番号の存在確認

##### 指定番号へのアクセス

##### 指定番号をBMP出力

##### すべての格納画像をBMP出力

##### メンバー初期化

##### 

##### 

##### サイズ取得

##### エラー出力切り替え設定/取得
このライブラリ関数で発生したエラーを例外として投げるかログとして記録するかを指定できます  

SetThrowError(bool flag);  
引数1 bool (false = ログとして記録する, true = 例外を投げる)  
戻り値 なし(void)  

GetThrowError(); // エラー出力切り替え設定を取得  
戻り値 bool (false = ログとして記録する, true = 例外を投げる)

##### エラーログファイルを作成設定/取得
このライブラリ関数で発生したエラーのログファイルを出力するかどうか指定できます  

SetCreateLogFile(bool flag);  
引数1 bool (false = ログファイルを出力しない, true = ログファイルを出力する)  
戻り値 なし(void)  

GetCreateLogFile(); // エラーログファイルを作成設定を取得  
戻り値 bool (false = ログファイルを出力しない, true = ログファイルを出力する)    

##### SAELibフォルダを作成設定/取得
ファイルの出力先としてSAELibファイルを使用するかを指定できます  

SetCreateSAELibFile(bool flag, const std::string& Path = "");  
引数1 bool (false = SAELibファイルを使用しない, true = SAELibファイルを使用する)  
引数2 const std::string& (SAELibフォルダ作成先 (省略時はパスの設定なし))  
戻り値 なし(void)  

GetCreateSAELibFile(); // SAELibフォルダを作成設定を取得  
戻り値 void  

##### SAELibフォルダのパス設定/取得
SAELibファイルの作成パスを指定できます  

SetSAELibFilePath(const std::string& Path = "");  
引数1 const std::string& Path SAELibフォルダ作成先  

GetSAELibFilePath(); // SAELibフォルダを作成パス取得

##### SFFファイルの検索パス設定/取得
SFFファイルの検索先のパスを指定できます  
SFFコンストラクタもしくはLoadSFF関数で検索先のパスを指定しない場合、この設定のパスで検索します  

SetSFFSearchPath(const std::string& Path = ""); // SFFファイルの検索パス設定  
引数1 const std::string& Path SFFファイルの検索先のパス  

GetSFFSearchPath(); // SFFファイルの検索パス取得

## エラー情報一覧




## 使用例
////////////////////////////////////////////////////  
// 定義時にファイルを読み込む  
ReadSffFile::SFF Test("kfm.sff");  

// スプライトリスト9000-1番の座標を取得  
Test.DataList(9000, 1).AxisX();  
Test.DataList(9000, 1).AxisY();  

// 既存のデータを上書きして別のファイルを読み込む  
Test.LoadSFF("kfm720.sff");  
////////////////////////////////////////////////////  
