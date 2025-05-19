# README.md

## Projeto Odontoprev – CI/CD com Azure DevOps

Este documento descreve como executar e validar a Pipeline de Integração Contínua (CI) e Entrega Contínua (CD) para a aplicação **Odontoprev**.

---

### 1. Pré-requisitos

* Conta no **Azure DevOps** com permissão de execução de pipelines.
* **Service Connection** configurada (`MyAzureSubscription`) apontando para sua subscription Azure.
* Acesso ao repositório Git contendo o código-fonte e o arquivo `azure-pipelines.yml` na branch **master**.
* **Azure CLI** instalada localmente para validação manual (opcional):

  ```bash
  az --version
  ```

---

### 2. Estrutura do Repositório

```
/ (root)
├─ azure-pipelines.yml       # Definição da Pipeline (stages: Infra, BuildApp, DeployApp)
├─ build.gradle              # Configuração Gradle
├─ settings.gradle           # rootProject.name = 'odontoprev'
├─ src/                      # Código-fonte Java Spring Boot
└─ README.md (este arquivo)
```

---

### 3. Configuração de Variáveis (opcional)

Caso precise sobrescrever variáveis do pipeline, crie ou atualize um **Variable Group** no Azure DevOps:

* Nome: **Odontoprev-Variables**
* Secretes:

  * `oracleConnectionString`
  * `rabbitMqConnectionString`

No pipeline, vá em **Edit** > **Variables** > **Variable Groups** e vincule **Odontoprev-Variables**.

---

### 4. Execução da Pipeline

1. Acesse **Pipelines** → **Pipelines** no projeto **Odontoprev**.
2. Clique em **Run pipeline**.
3. Selecione a branch **master** e clique em **Run**.

A pipeline possui três estágios:

| Stage     | Objetivo                                                                                      |
| --------- | --------------------------------------------------------------------------------------------- |
| Infra     | Provisionar Resource Group, App Service Plan e Web App no Azure                               |
| BuildApp  | Executar `gradlew clean build`, rodar testes unitários e publicar o JAR (artefato `drop`)     |
| DeployApp | Baixar artefato, fazer deploy no App Service, aplicar App Settings via Azure CLI e smoke test |

---

### 5. Validação e Testes

#### 5.1 Logs de Execução

* Cada stage apresenta logs detalhados:

  * **Infra**: comandos `az group create`, `az appservice plan create` e `az webapp create`.
  * **BuildApp**: saída do Gradle e resultados de testes JUnit.
  * **DeployApp**: status do ZIP Deploy e output do comando `az webapp config appsettings set`.

#### 5.2 Smoke Test

* Ao final do deploy, a pipeline executa:

  ```bash
  curl -f https://<APP_NAME>.azurewebsites.net/actuator/health
  ```
* **Expectativa**: retorno HTTP 200 e JSON de health

#### 5.3 Testes Manuais

1. Acesse a URL da aplicação (log final ou Pipeline > Job > **Browse**).
2. Faça login com:

   * Usuário: `admin@odontoprev.com`
   * Senha: `admin123`
3. Navegue até **Agendar Consulta** e cadastre uma nova data/hora.
4. Verifique em **Listar Consultas** o registro criado.
5. Cheque mensagens na fila `consulta.agendada.queue` via RabbitMQ UI (se configurado).

---

