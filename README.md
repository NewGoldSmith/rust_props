<!--# 【RUST】Vidual Studio 2026でRustを設定する-->
# この記事の対象読者
- RustをVisual Studio 2026で使いたい方
# Visual Studio 2026のビルドの仕組み
　Visual Studio 2026（以下VS）は、MSBuildというCUIのビルドエンジンがビルドを行っています。ですので、ソリューションのプログラム群は、実はコマンドラインからでもビルド出来ます。MSBuildはXMLフォーマットファイル用のメイクプログラムです。一方、VSのGUIの一つにソリューションエクスプローラーというのがあります。これは、MSBuildが読み込むXMLファイル群を独自に読み込んで、GUIに変換してプロパティ項目を表示しています。ですから、独自にプロパティ項目をXMLに定義して表示する事も可能だと思われます。
しかし、実用的な設定を出来るようにするのであれば、「VSIX extension」を使ってVSのUIやMSBuildと連携できるパッケージを作るのが正道でしょう。
# 簡易的な設定  
　この記事で紹介するのは簡易的な設定です。**cargo**コマンドが使える程度でtomlは自分で設定する事になります。ソリューションエクスプローラーのプロパティも、デフォルト値からの変更が必要になる場合もあります。
# 事前準備  
- Rustのインストール   
　Rustのインストールをしておきます。`cargo init`等のコマンドが動作する事。  
- VSワークロードのインストール    
　「C＋＋によるデスクトップ開発」をインストール。  
![構成図](https://github.com/NewGoldSmith/rust_props/blob/main/rust_imgs/01work_lord.png?raw=true)  
- 「rust.props」のダウンロード  
　下記のURLから「rust.props」をダウンロード。  
[https://github.com/NewGoldSmith/rust_props](https://github.com/NewGoldSmith/rust_props)  

# 実作業
1. VSを立ち上げプロジェクトテンプレート検索ボックスに「メイクファイル」と打ち込み検索する。「メイクファイル プロジェクト」というテンプレートが表示されるのでそれを選択。  
![構成図](https://github.com/NewGoldSmith/rust_props/blob/main/rust_imgs/03make_file_project.png?raw=true)  

1. プロジェクト名、場所、ソリューション名を設定する。  
![構成図](https://github.com/NewGoldSmith/rust_props/blob/main/rust_imgs/04rust_solution.png?raw=true)  

1. 「デバッグの構成の設定」画面になりますので、cargoコマンドを設定します。  
![構成図](https://github.com/NewGoldSmith/rust_props/blob/main/rust_imgs/05debug_composition.png?raw=true)  

1. 「リリース構成の設定」画面になりますので、「デバッグ構成と同じ」にします。  
![構成図](https://github.com/NewGoldSmith/rust_props/blob/main/rust_imgs/06release_composition.png?raw=true)  

1. 「ソリューションエクスプローラー」が表示されます。  
![構成図](https://github.com/NewGoldSmith/rust_props/blob/main/rust_imgs/07solution_exproller.png?raw=true)  

1. ここで、コマンドプロンプトをプロジェクトのディレクトリをカレントディレクトリにして、`cargo init`とコマンドを打ちます。  

``` powershell
PS G:\src\rust_solution\rust_project> cargo init
    Creating binary (application) package
note: see more `Cargo.toml` keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html  
```  

7. VSのメニューから「表示->その他のウィンドウ->プロパティマネージャ」を選びます。  
![構成図](https://github.com/NewGoldSmith/rust_props/blob/main/rust_imgs/08property%20_manager.png?raw=true)    
　「既存のプロパティシートの追加」で、ダウンロードしておいた`rust.props`を選択します。  

8.プロジェクトのプロパティページを開くとこんな感じになっています。  
  - 全般  
![構成図](https://github.com/NewGoldSmith/rust_props/blob/main/rust_imgs/09solution_exproller_property.png?raw=true)    
　「ビルドログファイル」項目は`cargo clean`した時に、消せないという趣旨のメッセージが出ますので、空白にするかまたは、クレートでないディレクトリを指定します。実際のクリーン作業はcargoが行って、logを出すので消して構わないと思います。  
  - デバッグ
![構成図](https://github.com/NewGoldSmith/rust_props/blob/main/rust_imgs/10solution_exproller_property_debug.png?raw=true)  
  - NMake  
![構成図](https://github.com/NewGoldSmith/rust_props/blob/main/rust_imgs/11solution_exproller_property_NMake.png?raw=true)  

9. ビルド、リビルド、クリーンが正常に動作したら成功です。  
![構成図](https://github.com/NewGoldSmith/rust_props/blob/main/rust_imgs/12solution_exproller_build_clean_rebuild.png?raw=true)  

1. ソースファイルフィルターにCargo.tomlやmain.rsを入れる事も出来ます。  
![構成図](https://github.com/NewGoldSmith/rust_props/blob/main/rust_imgs/13solution_exproller_source_files.png?raw=true)  

1. ブレークポイントも有効です。  
![構成図](https://github.com/NewGoldSmith/rust_props/blob/main/rust_imgs/14run_breakpoint1.png?raw=true)  

1. VSの「メニューのカスタマイズ」機能を使えば、独自のコマンドのメニューを作る事も出来ます。  
![構成図](https://github.com/NewGoldSmith/rust_props/blob/main/rust_imgs/15custom_menu.png?raw=true)  
# 終わりに
　この記事が閃きのきっかけになれば幸いです。  
