## Seq2Seq翻譯

### README.md
[中文](/README.md "link")<br>
[English](/README.en.md "link")<br>

### 流程架構
![modelflow](./images/modelflow.png)
<br>
主要流程:
1. 資料前處理、清理(dataclean.ipynb)
2. 建dataset
    * 使用sentencepiece model將語句tokenize，並進行編碼
3. 輸入Seq2Seq模型
<br>

### Seq2Seq模型運作
![seq2seq](./images/seq2seq.png)
<br>
* Encoder: src(輸入語言)資料依序輸入進RNN-base encoder，得到內含時間資訊的hidden state。
* Decoder: hidden state傳入RNN-base decoder，並依序輸入prev(目標語言上一個時刻的token，從bos開始)，以此預測出接續的目標語言。
* 預測結果將與tgt(目標語言此時刻的token)進行比對，以此訓練模型。
<br>

### Attention
![attention](./images/attention.png)圖片源自網路global attention
<br>
* Decoder內部有attention layer，負責找出要翻譯該時刻的目標語言，所需關注的是輸入語言的哪個部分。
* 使與src相關的hidden state(來自Encoder)和prev的hidden state(可能經過embed或神經網路)這兩者產生聯繫，方法如矩陣相乘、兩者串聯後交由神經網路處理等。
* 此聯繫結果會產生一組分數，能透過分數高低來表示是否關注於src某位置的token，並將此分數作為權重與encoder傳來的hidden state相乘，透過此種方法來影響模型翻譯的結果。
<br>
翻譯及attention如下圖，能觀察到將英文翻成中文時，每個token的翻譯會注重在英文句子的何處:<br>

![attnmap](./images/attnmap.png)
