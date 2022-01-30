## BERT (Bidirectional Encoder Representations from Transformers) 간략 소개

- 2018년 10월 구글이 공개한 [논문](https://arxiv.org/abs/1810.04805)에 기반을 둔 모델 ([Official release github](https://github.com/google-research/bert))
- 기존 자연어 모델의 성능을 가볍게 뛰어넘었으며, 일부 성능 평가에서는 인간보다 더 높은 성능을 기록함
- Wikipedia, [BookCorpus](https://yknzhu.wixsite.com/mbweb) 같은 방대한 양의 Corpus를 기반으로 **general-purpose "language understanding" model**을 사전 학습함
- 이미 학습을 마친 언어에 대한 폭 넓은 이해를 바탕으로 약간의 fine-tuning 만으로도 다양한 문제들에 적용이 가능해짐



## 모델 학습 과정의 특징

1. Unsupervised learning 방식으로 학습을 진행 (엄밀하게는 스스로 라벨을 만들고 수행하므로 Semi-supervised learning)

```
-> 방대한 데이터에 라벨링을 하지 않아도 학습 데이터로 활용이 가능 
```

2. **MLM(Masked Language Model)**

: 문장의 중간에 놓인 단어를 가리고 이를 맞추는 방식으로 bi-directionally 학습을 진행 (80% masked, 10% changed, 10% un-changed)

```
Input: 그 남자는 [MASK1]에 방문했다. 그는 한 [MASK2]의 아메리카노를 구매했다. 
Labels: [MASK1] = 스타벅스; [MASK2] = 잔
-> 양방향의 문맥을 학습함으로써 맞춰야 하는 단어 이후의 문맥까지도 고려해 성능을 높일 수 있음 & 과적합 경감
```

3. **NSP(Next Sentence Prediction)**

: 쌍을 이룬 문장들을 대상으로 순서 상 연결되어 corpus에서 등장한 문장들인지 맞추는 학습을 진행 (50% IsNext, 50% NotNext)

```
Sentence A: 그 남자는 스타벅스에 방문했다.
Sentence B: 그는 한 잔의 아메리카노를 구매했다.
Label: IsNextSentence

Sentence A: 그 남자는 스타벅스에 방문했다.
Sentence B: 기차는 레일 위를 달린다.
Label: NotNextSentence
-> 다양한 세부 NLP task들에서 필요한 문장 간의 관계를 학습할 수 있음
```



* Pre-training은 1개의 언어에 대해 4~16 Cloud TPUs를 기반으로 4일이 소요됨

* Fine-tuning은 1개의 언어에 대해 1 Cloud TPU만으로도 최대 1시간 만에 가능



이미 언어의 다양한 문맥을 학습한 상태이므로, 용도에 따른 약간의 레이어 추가만으로도 다양한 과제를 해결할 수 있게 됨 (수많은 문장들을 접한 사람이 자연스럽게 문장 내용의 카테고리를 구분하거나, 문장 간의 연결이 어색한지 판단하거나, 문장에서 특정한 단어의 역할을 유추할 수 있는 것과 동일)