# School_work
My school work

decision tree들은 dependent var이 categorical, regression은 dependent var이 numerical 인경우에 대해서 고안되어 있음.

mxn matrix R이 있다고 할때 n-1개의 칼럼이 independent var이고 마지막 칼럼이 dependent. 모든 var이 binary 하고 하면 우리는 regression tree 보단 decision tree에 대해서 얘기하는거다. 나중에 이를 다양한 var에 적용해보자.

decision tree는 계층적 decision을 통해서 데이터 공간을 계층적으로 분리함 이는 split critieria로도 알려져 있음. univariate decision tree에서는 한개의 feature가 split을 하도록 사용됨. 예를들어 feature의 값이 0이나 1인 binary matrix R에서 한쪽 가지에는 값을 0으로 가지는 data record들이 위치하고 한쪽에는 1을 가지는 data record들이 위치한다. 한 브랜치는 하나의 클래스를 가지고 다른 브랜치는 다른 클래스를 가질 것이다. 각 노드가 두개의 자식이 있는 경우 decision tree를 binary decision tree로 부른다.

분리의 품질은 자식 노드들의 가중 평균 gini index로 평가될수 있다. 만약 p1 부터 pr이 s노드에 있는 서로 다른 r개의 클래스에 속한 데이터 record인 경우 노드의gini index G(S)는 아래 처럼 정의된다.
<수식>

Gini index는 0과 1사이에 위치하고 작은 값일 수록 큰 분별능력을 의미한다. 분할의 전체적인 gini index는 자식 노드들의 gini index 가중평균과 일치한다. 노드의 weight는 포함된 data point의 개수로 정의 된다. binary decision tree에서 s노드의 두개의 자식 노드가 s1,s2이고 n1,n2가 data record일 때 S 분할의 gini index는 아래 수식 처럼 나타낼수 있다
<수식>

gini index는 tree의 특정 레벨에서 분할을 할때 적절한 attribute를 선택하기 위해 사용된다. 각 attribute를 gini index로 평가하여 gini index가 가장 작은 attribute가 선택된다. 이 접근 방식은 탑다운 방식으로 계층적으로 진행되며 각 노드가 특정 클래스에 속한 data record를 가질대 까지만 진행된다. 노드에 특정 클래스에 해당하는 최소의 records들이 있을 경우 tree의 성장을 이르게 멈출 수 도 있다. 이런 노드는 leaf노드라고 하며 노드의 주요 클래스로 라벨된다. 테스트 케이스를 분류할떄 independent variable들이 decision tree의 root 부터 leaf까지 path를 매핑할때 사용된다. decision 트리가 데이터의 계층적 분류 이기 때문에 root부터 leaf 까지 특정한 한 path를 따를것이다. figure 3.2에 4개의 binary attribute로 구성된 decision 트리의 예시이다. leaf node들은 음영 표시되어 있음. 모든 attribute들이 decision tree의 분할에 사용되진 않음. 예를들어 맨 왼족은 1,2를 사용하고 3,4를 쓰진 않음. 더 나아가 나른 path 들은 다른 순서의 attribute들을 사용함. 이런 상황은 고차원 데이터 일수록 잦음. 테스트 인스턴스들은 각각의 unique한 leaf node에 매핑됨 데이터 분할의 계층적 특성 때문에

이 접근은 수치로된 dependent랑 independent var에도 작은 수정을 통해 적용 가능. 수치 variable들을 다루기 위해서는 attribute value는 구간에 따라 나눠질 수 있음. 이 접근은 다수의 분할로 이어질 수 있음 각 분할이 다른 구간에 대응하는. 이런 접근법은 categorical var에도 적용 가능함.

수치 dependent var를 다루기 위해서는 gini index에서 numeric attribute를 다루기 좋은 기준으로 변경된다. 구체적으로는 numeric dependent var의 분산이 gini index 대신에 사용됨. 낮은 분산이 더 바람직하다 왜냐면 node가 가지는 인스턴스가 ~~. leaf node의 평균 값이나 linear regression model이 prediction을 구하기 위해서 사용된다.

overfitting을 줄이기 위해서 가지치기를 수행한다. 이 경우에는 트레이닝 data의 일부가 tree생성 단계에서 사용되지 않는다. 가지치기를 테스트 하기 위해 사용되지 않은 데이터를 쓴다. node의 삭제가 제외된 데이터를 대상으로 정확도를 향상시키면 node는 가지치기 당한다. 

3.2.1
decision tree를 collabo filte 로 확장하는데 있어 주요 챌린지는 예측된 entry와 관측된 entry들이 column 기반으로 확실히 구분되지 않는다는데 있다. 더 나아가 rating matrix에 대다수의 데이터가 비어있다. 이는 트리 생성 단계에서 트레이닝 데이터를 계층적으로 분리하는데 챌린지가 된다. 더 나아가 dependent와 independent variable 간에 확실한 경계가 없기 때문에 어떤 item이 예측되어야 하는지가 불분명함

후자의 이슈는 각 item의 rating을 예측하는 decision tree를 만듬으로서 쉽게 해소된다. mxn matrix R이 있을때 각 attribute를 dependent로 고정하고 나머지를 independent로 해서 tree를 만들어야 한다. 그러므로 생성된 decision tree의 수는 item 개수와 일치한다. 

반면에 independent feature가 없는것에 대한 이슈는 해소가 더 힘들다. 특정 item이 splitting attribute로 사용될때 한쪽 가지에 item에 대한 rating이 낮은 사용자들이 배정되고 한쪽엔 높은 사용자들이 배정된다. rating matrix들이 sparse 하기 때문에 대다수의 사용자들은 rating이 없을거다. 이 사용자들은 어디로 배정되어야 하나? 논리적으로는 둘다에 배정되어야 한다. 하지만 이 경우에는 decision tree가 트레이닝 데이터에 대한 엄격한 구분이 아니다. 더 나아가 예시들이 이 트리에서는 다수의 path로 매핑되고 이는 하나의 예측으로 합쳐져야 한다.

두번째 그리고 더 합리적인 접근법은 데이터의 더 낮은 차원의 표현을 만드는거다. j번째 item에 대한 rating이 예측어야 한다 할때 j번째 칼럼을 제외한 mx(n-1) rating matrix는 저차원의 mxd 표현으로 변경되고 모든 attribute는 명시되어 있다. covariance 구하는 과정을 통해서 각 user의 rating이 모두 명시된 d차원의 벡터를 구할 수 있다. 이 축소된 표현이 j번째 item에대한 tree를 만드는데 사용된다. n개 decision tree 만들기 위해서 이 과정들이 반복된다. 그래서 j번째 트리는 j번째에대한 rating 예측에만 사용된다. 

