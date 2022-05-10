# Attention Is All You Need



### Abstract



지배적인 sequence transduction model들은 인코더와 디코더를 포함한 RNN이나 CNN을 기반으로 했다. 좋은 성능을 보이는 모델은 인코더와 디코더를 연결할 때 attention을 썼다. 



Transformer는 recurrence과 convolution을 사용하지 않고 만든 attention 메커니즘이다. 실험에서는 해당 모델이 적은 training 시간으로 우수한 성능을 보여줌을 보여준다. 



### 1 Introduction



Recurrent model에서는 이전 hidden state ht-1 을 포함하는 hidden states ht를 생성한다. 이 일은 효율성에 상당한 향상을 가져왔지만 근본적으로 sequential computation의 제약을 가지고 있다. 



Attention mechanism은 compelling sequence modeling과 transduction model들의 통합된 모델이다. 



Transformer는 전체적인 input과 output 사이 attention 메커리즘에 의존한다. 



### 2 Background



self-attention : 다음 인코더 블록의 입력은 이전 블록의 출력



### 3 Model Architecture



Trnasformer는 인코더와 디코더 둘을 연결한다.



##### 3.1 Encoder and Decoder Stacks



![image](https://user-images.githubusercontent.com/89879599/167529648-8ca31029-e935-4647-9854-ea1e1ec213cb.png)



인코더는 N = 6인 레이어로 구성되는데, 각각 레이어는 두 개의 서브 레이어를 가진다.
첫 번째는 multi-head self-attention mechanism이고, 두 번째는 positionwise fully connected feed-forward network이다. 
두 레이어는 residual connection이다. (residual connection : 어떤 레이어를 스킵할 수 있는 방법을 제공한다.)
서브 레이어의 결과는 LayerNorm(x + Sublayer(x))이다. 
output의 차원은 d model = 512이다.



디코더는 또한 N = 6 레이어로 구성되고 서브 레이어는 3개이다. 두 레이어는 인코더와 같고 Masked multi-head self-attention을 추가로 사용한다.



##### 3.2 Attention



vector : query, keys, values, and output



query, keys, values(a weighted sum of the values)의 벡터 연산으로 output 벡터가 만들어진다. 




##### 3.2.1 Scaled Dot-Product Attention



![image](https://user-images.githubusercontent.com/89879599/167530695-c4af1b42-8f75-4bd9-b9c7-39b1f820331e.png)



Q와 K^T의 내적값(유사도) -> 어떤 시점과 연관이 높나



V(문장 행렬)는 가중치 곱



-> 특정값을 분모로 나눠주는 형식



![image](https://user-images.githubusercontent.com/89879599/167542888-1868682d-7cf7-4b23-aeb9-61a682e7c325.png)



##### 3.2.2 Multi-Head Attention



![image](https://user-images.githubusercontent.com/89879599/167543151-c60150fb-ffa7-428d-866b-9566df912c20.png)



전체 차원에 attention을 한 번 수행하는 것이 아닌 쪼개서 h번 수행 후 다시 결합 -> 병렬화



(Concat : 데이터 결합)



##### 3.2.3 Applications of Attention in our Model



- query는 이전 decoder 층에서 받고, key, value는 encoder output에서 받으므로 디코더가 모든 시점에 접근 가능하다.


- 디코더도 key, vlaue, query를 같은 곳에서 받으므로 모든 시점에 접근 가능하다.


- 디코더 층의 self-attention은 마스킹을 통해 다음 단어를 못보게 한다.



##### 3.3 Position-wise Feed-Forward Networks



![image](https://user-images.githubusercontent.com/89879599/167544408-864ee7c5-aef3-4d92-957b-84f9b62209f7.png)



##### 3.4 Embeddings and Softmax



##### 3.5 Positional Encoding



![image](https://user-images.githubusercontent.com/89879599/167545239-863ae407-5b1e-4be6-a328-d2d45627f322.png)




자리에 따라 다르게 모델이 입력된다. sin과 cos 함수가 값이 동일해지는 것을 막아준다.



### 4 Why Self-Attention



다양한 모델과 비교 : 더 작은 차원에서도 빠르다, 연산의 병렬화, 모든 시점에 일정한 연산이 가해지므로 RNN보다 빠르다.




### 5 training



##### 5.1 Training Data and Batching



4.5만 문장 이상 학습


##### 5.2 Hardware and Schedule


8 NVIDIA P100 GPUs



##### 5.3 Optimizer

B1 = 0.9



B2 = 0.98 



e = 10^-9



learning_rate = ![image](https://user-images.githubusercontent.com/89879599/167547978-5f68390c-2db6-433b-924c-00d40544098a.png)



##### 5.4 Regularization



Residual Dropout, Label Smoothing 적용



### 6 Results

기계번역 BLUE English-to-German 28.4 English-to-French 41.0



모델 변수 튜닝 완료

### 7 Conclusion

Transformer : the first sequence transduction model based entirely on
attention
