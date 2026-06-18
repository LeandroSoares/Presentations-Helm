# 🚢 Presentations-Helm

Bem-vindo ao repositório oficial da apresentação **Introdução ao Helm**! Este projeto contém não apenas os slides interativos da palestra, mas também todo o código-fonte, infraestrutura e guias para rodar os laboratórios práticos demonstrados.

🔗 **[Acessar a Apresentação Online](https://leandrosoares.github.io/Presentations-Helm/)**

---

## 🎯 Objetivo

Desmistificar o uso do Helm no ecossistema Kubernetes, ensinando na prática como empacotar, parametrizar, versionar e distribuir aplicações de forma rápida, auditável e eficiente utilizando Charts.

## 📂 Estrutura do Repositório

- **`apps/`**: Código-fonte das aplicações web de demonstração.
- **`labs/`**: Guias passo a passo e arquivos de base para os workshops práticos realizados durante a apresentação.
  - **`sessao-1/`**: Laboratório introdutório focado em criar um Chart do zero, gerenciar templates de Kubernetes (Ingress, Deployment, ConfigMap), lidar com injeção de variáveis via `values.yaml`, realizar Upgrades de release e testar Rollbacks em tempo real.
  - **`sessao-2/`**: Laboratório avançado abordando boas práticas com dependências (Subcharts), uso de repositórios externos, funções avançadas de template (`helpers.tpl`, controle de fluxo) e publicação do Chart.
- **`.github/workflows/`**: Pipelines de automação CI/CD (GitHub Actions) responsáveis pelo build automático das imagens Docker do projeto para o Github Container Registry (GHCR) e publicação dos slides no GitHub Pages.

## 💻 Pré-requisitos para os Laboratórios

Se você quiser reproduzir os laboratórios da apresentação no seu próprio ambiente local ou em uma VPS, você precisará de:

1. Um cluster **Kubernetes** saudável e rodando (ex: Minikube, Kind, k3s, ou qualquer provedor Cloud).
2. O utilitário **`kubectl`** instalado e devidamente autenticado com o seu cluster.
3. O gerenciador de pacotes **[Helm](https://helm.sh/docs/intro/install/)** (versão 3 ou superior) instalado.

### 🚀 Como Começar
Acesse a pasta do primeiro laboratório e siga as instruções contidas no tutorial:

```bash
cd labs/sessao-1
cat tutorial.md
```

---

> Desenvolvido por **Leandro Soares** para sessões de nivelamento e capacitação em Engenharia de Plataforma e DevOps.