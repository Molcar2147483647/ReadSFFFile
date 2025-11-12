# ReadSffFile

現在開発中...以下下書き

## 基本仕様
SAEツールで使用される.sffファイルを読み取り、スプライトリスト等のデータを取得できます  
SFFv1のみで使用可能です　SFFv2/SFFv2.1は想定していません

## 想定環境
Windows11  
C++言語標準：ISO C++17標準  

上記の環境で動作することを確認しています  
上記以外の環境での動作は保証しません  

## クラス/名前空間の概要
### class SAELib::SFF
読み込んだSFFファイルのデータが格納される  
インスタンスを生成して使用する  

### class SAELib::SFFConfig
ReadSffFileライブラリの動作設定が可能  
インスタンス生成不可  

### namespace SAELib::SFFError
本ライブラリが扱うエラー情報のまとめ  
throwされた例外をcatchするために使用する  

## クラス/名前空間の関数一覧
## class SAELib::SFF
#### デフォルトコンストラクタ
SFFクラスのインスタンスを生成します  
LoadSFF関数と同様の引数を指定可能です  
引数なしで生成した場合、ファイル読み込みは行いません  
```
SAELib::SFF sff;
```
#### 指定されたSFFファイルを読み込み
実行ファイルから子階層へファイル名を検索して読み込みます  
第二引数指定時は指定した階層からファイル名を検索します(SFFConfigよりも優先されます)  
実行時に既存の要素は初期化、上書きされます  
```
sff.LoadSFF("kfm.sff");                 // 実行ファイルの階層から検索
sff.LoadSFF("kfm.sff", "C:/MugenData"); // 指定パスから検索
```
引数1 const std::string& ファイル名(拡張子 .sff は省略可)  
引数2 const std::string& 対象のパス(省略時は実行ファイルの子階層を探索)  
戻り値 bool(true = 成功：false = 失敗)  

#### 指定番号の存在確認
読み込んだSFFデータを検索し、指定番号が存在するかを確認します  
```
sff.ExistSpriteNumber(9000, 0); // 画像番号9000-0が存在するか確認
```
引数1 int32_t グループ番号  
引数2 int32_t イメージ番号  
戻り値 bool(true = 成功：false = 失敗)  

#### 指定番号のデータへのアクセス
指定番号のSFFデータへアクセスします  
対象が存在しない場合はSFFConfig::SetThrowErrorの設定に準拠します  
```
sff.DataList(9000, 0).AxisX(); // 画像番号9000-0のX軸を取得
```
引数1 int32_t グループ番号  
引数2 int32_t イメージ番号  
戻り値1 対象が存在する DataList(GroupNo, ImageNo)の参照  
戻り値2 対象が存在しない SFFConfig::SetThrowError (true == 例外を投げる：false == ダミーデータの参照)  

#### 指定番号の画像をBMP出力
指定番号のSFFデータをBMPファイルとして出力します  
出力先のファイルは SFFConfig::SetSAELibPath の設定に準拠します  
```
sff.ExportToBMP(9000, 0); // 画像番号9000-0の画像をBMP出力
```
引数1 int32_t グループ番号  
引数2 int32_t イメージ番号  
戻り値 bool(true = 成功：false = 失敗)  

#### 全ての格納画像をBMP出力
読み込んだSFFデータ全てをBMPファイルとして出力します  
出力先のファイルは SFFConfig::SetSAELibPath の設定に準拠します  
```
sff.ExportToBMP(true); // 取得画像をBMP出力
```
引数1 bool 重複した画像を出力するか(false = 含まない：true = 含む)  
戻り値 bool(true = 成功：false = 失敗)  

## class SAELib::SFFConfig
#### エラー出力切り替え設定/取得
このライブラリ関数で発生したエラーを例外として投げるかログとして記録するかを指定できます  
```
SAELib::SFFConfig::SetThrowError(bool flag); // エラー出力切り替え設定
```
引数1 bool (false = ログとして記録する：true = 例外を投げる)  
戻り値 なし(void)  
```
SAELib::SFFConfig::GetThrowError(); // エラー出力切り替え設定を取得
```
戻り値 bool (false = ログとして記録する：true = 例外を投げる)

#### エラーログファイルを作成設定/取得
このライブラリ関数で発生したエラーのログファイルを出力するかどうか指定できます  
```
SAELib::SFFConfig::SetCreateLogFile(bool flag); // エラーログファイルを作成設定
```
引数1 bool (false = ログファイルを出力しない：true = ログファイルを出力する)  
戻り値 なし(void)  
```
SAELib::SFFConfig::GetCreateLogFile(); // エラーログファイルを作成設定を取得  
```
戻り値 bool (false = ログファイルを出力しない：true = ログファイルを出力する)    

#### SAELibフォルダを作成設定/取得
ファイルの出力先としてSAELibファイルを使用するかを指定できます  
```
SAELib::SFFConfig::SetCreateSAELibFile(bool flag, const std::string& Path = ""); // SAELibフォルダを作成設定
```
引数1 bool (false = SAELibファイルを使用しない：true = SAELibファイルを使用する)  
引数2 const std::string& SAELibフォルダ作成先(省略時はパスの設定なし)  
戻り値 なし(void)  
```
SAELib::SFFConfig::GetCreateSAELibFile(); // SAELibフォルダを作成設定を取得  
```
戻り値 const std::string& SAELibフォルダ作成先  

#### SAELibフォルダのパス設定/取得
SAELibファイルの作成パスを指定できます  
```
SAELib::SFFConfig::SetSAELibFilePath(const std::string& Path = ""); // SAELibフォルダのパス設定
```
引数1 const std::string& SAELibフォルダ作成先  
戻り値 なし(void)  
```
SAELib::SFFConfig::GetSAELibFilePath(); // SAELibフォルダを作成パス取得  
```
戻り値 const std::string& SAELibフォルダ作成先  

#### SFFファイルの検索パス設定/取得
SFFファイルの検索先のパスを指定できます  
SFFコンストラクタもしくはLoadSFF関数で検索先のパスを指定しない場合、この設定のパスで検索します  
```
SAELib::SFFConfig::SetSFFSearchPath(const std::string& Path = ""); // SFFファイルの検索パス設定  
```
引数1 const std::string& SFFファイルの検索先のパス  
戻り値 なし(void)  
```
SAELib::SFFConfig::GetSFFSearchPath(); // SFFファイルの検索パス取得  
```
戻り値 const std::string& SFFファイルの検索先のパス  

## エラー情報一覧




## 使用例
```
// 定義時にファイルを読み込む  
ReadSffFile::SFF Test("kfm.sff");  

// スプライトリスト9000-1番の座標を取得  
Test.DataList(9000, 1).AxisX();  
Test.DataList(9000, 1).AxisY();  

// 既存のデータを上書きして別のファイルを読み込む  
Test.LoadSFF("kfm720.sff");
```
