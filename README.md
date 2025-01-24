## Desafio SRE - Elo

## Tabela de Conteúdos

- [Introdução](#introdução)
- [Requisitos](#requisitos)
- [Configure o ambiente do desafio](#configure-o-ambiente-do-desafio)
- [Uso](#uso)
- [Documentação](#documentação)
- [Contribuição](#contribuição)
- [Licença](#licença)

## Introdução

Este repositório contém o desafio de SRE proposto pela Elo. O objetivo é testar habilidades de infraestrutura, automação e escalabilidade de aplicações utilizando ferramentas como Docker, Kubernetes e Artillery para testes de carga.

## Requisitos

Antes de começar, certifique-se de ter os seguintes requisitos instalados:

- [Docker](https://docs.docker.com/get-docker/)
- [Kubernetes](https://kubernetes.io/docs/home/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [Artillery](https://www.artillery.io/docs)

## Configure o ambiente do desafio

### Clone o repositório:

```bash
git clone https://github.com/edemirtoldo/sre-challenge-elo.git
cd sre-challenge-elo
```

## Parte 1 - Configuração dos aplicativos

A aplicação sre-challenge-app e seu banco de dados executados em um cluster K8s.

Requisitos:

1. A aplicação deve ser acessível de fora do cluster.
2. Manifestos de implantação do kubernetes para executar com limitação de requests e usando HPA.

## Uso

### Rodando a aplicação no Docker Compose

A aplicação pode ser iniciada localmente utilizando Docker Compose. Para mais informações, consulte a [documentação](docs/docker-compose.md).

### Implantacão no Minikube

Para implantar a aplicação no Minikube, siga as instruções detalhadas na [documentação](docs/minikube.md).

### Testes de Carga com Artillery

Os testes de carga são realizados com Artillery para verificar o comportamento da aplicação sob carga. Consulte a [documentação](docs/hpa-test.md) para instruções detalhadas.

### Remover a infraestrutura do K8s

Apos todos os passos acima voce pode remover toda a infraestrutura criada no Minikube. Consulte a [documentação](docs/del-k8s.md) para instruções detalhadas.

## Parte 2 - Corrigir o problema

## Parte 3 - Melhores práticas

1. Manifesto do Kubernetes para a Secret do MySQL

- Para armazenar as credenciais do MySQL de forma segura, criamos uma Secret do Kubernetes.

Nota: As credenciais devem ser codificadas em Base64 antes de serem armazenadas.

2. Manifesto do Kubernetes para a aplicação utilizando a Secret

- As credenciais foram injetadas via Secret.

3. Configuração do código da aplicação no Application Properties (Spring Boot)

- Baseado no arquivo application.properties do Spring Boot, foi possivel configurar as variáveis de ambiente injetadas pelo Kubernetes.

## Parte 4 - Perguntas

### 1. O que você faria para melhorar essa configuração e torná-la “pronta para produção”?

Para tornar a configuração pronta para produção, eu implementaria as seguintes melhorias:

Segurança:

- Usar Vault (HashiCorp) ou AWS Secrets Manager para armazenamento dinâmico de credenciais.
- Configurar RBAC (Role-Based Access Control) para restringir o acesso às secrets apenas para pods autorizados.
- Configurar NetworkPolicies para restringir comunicação entre pods.
- Habilitar TLS para comunicação entre os serviços e o banco de dados.
- Monitoramento e Observabilidade:

  - Configurar Prometheus e Grafana para monitorar métricas de aplicação e banco de dados.
  - Implementar liveness e readiness probes para garantir a disponibilidade do serviço.
  - Utilizar centralização de logs com ELK Stack ou Loki.

- Alta Disponibilidade e Escalabilidade:

  - Habilitar HPA (Horizontal Pod Autoscaler) para escalar a aplicação conforme a carga.
  - Configurar ReplicaSets para o banco de dados MySQL, garantindo redundância.
  - Usar Persistent Volumes com armazenamento adequado para dados críticos.

- Ciclo de Vida da Aplicação:
  - Automatizar deploys com GitOps (ArgoCD) ou pipelines CI/CD.
  - Usar Helm Charts para gerenciar configurações complexas.

### 2. Existem 2 microsserviços mantidos por 2 equipes diferentes. Cada equipe deve ter acesso apenas ao seu serviço dentro do cluster. Como você abordaria isso?

Para garantir isolamento entre os serviços, adotaria as seguintes práticas:

#### Namespaces:

- Criar namespaces separados para cada equipe.

#### RBAC (Role-Based Access Control):

- Criar regras de permissão específicas para que cada equipe acesse somente seus recursos.

- Vincular um usuário ou grupo à Role usando RoleBinding.

#### NetworkPolicies:

- Configurar Network Policies para restringir a comunicação entre namespaces e serviços.

#### Separate Service Accounts:

- Criar contas de serviço distintas para cada equipe, garantindo isolamento por autenticação.

### 3. Como você evitaria que outros serviços em execução no cluster se comunicassem com o sre-challenge-app?

Para impedir que outros serviços se comuniquem com o sre-challenge-app, seguiria estas práticas:

#### NetworkPolicy para restringir o tráfego:

Criaria uma política de rede permitindo apenas o acesso de serviços autorizados:

#### Isolamento de Namespace:

Executar a aplicação em um namespace isolado e configurar regras para que apenas serviços internos possam se comunicar.

#### RBAC para restringir acessos:

Criar políticas RBAC para garantir que apenas determinados serviços tenham acesso ao sre-challenge-app.

#### Uso de Service Mesh:

Implementar um service mesh como Istio para controlar o tráfego e definir regras de segurança.

## Documentação

A documentação detalhada do projeto está disponível nos arquivos Markdown dentro da pasta `docs`. Eles incluem guias para:

- [Execução via Docker Compose](docs/docker-compose.md)
- [Implantação no Minikube](docs/minikube.md)
- [Testes de HPA com Artillery](docs/hpa-test.md)
- [Remover a infraestrutura do K8s](docs/del-k8s.md)

## Contribuição

Contribuições são bem-vindas! Sinta-se à vontade para abrir issues e pull requests.

1. Faça um fork do repositório
2. Crie uma branch para sua feature (`git checkout -b minha-feature`)
3. Commit suas alterações (`git commit -m 'Adicionando nova feature'`)
4. Envie para o repositório remoto (`git push origin minha-feature`)
5. Abra um Pull Request

## Licença

Este projeto é licenciado sob a licença MIT. Consulte o arquivo [LICENSE](LICENSE) para mais informações.
