# Code-Switching Sentence Generation by Bert and Generative Adversarial Networks

*INTERSPEECH 2019； China Mobile Research*

### Abstract
相較於對單一語言做語音辨識(automatic speech recognition)的情境，想辨識語碼轉換(code-switching)的句子是更加困難的挑戰，其原因有二：

不同語言(或多語言)的音素(phoneme)會互相混淆。
code-switching的資料量不足以訓練聲學(acoustic)及語言(language)模型。
這篇由中國移動研究院團隊發表的paper，主旨在想解決上述的第二點。

> *In this paper, we propose an approach to compositionally employ the Bidirectional Encoder Representations from Transformers (Bert) model and Generative Adversarial Net (GAN) model for code-switching text data generation.*
> 
> *It improves upon previous work by (1) applying Bert as a masked language model to predict the mixed-in foreign words and (2) basing on the GAN framework with Bert for both the generator and discriminator to further assure the generated sentences similar enough to the natural examples.*

### Code-switching實例
code-switching大致可分類成為2種：(範例感謝學弟世弦提供！)

**1. 句內轉換(intra-sentential switching)：同一句內發生轉換**

- 你們NCS的paper寫完了沒啊？Deadline要到了耶！
- 看你說話的表情多麼地驕傲，難道不怕我 say sorry get out？(蔡依林《愛情36計》2004)
- 莎郎嘿呦 means I love you，代表著我 離不開妳。(林俊傑《只對妳說》2006 )
 - 我只幹大事 我幹大的，恁爸起大厝 我蓋大的，DAT ASS SO FAT，我幹大的，賓士AMG 我開大的 (頑童《幹大事》2017)
 
**2. 句外轉換(inter-sentential switching)：不同句發生轉換**

- 妳不回來甘講要我抱伊，按呢甘好，是我不好，我沒有緊緊抱住妳。(周杰倫 《好久不見》2010)
- Merry Merry Christmas. Lonely lonely Christmas. 想祝福不知該給誰 愛被我們打了死結。(陳奕迅《聖誕結》2007)
- I don‘t wanna live without you, I don‘t wanna be alone and
- 想念妳的每個角度，如果我們還能夠重來。(高爾宣《沒了妳》2019)

### Several generated examples
以下是論文作者展示的生成句子：
![Several generated examples](https://miro.medium.com/max/720/1*JtgtS2MKGvSHcPkt_aC7gA.png)

### Conclusion
本paper提出了基於BERT和GAN，來生成code-switching資料的方法。其中：

1. BERT用來預測另一個語言的詞，因它能考慮bidirectional context的資訊。
2. GAN用以確保生成的句子，要能貼近現實中code-switching的情境。

未來他們想要讓這個方法能夠生成更長的code-switching的片語(phrase)或段落(segment)。

### Reference
[Code-Switching Sentence Generation by Bert and Generative Adversarial
Networks](https://www.isca-speech.org/archive_v0/Interspeech_2019/pdfs/2501.pdf)
