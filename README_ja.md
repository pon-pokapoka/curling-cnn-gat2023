# Curling CNN
CurlingCNNは、GAT2023の第9回UEC-cupデジタルカーリング大会で優勝した、CNNを使ったカーリングAIです

トーナメントのホームページ:
http://minerva.cs.uec.ac.jp/cgi-bin/curling/wiki.cgi?page=GAT%5F2023

## 概要
CurlingCNNはDigitalCurling3のシステム上で動作します。
- [ウェブサイト](http://minerva.cs.uec.ac.jp/cgi-bin/curling/wiki.cgi?page=FrontPage)
- [GitHub](https://github.com/digitalcurling/DigitalCurling3)

CNNはシートの画像を入力とし、勝利確率と次のショットを予測します。
CNNは、カーリング世界選手権とオリンピックの試合データを用いて学習しました。
このプログラムはAlphaGo Zeroとdlshogiを参考にしています。

## 動作環境
UbuntuとWindowsでの動作を確認しています。
- g++
- cmake
- boost
- libtorch
- CUDA
- CUDnn

CurlingCNNはGPUがなくても動作しますが、処理は遅くなります。GPUの有無にかかわらず、ビルドにはCUDA版のlibtorchが必要です。

## ビルド

1. CMakeLists.txtを編集し、libtorchとboostへのパスを設定する。
2. 以下のコマンドを実行します。
```
git submodule add https://github.com/digitalcurling/DigitalCurling3.git extern/DigitalCurling3
git submodule update --init --recursive

mkdir ビルド
cd ビルド
cmake -DCMAKE_BUILD_TYPE=Debug ....
cmake --build . --config デバッグ
```

## 使用方法
CurlingCNNはDigitalCurling3上で動作します。
ローカルで対戦するには、DigitalCurling3-Serverをインストールしてください。[チュートリアル](https://github.com/digitalcurling/DigitalCurling3/wiki/%E6%80%9D%E8%80%83%E3%82%A8%E3%83%B3%E3%82%B8%E3%83%B3%E3%81%AE%E9%96%8B%E7%99%BA%E6%96%B9%E6%B3%95#%E3%83%AD%E3%83%BC%E3%82%AB%E3%83%AB%E5%AF%BE%E6%88%A6%E3%82%92%E8%A1%8C%E3%81%86).

## モデル
モデルは非公開ですが、概要は以下のとおりです。
ネットワークは、シート、エンド、得点差、投数を入力とします。
Policyネットワークは次のショットの候補を予測します（スピード、角度、回転）。
ValueネットワークとScoreネットワークはそれぞれゲームの勝敗と終了時のスコアを予測する。

## 学習
モデルの学習には、[curlit](https://curlit.com/results)で公開されているshot by shotから抽出した試合データを使用します。データ抽出については[こちら](https://www.jordanmyslik.com/portfolio/curling-analytics/)を参照してください。
なお、shot by shotにはショットのスピードや角度が含まれていないため、モデルのPolicyネットワークは学習していません。

## ショットの選択方法
CurlingCNNは、現在の状態からショット候補（速度、角度、回転）を推定します。
ショット候補のシミュレーションの結果、最も勝率が高くなるショットを選択します。
CurlingCNNは深さ1の探索木を用いています。

## 著者
吉岡岳洋
- [@pon_pokapoka](https://twitter.com/pon_pokapoka)


# ライセンス
このプロジェクトは、MITライセンスに準拠します。
