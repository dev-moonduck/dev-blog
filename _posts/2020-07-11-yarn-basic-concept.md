---
title: Hadoop Yarn 용어 정리
date: 2020-07-11 23:30:09
categories: "HadoopEcosystem"
tags: ["Hadoop Yarn", "Hadoop"]
---

# Hadoop Yarn
![Yarn architecture](../images/yarn_architecture.gif)

# 용어 정리
## Resource Manager
모든 Application에 걸쳐 Resource 할당에 대한 권한을 갖는다.
Resource Manager는 2 개의 Main Component로 구성된다.
### Scheduler  
Cluster 전체에서 작업중인 모든 Application의 Resource 할당을 관리하며, 별도의 관리 정책에 따라 관리한다. Resource를 Partitioning하여 각 Partition별로 Queue를 두고 Resource를 관리할 수 있다. 예를 들면, Production 용 Queue에 Core 100, Memory 12TB, test 용 Queue에 Core 10, Memory 100GB으로 할당하여 각 용도별로 Resource를 제한적으로 운영할 수 있다. Scheduler 그 자체는 Resource 할당에 관한 기능만 할 뿐, Job의 상태 모니터링이라던가, Job 실행 실패에 관한 정책(재시작 등)에는 전혀 관여하지 않는다. 이 Scheduler의 할당 정책은 User임의로 구현할 수 있으며 `yarn-site.xml`에서 `yarn.resourcemanager.scheduler.class` 를 지정할 수 있다.

### Application Manager
Job submission, Application 실행을 위한 Container 할당을 위해 각 Application Master와의 Negotiation, 실패한 Job의 ApplicationMaster Container 재시작 등을 담당한다.

## Node Manager
각 Machine별 Resource사용(CPU, disk, network, memory)을 Container화하여 관리하고 Monitoring 하며 Resource Manager에게 보고한다.

## Container
Container는 한 Node에 할당된 Resource 덩어리로 볼 수 있다. 각 Application Master가 Resource Manager로부터 Scheduling을 통해 할당받으며, 이 Container내에서 MR task가 동작한다. 참고로 1개의 MR task는 여러 Node에 걸쳐 분산 처리되며, 여러 Container가 하나의 Application을 위해 동작할 수 있다. 할당받은 Container는 각 Node Manager에 의해 Resource 사용에 대해 모니터링 받는다. Container에 할당된 Resource보다 사용량이 클 경우 Exception이 발생하여 Task 진행이 중단될 수 있다.
