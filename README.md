# MicroK8s (standalone)

Bem-vindo ao repositório de instalação do MicroK8s! Este guia é projetado para ajudar iniciantes a se familiarizarem com o Kubernetes usando o MicroK8s.

Não é recomendado seguir com algumas dessas configurações para ambiente produtivo.

Documentação oficial no endereço: https://microk8s.io/docs

## O que é Kubernetes?

Kubernetes é uma plataforma de código aberto usada para automatizar a implantação, o dimensionamento e a operação de aplicações em contêiner. Ele fornece uma estrutura para executar sistemas distribuídos de forma resiliente. Com Kubernetes, você pode garantir que suas aplicações rodem onde e quando você deseja, e pode ajudar a encontrar os recursos e ferramentas de que você precisa para trabalhar com essas aplicações.

## O que é MicroK8s?

MicroK8s é uma distribuição leve e fácil de instalar do Kubernetes, desenvolvida pela Canonical. Ele é ideal para desenvolvedores, operações e IoT. MicroK8s pode ser instalado em um ambiente local, permitindo que você configure e experimente o Kubernetes rapidamente. É uma maneira simples de ter um cluster Kubernetes funcionando sem a complexidade de uma instalação tradicional.

## Requisitos do Sistema Operacional

Para instalar o MicroK8s, você precisará de um sistema operacional compatível. Aqui estão os requisitos mínimos:

- **Sistema Operacional:** Linux (preferencialmente Ubuntu)
- **Arquitetura:** x86_64 ou arm64
- **Memória:** Mínimo de 4 GB de RAM
- **Armazenamento:** Pelo menos 20 GB de espaço em disco livre
- **Privilegios:** Acesso sudo ou root

## Passos para Instalação

### Passo 1: Atualize o Sistema

Antes de instalar o MicroK8s, é recomendável atualizar o sistema para garantir que todos os pacotes estejam atualizados.

```bash
sudo apt update && sudo apt upgrade -y
```

### Passo 2: Instale o MicroK8s
Para instalar o MicroK8s, execute o seguinte comando:

```bash
sudo snap install microk8s --classic
```
Observação: lembre sempre se destacar a versão que será instalada --channel=1.30 - está é a última avaliada no momento da criação desse repositório.

### Passo 3: Verifique a Instalação
Após a instalação, você pode verificar se o MicroK8s está funcionando corretamente:

```bash
microk8s.status --wait-ready
```

### Passo 4: Adicione seu Usuário ao Grupo microk8s
Para evitar a necessidade de usar sudo em todos os comandos do MicroK8s, adicione seu usuário ao grupo microk8s e aplique as permissões corretas:

```bash
sudo usermod -a -G microk8s $USER
mkdir -p ~/.kube
chmod 0700 ~/.kube

su - $USER
```

Depois, saia e entre novamente na sessão do seu usuário.

### Passo 5: Ative Add-ons Necessários
O MicroK8s vem com vários add-ons opcionais que podem ser ativados conforme necessário. Por exemplo, para ativar o DNS e o dashboard do Kubernetes, use:

```bash
microk8s.enable dns dashboard
```

Consulte a lista completa de add-ons disponíveis em [Addons](https://microk8s.io/docs/addons).

O painel do kubernetes é uma forma visual de gerencialmento muito útil para monitorar o seu cluster. Para ativar o painel do kubernetes, use:
```bash
microk8s kubectl -n kube-system edit service kubernetes-dashboard
```

Substitua o campo "type" por NodePort e acrescente a chave "nodePort: 30443" e salve o arquivo. O resultado deverá ser semelhante ao abaixo.
```yaml
  .....
  ports:
  - port: 443
    protocol: TCP
    targetPort: 8443
    nodePort: 30443 #porta exposta    
  selector:
    k8s-app: kubernetes-dashboard
  sessionAffinity: None
  type: NodePort #expor node para acesso remoto
 ......
```

Ao acessar o endereço será solicitado um token valido para acesso, para gerar utilize o comando:
```bash
microk8s kubectl create token default
```

### Passo 6: Valide acesso aos recursos kubernetes
Microk8s utilizada sua própria version do kubectl para acessar o kubernetes. Use esse comando para monitorar ou controlar seu kubernetes. Por exemplo:

```bash
microk8s kubectl get nodes
```

```bash
microk8s kubectl get services --all-namespaces
```

### Passo 7: Deploy de aplicação
Agora que o MicroK8s está instalado e configurado, você pode implantar suas aplicações e serviços. Você pode usar o kubectl para fazer isso como você faria com qualquer Kubernetes. Experimente instalar uma aplicação de demonstração:

```bash
microk8s kubectl create deployment nginx --image=nginx
```

```bash
microk8s kubectl get pods
```
