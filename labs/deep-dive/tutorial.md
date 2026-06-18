# Lab Deep Dive - Helm Avançado

## Objetivo

Aplicar práticas de maturidade em um chart real, estabelecendo um **contrato firme** de valores, aplicando **reuso de código** com helpers e realizando a **validação pós-deploy**.

Vamos trabalhar na pasta `advanced-chart`, que é uma evolução do chart que construímos na Sessão 1.

---

## Passos do Laboratório

### 1. Entender o Contrato (JSON Schema)

O Helm suporta um arquivo de schema para validar os dados do `values.yaml` antes de processar qualquer template. 

Abra o arquivo `values.schema.json` na raiz do diretório `advanced-chart` e observe as regras definidas:

```json
{
  "$schema": "http://json-schema.org/schema#",
  "type": "object",
  "required": ["replicaCount", "image", "service"],
  "properties": {
    "replicaCount": {
      "type": "integer",
      "minimum": 1,
      "description": "Número de réplicas do pod. Deve ser maior ou igual a 1."
    },
    "image": {
      "type": "object",
      "required": ["repository", "tag"],
      "properties": {
        "repository": { "type": "string" },
        "tag": { "type": "string" },
        "pullPolicy": {
          "type": "string",
          "enum": ["Always", "IfNotPresent", "Never"]
        }
      }
    },
    "service": {
      "type": "object",
      "required": ["port", "type"],
      "properties": {
        "port": {
          "type": "integer",
          "minimum": 1,
          "maximum": 65535
        },
        "type": {
          "type": "string",
          "enum": ["ClusterIP", "NodePort", "LoadBalancer"]
        }
      }
    }
  }
}
```

### 2. Testar a Validação e Forçar um Erro

Abra o arquivo `advanced-chart/values.yaml` e altere propositalmente o valor de `replicaCount` para uma string (ex: `replicaCount: "dois"`).

Agora, execute a validação:

```bash
helm lint ./advanced-chart
```

**Observe:** O comando deve falhar com um erro claro do JSON Schema informando que o tipo esperado era *integer* e não *string*. Corrija o valor de volta para `1` e rode o lint novamente para ver o sucesso. O `helm template` também será bloqueado por essa validação.

### 3. Evitar Repetição com Helpers

No K8s, repetimos muitas labels. Abra o arquivo `advanced-chart/templates/_helpers.tpl` e adicione este bloco no final para padronizar suas labels de seleção:

```gotemplate
{{/* Selector labels */}}
{{- define "advanced-chart.selectorLabels" -}}
app.kubernetes.io/name: {{ include "advanced-chart.name" . }}
app.kubernetes.io/instance: {{ .Release.Name }}
{{- end }}
```

Em seguida, abra o arquivo `advanced-chart/templates/deployment.yaml` e substitua o bloco `matchLabels` (que tem as labels explícitas) pelo `include`:

```yaml
  selector:
    matchLabels:
      {{- include "advanced-chart.selectorLabels" . | nindent 6 }}
```
Faça o mesmo na raiz de `labels` dentro de `metadata` e `template.metadata`. O `helm template` confirmará que o YAML final continua idêntico, mas agora seu código está DRY (Don't Repeat Yourself).

### 4. Criar um Teste Pós-Deploy

Vamos adicionar um pod efêmero que testa se o Service subiu corretamente. Crie uma pasta `tests` dentro de `templates` (se não existir) e adicione o arquivo `advanced-chart/templates/tests/test-connection.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: "{{ include "advanced-chart.fullname" . }}-test-connection"
  labels:
    {{- include "advanced-chart.selectorLabels" . | nindent 4 }}
  annotations:
    "helm.sh/hook": test
spec:
  containers:
    - name: wget
      image: busybox
      command: ['wget']
      args: ['{{ include "advanced-chart.fullname" . }}:{{ .Values.service.port }}']
  restartPolicy: Never
```

### 5. Executar o Ciclo Completo com Teste

Agora vamos instalar a release com as novas práticas:

```bash
helm upgrade --install workshop-adv ./advanced-chart -n helm-workshop --create-namespace
```

Após o pod da aplicação subir, acione a esteira de validação automática do Helm:

```bash
helm test workshop-adv -n helm-workshop
```

**Observe:** O Helm subirá temporariamente o pod de teste (`busybox`) que tentará bater no seu Service. Se a resposta for 200, ele avisa que o teste **PASSOU**. Você garantiu não apenas que os objetos K8s foram criados, mas que a aplicação está viva e respondendo.
