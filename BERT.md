# BERT: Pre-trainig of Deep Bidirectional Transformers for Language Understanding

### Introduction



transformer 구조를 활용하여 encoder 만을 사용한 모델



wiki 나 book data 를 pre-trained 한 데이터를 바탕으로, 특정 task 를 가지고 있는 labeled  data 로 fine-tuning 하는 방법



이전 모델들의 접근 방식은 unidirectional 하므로 language represention 이 부족하여, bidirectioal 한 모델을 고안



### pre-trained language representation을 적용하는 방식



1. feature-based approach



특정 task를 수행하는 network에 pre-trained language representation을 추가적인 feature로 제공 → 두 개의 network를 붙여 사용 (ELMo)



2. fine-tuning approach




특정 task를 수행하는 parameter을 최대한 줄이고, pre-trained 된 parameter들을 다운 스트림 task 학습을 통해 바꿔줌 (OpenAL GPT)



### BERT 방법론



1. Masked Language Model, MLM



무작위로 15%의 token 을 선택한다. 선택된 token 중 80%를 마스킹하고, 10%를 다른 단어, 10%를 기존 단어로 유지한다. 



(굳이 이렇게 하는 이유는 fine-tuning 과정에는 masking token 을 적용하지 않기 때문에 pre-trained 과정과의 차이를 줄이기 위해서라고 한다. )



-> deep bidirectioal



2. next sentence prediction, NSP



pre-training 시 두 문장을 같이 넣어 두 문장이 이어지는 문장인지 아닌지 결정



50%의 확률로 이어지지 않는 문장이 들어간다.




-> QA 와 NLI의 성능 성향을 가져온다.



### Input Representation



![bert-input-representation.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ba3b81b0-a786-4b5a-b992-d8599b6b55f9/bert-input-representation.png)



toekn 임베딩 : wordpiece 임베딩을 사용



segment 임베딩 : 만약 두 개의 문장이 들어왔다면 이를 구분



position 임베딩 : transformer에서는 encoding을 사용



[CLS] : 모든 sentence의 첫번째 token



-> 전체 층을 다 거치고 나면 token sequence의 결합된 의미를 가지게 되는데, 여기에 간단한 classifier를 붙이면 단일 문장 또는 연속된 문장의 classification을 쉽게 알 수 있게 된다. 



[SEP] : 두 개의 문장을 구분하기 위해 사용





-> input에서 Sentence pair는 합쳐서 single sequence로 입력한다. 



sentence : 문장이 아니어도 됨 (연속된 텍스트)



sequence :  입력되는 하나나 두 개의 sentence



