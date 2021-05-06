# infinite_buying_upbit
업비트 API를 이용하여 코인 시장에서 무한매수법을 자동화한 코드이다.
[무한매수법의 원 출처](https://m.blog.naver.com/edgar0418/222224056120)    


## 무한매수법의 기본 전략
자세한 방법은 원 출처에 서술되어 있다. 간단히 요약하면, 무한매수법은 원 출처에서는 40일+ 의 주기를 가지는, 일종의 스윙트레이딩이다. 전체 예산을 40등분해 하루에 매수할 금액을 정하고, 그 중 절반의 금액은 무조건 시장가에 매수한다. 나머지 절반의 금액은 현재 호가가 평균단가보다 낮을 경우 매수한다. 그리고 예산을 다 사용했을 시 가격에 관계없이 전부 매도하고 처음부터 다시 시작한다.      


## 무한매수법의 기본 원리
정액매수이기 때문에 평균단가 이하에서 매수시 정량매수보다 평균단가를 더 끌어내리는 효과를 갖는다. 그리고 일정 비율(원 출처에서는 10%) 이득이 났을 경우 바로 매도한다. 그렇기 때문에 변동성이 높은 주식을 고르는 것이 중요하다. 랜덤한 변동성이 아래로 향할 때는 정액매수로 평균단가를 낮추고, 위로 향할 때 이득을 보고 매도를 하는것이다. 계속 상승하는 장에서도 절반씩은 무조건 구매하기 때문에, 이득을 낼 수 있다.     


## 코인에서의 무한매수법
정액매수이기 때문에 소수점 단위로 살 수 있는 코인에 특히 더 효과적이다. 주식의 경우 하루치 매수액으로 살 수 있는 주식이 홀수가 될 수도 있고, 매일 나누어 떨어지지 않고 남는 돈이 생기기 때문이다. 그리고 변동성이 크다는 점도 무한매수법에 적합하다. 무한매수법은 변동성을 이용해 수익을 내는 전략이기 때문이다.     


## 기존의 무한매수법과 다르게 변형한 점들      

1. 코인시장은 주식시장과는 달리 24시간이기 때문에 장 시작과 장 마감의 개념이 없다. 따라서 하루 중 아무 때나 고정된 시간을 정해서 그 때 절반을 매수하면 된다. 더 나아가서, 꼭 하루 주기로 절반씩을 매수할 필요가 없다. 현재 코드에서는 6시간을 주기로 하고 있다. 

2. 장이 계속 이어지기 때문에 LOC와 같은 편리한 기능 또한 없다. 따라서, '평균단가보다 낮으면 매수하기' 를 구현하기 위해 짧은 시간(현재 코드에서는 6분) 마다 호가를 체크하고 평균단가보다 낮을 때 매수하는 방식으로 구현하였다. 

3. 기존 무한매수법의 절반씩은 무조건 시장가에 매수하는데, 이를 좀 더 효율적으로 할 수 있게 시도해보았다. [비서 문제(술탄의 딸 문제)](https://en.wikipedia.org/wiki/Secretary_problem) 에서의 결론을 사용해 보았는데, 매수주기의 1/e동안 시장가를 관측해 최소가를 구하고, 그 후 구한 최소가에 매수주문을 걸어 놓는다. 실제 구현은 2에서 짧은 시간 동안 호가를 체크할 때 최소가도 계속 업데이트하게 하였다. 그리고, 주기가 끝날 때까지 매수가 되지 않았다면, 그 때 시장가에 구매한다. 무조건 시장가에 구매하는 것보다는 이쪽이 좀 더 싸게 살 수 있다고 생각하였다. 

4. 코인시장은 종종 가격이 큰 폭으로 급락할 때가 있다. 기존의 무한매수법은 손절 기능이 없다. 40일간의 주기가 끝나면 모두 매도하는 것으로 손절을 대신하는 것이다. 하지만 코인시장은 주식시장보다 변동성이 훨씬 심하고, 하강할때도 마찬가지다. 그래서 최소한의 안전장치를 마련해 놓고자 2번에서 매번 호가를 체크할 때 호가가 평균단가의 10%미만이라면 손절하는 것으로 하였다. 그래서 최대 손실을 10%남짓으로 제한할 수 있다. 

5. 모든 하이퍼파라미터들(예산을 몇등분하는지, 몇 %의 이득에 매도하는지 등)은 원 출처에서 저자가 미국주식을 대상으로 백테스팅한 결과이다. 코인시장에서는 조금 다를 수 있다고 생각하여, 파라미터들을 다르게 설정하고 있다. 
