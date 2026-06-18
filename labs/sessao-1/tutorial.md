# Lab Sessão 1 - Ciclo mínimo de Helm

## Objetivo

Executar o fluxo essencial de Helm em um chart simples e observar release, revision e rollback.

## Passos

### 1. Validar chart

```bash
helm lint ./starter-chart
```

Observe:
- o chart precisa estar estruturalmente válido antes de ir para o cluster.

### 2. Renderizar manifests

```bash
helm template workshop ./starter-chart
```

Observe:
- esse comando mostra o YAML final que o Kubernetes receberia.

### 3. Instalar/atualizar release

```bash
helm upgrade --install workshop ./starter-chart -n helm-workshop --create-namespace
```

Observe:
- `upgrade --install` funciona bem para automação porque cria ou atualiza a release.

### 4. Consultar status e histórico

```bash
helm status workshop -n helm-workshop
helm history workshop -n helm-workshop
```

Observe:
- cada alteração relevante gera uma revision.

### 5. Testar a Aplicação

Faça o port-forward para ver a aplicação rodando no seu navegador:

```bash
kubectl port-forward svc/workshop-starter-chart 8080:80 -n helm-workshop
```
Abra o navegador em `http://localhost:8080`. Você verá a cor de fundo padrão!

### 6. Simular mudança visual e rollback

Vamos mudar a cor de fundo do app atualizando apenas o nosso Config via Helm!

```bash
helm upgrade workshop ./starter-chart -n helm-workshop --set appConfig.bgColor="#8b5cf6"
```

Acesse o navegador novamente e veja a cor atualizada sem nenhum downtime perceptível.
Em seguida, confira o histórico e faça o rollback:

```bash
helm history workshop -n helm-workshop
helm rollback workshop 1 -n helm-workshop
helm history workshop -n helm-workshop
```

Observe:
- rollback também cria uma nova revision e retorna à cor anterior.
