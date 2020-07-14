---
title: Spark deploy mode 비교
date: 2020-07-14 17:43:00
categories: "HadoopEcosystem"
tags: ["Apache Spark", "Hadoop"]
---

Spark는 Client mode와 Cluster mode 두 가지 deploy mode가 있다. 내 팀에서는 2개의 메인 프로덕트가 있는데, 하나는 Cluster mode를 사용하여 Yarn에 의해 관리되는 Hadoop Cluster에 배포되어 실행되며 대개 1~2시간 내에 끝나는 비교적 짧은 라이프 타임의 앱이고, 다른 하나는 Client Mode를 사용하여 새로운 배포가 있거나 의도적으로 Kill이 없는 이상 계속 살아 있는 긴 라이프 타임앱이다.

이 두 가지 Deploy mode가 어떻게 다른지 살펴보자

먼저 공식 Document에는 아쉽게도 아주 짧게 설명되어 있다. 처음 Spark에 입문할 때 나는 이 짧은 문장이 두 모드의 차이가 어떻게 되는지 명료하진 않았다.

```
In cluster mode, the Spark driver runs inside an application master process which is managed by YARN on the cluster, and the client can go away after initiating the application. In client mode, the driver runs in the client process, and the application master is only used for requesting resources from YARN.
```
Cluster mode에서는 Driver는 Yarn에 의해 관리되는 Cluster의 Application Master process에서 실횡되며, Client는 초기화 작업 후에 가버릴(?) 수 있다. 반면 Client mode에서는 Driver는 Client process에서 실행되며, Application master는 Yarn에 리소스 요청을 위해서만 사용된다 

# Cluster mode
![Spark cluster mode architecture](/images/spark_cluster_overview.png)

`Yarn`과 같은 Resource manage 어플리케이션을 통해 배포되면, Cluster에 있는 하나의 Worker 노드에 `Driver Program`이 할당이 된다. `Driver program`은 하나의 독립된 Process이며, Master node에 의해 어느 Node에서 실행될건지 결정된다(Master node는 참고로 `Yarn`의 `Resource manager`가 실행되고 있는 Node를 뜻한다). 그림에서 `Driver program`은 `SparkContext`를 build하고 이를 통해 `Yarn`과 소통하고 각 `Executor`를 제어/소통한다. 참고로 `Cluster Manager`는 `Yarn`만을 지칭하는 것이 아니라, `Spark`에서 제공하는 자체 `Cluster manager`일 수도 있고, `Mesos`일 수도 있다(이들 모두 Cluster 전체 Resource를 관리해주는 Application이다). `SparkContext`에 의해 `Cluster Manager`와 연결되면, `Cluster manager`에게 `Executor`를 요청한다. 다음으로 각 `Executor`에 실행에 필요한 파일을 전송한다(Jar 또는 Python 파일 등). 마지막으로 각 `Executor`에서 실행해야할 Task들을 전송한다. 각 `Executor`는 Cluster에 있는 Worker node들이며, 각각이 독립된 Process이다. 물론 한 Node에 두 개 이상의 `Executor`가 실행될 수도 있다. 다만 각각의 `Executor`는 독립된 JVM Process이다.

# Client mode
Spark document에는 따로 없어 직접 그려보았다.
![Spark client mode architecture](/images/spark_client_overview.png)

Cluster 모드와 가장 큰 차이점은, `Driver program`이 Client machine에서 실행된다는 점이다. 즉 사전에 지정된 Machine에 Spark App을 배포하고, 이 App이 SparkContext를 Build하면 Client가 Driver Program이 된다. Cluster mode에서는 `ResourceManager`에 의해 할당된 `Application Master`에서 Driver Program이 실행된다는 점에서 대비된다. 따라서, Cluster mode는 별도의 Driver를 위한 리소스가 Cluster에 필요한데, Client에서는 Client의 리소스를 사용하기때문에 Cluster 리소스를 아주 조금 더 아낄 수 있는 셈이다. 다만 Application Master는 두 모드 동일하게 Cluster에서 실행되며, ResourceManager에 리소스를 요청하는 역할을 담당한다.

각각의 모드는 언제 더 적합한지, 장단점은 무엇인지는 다음 Post에서 다루겠다.