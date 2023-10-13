# EMUZ80-8080

![MEZ8080](https://github.com/satoshiokue/EMUZ80-8080/blob/main/MEZ8080_1.jpeg)    
NEC 8080AF  

![MEZ8080](https://github.com/satoshiokue/EMUZ80-8080/blob/main/MEZ8080_2.jpeg)  
+12V / -5V 生成部  

電脳伝説さん(@vintagechips)のEMUZ80が出力するZ80 CPU制御信号をメザニンボードで組み替え、動作に必要な電源を用意することで8080を動作させることができます。  

Z8S18020VSCとPIC18F47Q83の組み合わせで動作確認しています。  

動作確認済みCPU  
Intel P8080A-1  
NEC 8080AF  
NEC D8080AFC  
TESLA MHB8080A  

このソースコードは電脳伝説さんのmain.cを元に改変してGPLライセンスに基づいて公開するものです。

## 8080の制御信号について
8080を動作させるためには3種の電源電圧 +12V, +5V, -5Vと非対称の2相クロック、さらに制御信号を作成するシステムコントローラが必要です。  
CPU 8080は専用のサポートICによって動作に必要な信号を生成しています。  

本システムではφ2=0.5MHzのクロック周波数で動作します。専用のクロックジェネレータが出力するφ2とφ1は一定周期で変化するのですが、EMUZ80_8080では全体の処理速度を上げるためにφ1クロックを内部処理に合わせて変化させます。  

## メザニンボード
https://github.com/satoshiokue/MEZ8080 

## 回路図
https://github.com/satoshiokue/MEZ8080/blob/main/MEZ8080.pdf

## ファームウェア

EMUZ80で配布されているフォルダemuz80.X下のmain.cと置き換えて使用してください。
* emuz80_8080.c

## アドレスマップ
```
Memory
ROM   0x0000 - 0x3FFF 16kB
RAM   0x8000 - 0x8FFF  4kB PIC18F47Q43
      0x8000 - 0x9FFF  8kB PIC18F47Q8x

I/O
通信レジスタ 0x00
制御レジスタ 0x01
```

## PICプログラムの書き込み
EMUZ80技術資料8ページにしたがってPICに適合するファイルを書き込んでください。  

またはArduino UNOを用いてPICを書き込みます。  
https://github.com/satoshiokue/Arduino-PIC-Programmer

PIC18F47Q43 emu8080_Q43.hex 

PIC18F47Q83 emu8080_Q8x.hex  
PIC18F47Q84 emu8080_Q8x.hex  

## 8080プログラムにつて
ROMにBASICとモニターを格納しています。  
電源投入でUniversal Monitorが起動します。  
モニターに追加したBコマンドによってINTEL8080 BASICが起動します。  
BASICから”MONITOR”命令を実行するとUniversal Monitotがコールドスタートします。  

INTEL8080 BASICは電脳伝説さんのSBC8080向けに整備されたデータパックに収蔵されたものにパッチを施しています。
https://vintagechips.wordpress.com/2018/06/24/sbc8080-cpuルーズキット/  

Universal Monitor  
https://electrelic.com/electrelic/node/1317  

## 8080プログラムの格納
インテルHEXデータを配列データ化して配列rom[]に格納すると0x0000に転送され8080で実行できます。  

## 謝辞
思い入れのあるCPUを動かすことのできるシンプルで美しいEMUZ80を開発された電脳伝説さんに感謝いたします。

## 参考）EMUZ80
EUMZ80はZ80CPUとPIC18F47Q43のDIP40ピンIC2つで構成されるシンプルなコンピュータです。

![EMUZ80](https://github.com/satoshiokue/EMUZ80-6502/blob/main/imgs/IMG_Z80.jpeg)

電脳伝説 - EMUZ80が完成  
https://vintagechips.wordpress.com/2022/03/05/emuz80_reference  
EMUZ80専用プリント基板 - オレンジピコショップ  
https://store.shopping.yahoo.co.jp/orangepicoshop/pico-a-051.html  
