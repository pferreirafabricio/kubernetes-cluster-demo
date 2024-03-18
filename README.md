<h1 align="right">
  <img src="https://blog.geekhunter.com.br/wp-content/uploads/2019/04/how-to-setup-webserver-on-Kubernetes-750x410-1.png" width="200px" align="left" />
  Kubernetes cluster demo
</h1>

<p align="right">
🐋 This project provides a practical and hands-on approach to deploying and managing applications using Kubernetes. It includes YAML configuration files for deploying MySQL and Apache servers as Kubernetes pods.
  <br><br>
  <!-- License -->
  <a>
    <img alt="license url" src="https://img.shields.io/badge/license%20-MIT-1C1E26?style=for-the-badge&labelColor=1C1E26&color=2E76E1">
  </a>
</p>
<br>

## :open_book: About

The [pod-apache.yml](./pod-apache.yml) and [pod-mysql.yml](./pod-mysql.yml) files are Kubernetes Pod configurations for Apache and MySQL servers respectively. These files define the containers to run within the pods, including the Docker images to use and the names of the containers.

> [!WARNING]
> The file [pod-mysql.yml](./pod-mysql.yml) is broken on purpose and doesn't provide any `env` configuration for MySQL.

The [deploy-mysql.yml](./deploy-mysql.yml) file contains the configuration for a Kubernetes Deployment of a MySQL server. It specifies the Docker image to use, and sets up labels and selectors for managing the pods.

The [docs/explanation.pt-BR.md](./docs/explanation.pt-BR.md) file provides a comprehensive explanation of Kubernetes, containers, and the differences between containers and virtualization. It also covers the architecture of Kubernetes, including the Control Plane and Compute Nodes, and explains the roles of various components such as the kube-apiserver, kube-scheduler, kube-controller-manager, and etcd. The document also introduces Minikube, a tool for running Kubernetes locally, and kubectl, the standard command-line tool for interacting with Kubernetes.

> [!NOTE]
> This project is a resource for me to help me understand Kubernetes and its use in deploying and managing containerized applications.

## :bricks: This project was built with

- [minikube](https://minikube.sigs.k8s.io/docs/)
- [wsl](https://learn.microsoft.com/en-us/windows/wsl/about)

## 📚 Learn more

- [Kubernetes Para Quem Não Entende Nada - Aula 00](https://www.youtube.com/watch?v=2kZRM-KHK0k)
- [Kubernetes Para Quem Não Entende Nada - Aula 01](https://www.youtube.com/watch?v=XLHtP-27q-o)
