# Word Prediction using Convolutional Neural Networks—can you do better than iPhone™ Keyboard?

이 프로젝트에서 우리는 신경망이 현재 또는 다음 단어를 얼마나 잘 예측할 수 있는지 조사합니다. 언어 모델링은 가장 중요한 nlp 작업 중 하나이며, 학습 방법을 쉽게 찾을 수 있습니다. 우리의 공헌은 세 가지입니다. 첫째, 일반적인 모델링 목적보다는 모바일 환경을 시뮬레이션하는 모델을 만들고 싶습니다. 따라서 perplexity를 평가하는 대신 사용자가 입력해야하는 키 입력을 저장하려고합니다. 이를 위해 비교를 위해 iPhone 7을 사용하여 수동으로 64 개의 영어 단락을 입력했습니다. 슈퍼 지루했지만 잘하면 그것은 다른 사람들에게 유용 할 것입니다. 다음으로 RNN 대신 CNN을 사용합니다. RNN은 언어 모델링 작업에서보다 널리 사용됩니다. RNNs - LSTM 또는 GRU와 같은 향상된 유형조차도 단기 기억에 시달립니다. CNN의 깊은 층은 한계를 극복 할 것으로 예상됩니다. 마지막으로 우리는 여기서 문자 대 단어 모델을 사용합니다. 구체적으로, 현재 또는 다음 단어를 예측하여 앞의 50자를 봅니다. 타이핑 할 때마다 예측을해야하기 때문에 단어 대 단어 모델은 잘 맞지 않습니다. char-to-char 모델은 자기 회귀 적 가정에 의존한다는 점에서 한계가 있습니다. 우리의 현재의 믿음은 문자 대 단어 모델이이 작업에 가장 적합하다는 것입니다. 우리의 비교적 단순한 모델이 iPhone 7 키보드의 몇 단계 뒤에 있지만, 우리는 그 잠재력을 관찰했습니다.

## Requirements
  * numpy >= 1.11.1
  * sugartensor >= 0.0.2.4
  * lxml >= 3.6.4.
  * nltk >= 3.2.1.
  * regex

## Background / Glossary / Metric

<img src="image/word_prediction.gif" width="200" align="right">

* 대부분의 스마트 폰 키보드는 사용자의 타이핑을 저장하는 단어 예측 옵션을 제공합니다. 이 옵션을 켜면 키보드 영역 상단에 추천 단어가 표시됩니다. iPhone에서 가장 왼쪽에있는 것은 축 어적이며 중간에있는 것이 가장 인기있는 후보입니다.

* Full Keystrokes (FK): 사용자가 예측 옵션을 비활성화했다고 가정 할 때의 키 입력입니다. 이 실험에서 FK의 수는 문자의 수 (공백 포함)와 동일합니다.
* Responsive Keystroke (RK): 사용자가 의도 한 단어가 제안 될 때 사용자가 항상 그것을 선택하도록하는 키 스트로크. 특히, 우리는 여기서 최고의 후보만을 고려합니다.
* Keystroke Savings Rate (KSR):  예측 엔진에 의한 저축률. 다음과 같이 계산됩니다.
  * KSR = (FK - RK) / FK 


## Data
* 교육 및 테스트를 위해 지난 6 개월 동안 wikinews 덤프에서 영어 뉴스 코퍼스를 구축합니다.

## Model Architecture / Hyper-parameters

* 20 * conv layer with kernel size=5, dimensions=300
* residual connection

## Work Flow

* STEP 1. Download [English wikinews dumps](https://dumps.wikimedia.org/enwikinews/20170120/).
* STEP 2. Extract them and copy the xml files to `data/raw` folder.
* STEP 3. Run `build_corpus.py` to build an English news corpus.
* STEP 4. Run `prepro.py` to make vocabulary and training/test data.
* STEP 5. Run `train.py`.
* STEP 6. Run `eval.py` to get the results for the test sentences.
* STEP 7. We manually tested for the same test sentences with iPhone 7 keyboard.

### if you want to use the pretrained model,

* Download [the output files](https://drive.google.com/open?id=0B0ZXk88koS2KemFWdFNoSnBfNDg) of STEP 3 and STEP 4, then extract them to `data/` folder.
* Download [the pre-trained model files](https://drive.google.com/open?id=0B0ZXk88koS2KNHBuM09kSXFJNzA), then extract them to `asset/train` folder.
* Run `eval.py`.

## Updates
* In the fourth week of Feb., 2017, we refactored the source file for TensorFlow 1.0.
* In addition, we changed the last global-average pooling to inverse-weighted pooling. As a result, the #KSR improved from 0.39 to 0.42. Check [this](https://github.com/Kyubyong/word_prediction/blob/master/train.py#L81).

## Results

The training took ~~4-5~~ 2-3 days on my single GPU (gtx 1060). As can be seen below, our model is lower than iPhone in KSR by ~~8~~ 5 percent points. Details are available in `results.csv`. 

| #FK | #RK: Ours | #RK: iPhone 7 |
|--- |--- |--- |--- |--- |
| 40,787 | ~~24,727 (=0.39 ksr)~~ <br>->23,753 (=0.42 ksr) | 21,535 (=0.47 ksr)|

## Conclusions
* Unfortunately, our simple model failed to show better performance than the iPhone predictive engine.
* Keep in mind that in practice predictive engines make use of other information such as user history.
* There is still much room for improvement. Here are some ideas.
  * You can refine the model architecture or hyperparameters.
  * As always, bigger data is better.
* Can anybody implement a traditional n-gram model for comparison?

## Cited By
* Zhe Zeng & Matthias Roetting, A Text Entry Interface using Smooth Pursuit Movements and Language Model, Proceeding
ETRA '18 Proceedings of the 2018 ACM Symposium on Eye Tracking Research & Applications, 2018


