# Korean Parallel Corpus
1) [What is KPC?](#one)   
2) [Data](#two)   
3) [Translation Model Experiments](#three)   
4) [Change Log](#four)   
5) [Contacts](#five)
6) [Licnese](#six)

<a name="one"></a>
## **1) What is KPC?**

Korean is the official language of both South Korea and North Korea. Despite sharing the same language, the North Korean and South Korean language differ in various linguistic aspects such as vocabulary, grammar, and spelling. The ongoing separation between North Korea and South Korea has widened the differences between the two languages. This language gap can become a major communication obstacle after Korean reunification. 

Therefore, it is important to conduct research on how to bridge the gap between the North Korean and South Korean languages. One example would be to develop a North and South Korean translator. However, it is difficult to find a North Korean language dataset that has a corresponding South Korean language dataset. The lack of a North Korean and South Korean parallel corpus hinders active investments in machine translation of the North Korean language. 

To address this issue, the Korean Unification Parallel Corpus (KPC) repository has been created. Its main goal is to provide a high-quality North and South Korean parallel corpus and make it available to the public. The KPC also explains how to use the parallel corpus for research, particularly in the field of machine translation. 

### **1-1) Sources**

The dataset contains **130,738 rows** covering a range of topics of classical novel and the Bible. The classical novels are

### **1-2) Data Selection**
#### **Criteria for Selection**
- Data must be actually existing in South and North Korea.
- Data must be accurately matched as sentence pairs.

#### **Data Acquisition**
- `Bible`: The Bible is translated into many languages, divided into chapters and verses, with consistent content across verses, making it useful for matching.
- `Classic novels`: Classic novels are translated into various languages and with translations available in both South Korean and North Korean.

<a name="two"></a>
## **2) Data**
### **2-1) List**

<table border="1" width="80%">
  <tr>
    <th colspan="2">Category</th>
    <th>Book</th>
    <th colspan="2">Total Row</th>
  </tr>
  <tr>
    <td rowspan="5">Classic Novels</td>
    <td rowspan="2">Foreign</td>
    <td>Jane Eyre</td>
    <td>60,331</td>
    <td rowspan="2">94,459 (72%)</td>
  </tr>
  <tr>
    <td>The Red and the Black</td>
    <td>34,128</td>
  </tr>
  <tr>
    <td rowspan="3">Korean</td>
    <td>Onggojip-jeon</td>
    <td>988</td>
    <td rowspan="3">6,293 (5%)</td>
  </tr>
  <tr>
    <td>Sukhyang-jeon</td>
    <td>3,538</td>
  </tr>
  <tr>
    <td>Shimchung-jeon</td>
    <td>1,767</td>
  </tr>
  <tr>
    <td colspan="2">Bible</td>
    <td>-</td>
    <td>-</td>
    <td>29,986 (23%)</td>
  </tr>
    <td colspan="2">Total</td>
    <td>-</td>
    <td>-</td>
    <td>130,738 (100%)</td>
  </tr>
</table>

The dataset consists of classic novels and the Bible. The classic novel data is divided into two types of foreign novels and three types of Korean novels, each based on single data from North Korean publishers and multiple data from South Korean publishers. Consequently, the classic novel data collected a total of 100,752 North Korean-South Korean sentence pairs. The Bible data was collected in the same manner, resulting in a total of 29,986 data points. Thus, a total of 130,738 parallel corpora were constructed based on South Korean standards. Among these, the maximum number of characters per sentence is 286, and the minimum is 2.

### **2-2) Examples**
<table border="1">
  <tr>
    <th>nk</th>
    <th>sk</th>
  </tr>
  <tr>
    <td>안해는 남편앞에 무릎을 꿇고 그를 붙들어두려고 하면서 부르짖었다.
    </td>
    <td>부인은 남편 앞에 무릎을 꿇고 그를 붙잡으려고 애쓰면서 소리쳤다.
    </td>
  </tr>
  <tr>
    <td>나는 창가림을 드리우고 난로가에 되돌아왔다.
    </td>
    <td>나는 커튼을 내리고 난롯가로 되돌아갔다.
    </td>
  </tr>
</table>

<a name="three"></a>
## **3) Translation Model Experiments**
### **3-1) Experimental Settings**
#### **Foundation Model**
[KoBART](https://github.com/seujung/KoBART-translation) (Korean BART) was used as the foundation translation model. KoBART was developed by the SKT AI team.

#### **Training**
We trained a North Korean(NK) → South Korean(SK)  translation model and a South Korean(SK) → North Korean(NK) translation model. The training was conducted on 90% of all the 13,0738 rows of classic novels and bible data. The remaining 10% was used as the test data. 

The data was split into a 9:1 ratio for training and testing. For foreign novel data, since each book is based on single data from North Korean publishers and multiple data from South Korean publishers, the same North Korean sentences are repeated as many times as the number of publications from South Korean publishers. Thus, caution was taken to ensure that North Korean sentences in the test data did not exist in the training data. 

For Jane Eyre, a certain number of rows were randomly selected from the North Korean data, and the corresponding North-South Korean sentence pairs were extracted as test data, while the remaining sentence pairs were used as training data.

<table border="1">
  <tr>
    <th></th>
    <th>train</th>
    <th>test</th>
  </tr>
  <tr>
    <td>Count</td>
    <td>117,665</td>
    <td>13,073</td>
  </tr>
  <tr>
    <td>Size</td>
    <td>9.9MB</td>
    <td>961KB</td>
</table>

For the training process, the hyperparameters were set as follows. 

<table border="1">
  <tr>
    <th></th>
    <th>NK → SK model</th>
    <th>SK → NK model</th>
  </tr>
  <tr>
    <td>batch size</td>
    <td>4</td>
    <td>4</td>
  </tr>
  <tr>
    <td>epoch</td>
    <td>8</td>
    <td>8</td>
  </tr>
  <tr>
    <td>learning rate</td>
    <td>3e-5</td>
    <td>3e-5</td>
  </tr>
  <tr>
    <td>optimizer</td>
    <td>AdamW</td>
    <td>AdamW</td>
  </tr>
</table>

### **3-2) Experimental Results**
The evaluation metrics used on the test data set are the BLEU score and BERT Score. The below table presents the BLEU Score and BERT Score of the North Korean(NK) → South Korean(SK) translation model and the South Korean(SK) → North Korean(NK) translation model.

*cf. BERT Score computes precision, recall, and F1-score. For simplicity, only the F1-score is presented in the table.*

<table border="1">
  <tr>
    <th></th>
    <th>NK → SK model</th>
    <th>SK → NK model</th>
  </tr>
  <tr>
    <td>BLEU Score</td>
    <td>0.55</td>
    <td>0.25</td>
  </tr>
  <tr>
    <td>BERT Score</td>
    <td>0.821</td>
    <td>0.815</td>
  </tr>

</table>


<a name="four"></a>
## 4) Change Log

<a name="five"></a>
## 5) Contacts

<a name="six"></a>
## 6) License
## References
