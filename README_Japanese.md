[![Build Status](https://travis-ci.org/IBM/drupal-on-kubernetes-sample.svg?branch=master)](https://travis-ci.org/IBM/drupal-on-kubernetes-sample)

# Drupal on Kubernetes デプロイについて

In this Code Pattern, we will setup a Drupal site using Kubernetes and Postgres. Drupal is a popular free and open source content management system used as the backend for millions of web sites worldwide. By splitting out the services into containers, we have the ability to leverage the power of Kubernetes.

When the reader has completed this Code Pattern, they will understand how to:

* Configure an app running multiple containers in Kubernetes
* Run a website hosted via Kubernetes
* Use Kubernetes persistent volumes to maintain Drupal configurations between container restarts

![](images/architecture.png)

## 手順のおおまかな流れ

1. User interacts with the Drupal web interface.
2. The Drupal container uses its persistent volume to store website data (but not content).
3. Drupal container connects to PostgreSQL container to access website content.
4. PostgreSQL container uses its persistent volume to store the database contents.

## 含まれているコンポーネント

* [Kubernetes Cluster](https://console.bluemix.net/docs/containers/container_index.html): Create and manage your own cloud infrastructure and use Kubernetes as your container orchestration engine.
* [PostgreSQL](https://www.postgresql.org/): Sophisticated open-source Object-Relational DBMS supporting almost all SQL constructs.

## Featured technologies

* [Cloud](https://www.ibm.com/developerworks/learn/cloud/): Accessing computer and information technology resources through the Internet.

## 解説動画 (英語)

[![](http://img.youtube.com/vi/fQY8q6CjU68/0.jpg)](https://youtu.be/fQY8q6CjU68)

## 手順の流れ

[![Deploy to IBM Cloud](https://bluemix.net/deploy/button.png)](https://bluemix.net/deploy?repository=https://github.com/IBM/drupal-on-kubernetes-sample)

次の手順で Drupal on Kubernetes を実装することができます。

1. [Clone the repo](#1-clone-the-repo)
2. [Create Kubernetes cluster](#2-create-the-kubernetes-cluster)
   1. [Locally](#2.1-locally)
   2. [Hosted on IBM Cloud](#2.2-hosted-on-ibm-cloud)
3. [Create the service and deployment](#3-create-the-service-and-deployment)
4. [Access Drupal](#4-access-drupal)

### 1. リポジトリのクローン

Clone the `drupal-on-kubernetes-sample` locally. In a terminal, run:

```
$ git clone https://github.com/IBM/drupal-on-kubernetes-sample
```

### 2. Kubernetes クラスタの作成

Memo: Minimum version 1.10 is required for both Kubernetes server and kubectl client.

#### 2.1 ローカルで試す場合

Follow the instructions for [running Kubernetes locally via Minikube](https://kubernetes.io/docs/getting-started-guides/minikube/).

#### 2.2 IBM Cloud で試す場合
Follow the instructions for [creating a Kubernetes cluster in IBM Cloud](https://console.bluemix.net/docs/containers/container_index.html#clusters).

### 3. サービスの作成とデプロイ

Either run ['scripts/quickstart.sh'](scripts/quickstart.sh), or run the individual commands listed in it:

```shell
kubectl create -f kubernetes/local-volumes.yaml
kubectl create -f kubernetes/postgres.yaml
kubectl create -f kubernetes/drupal.yaml
```

### 4. Drupal へ アクセス

After deploying, we need to be sure that all pods are running. Check on the status via:

```shell
kubectl get pods -l app=drupal
```

Once all pod are running we need to know the IP adress of our Drupal.

If you are running in Minikube, run the following:

```shell
$ minikube service drupal --url
```

If you are running in IBM Cloud, we will need to run the following:

```shell
$ bx cs workers "$CLUSTER_NAME"
OK
ID                                                 Public IP        Private IP      Machine Type   State    Status
kube-dal13-cr896f6348d71b4fd1ba151bc7c32abd46-w1   <REDACTED>       10.187.85.198   free           normal   Ready
```

Access the newly deployed Drupal site via http://<IP_ADDRESS>:30080

## 参考文献

* **Kubernetes on IBM Cloud**: Deliver your apps with the combined the power of [Kubernetes and Docker on IBM Cloud](https://www.ibm.com/cloud-computing/bluemix/containers)

## License
[Apache 2.0](LICENSE)
