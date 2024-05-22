# Revisão Data Lake 🏞️
Repositório para revisitar os conceitos principais de Data Lake, estudando para a prova de Engenheiro de Dados.

Baseado no [repositório do professor](https://github.com/jlsilva01/adls-azure) e outros recursos oferecidos em sala.

## Table of Contents

- [Pré-requisitos](#📚-pré-requisitos)
- [Passos](#👣-passos)


## 📚 Pré-requisitos

- Docker
- SQL Server Management Studio (SSMS)
- Python/pip, PyEnv, Pipx, Poetry (use o [guia do professor](https://storage.satc.edu.br/arquivos/docentes/4906/20241/files/ED/Python%20ED/Python%20para%20Engenharia%20de%20Dados.pdf) para uma explicação completa da instalações)
- Azure CLI
- Visual Studio Code _(ou qualquer editor)_
- Terraform
- Conta da Microsoft Learning (utilizando o Active Sandbox)
- Conta no [Databricks Community](https://community.databricks.com/)

## 👣 Passos

##### 1. Configuração do Banco de Dados

###### 1.1 Criar um contâiner no Docker com SQL Server
```bash
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=satc@2023" -p 1433:1433 --name satc-sql-server --hostname satc-sql-server -d mcr.microsoft.com/mssql/server:2022-latest
```

###### 1.2 Conectar no SSMS 

```markdown
# Credenciais

nome do servidor: localhost
usuário:          sa
senha:            satc@2023
```

##### 1.3 Execute o script de criação do banco

Dentro do SSMS, execute o arquivo `create_cars_database.sql` na pasta `scripts`.

##### 2. Ative uma assinatura de teste
[MS Learn Sandbox (Área Restrita)](https://learn.microsoft.com/pt-br/training/modules/develop-test-deploy-azure-functions-with-core-tools/5-exercise-publish-function-core-tools?ns-enrollment-type=learningpath&ns-enrollment-id=learn.create-serverless-applications) - Concierge Subscription _(4 horas de limite)_ 

##### 3. Azure CLI

###### 3.1 Login

```bash  copy
az login
```

###### 3.2 Consultar o nome do Resource Group criado para a sua conta do Concierge Subscription

```bash copy
az group list -o table
```

##### 4. Ajustar o arquivo `variables.tf` com o resultado do passo anterior

```terraform
variable "resource_group_name" {
  default = "learn-9b3e07c7-1725-4464-9b6a-b0fee4cb4c0a"
}
```

##### 5. Criar recursos na Azure
###### 5.1 - Terraform init

```bash
terraform init
```

###### 5.2. Validar código em arquivos com `.tf`

```bash
terraform validate
```

###### 5.3. Ajustar formatação em arquivos com `.tf`

```bash
terraform fmt
```

###### 5.4. Cria um plano de execução

```bash 
terraform plan
```

###### 5.5. Aplicar o Terraform na núvem

```bash
terraform apply
```

##### 6. Checar o deploy do ADLS

Entrar em [portal.azure.com](https://portal.azure.com/) e checar o ADLS criado.

##### 7. Destruir recursos criados

**⚠️ Cuidado:** Use apenas se você não quiser continuar com as próximas etapas ⚠️

```bash
terraform destroy
```

> <b>Nota:</b> Para usar `apply` e `destroy` sem confirmação, use a tag `-auto-approve` (use pelo seu próprio risco).

##### 8. Env

###### 8.1. Copiar arquivo env
```bash
# Windows
copy .env.example .env

# Linux
cp .env.example .env
```

###### 8.2. Preencha as seguintes variáveis no `.env`:


`ADLS_ACCOUNT_NAME=datalakebf6f21398b44ea7b`

> Disponível em https://portal.azure.com/#view/HubsExtension/BrowseResource/resourceType/Microsoft.Storage%2FStorageAccounts

`ADLS_SAS_TOKEN=sv=2022-11-02&ss=bfqt&srt=sco&sp=rwdlacupyx&se=2024-05-10T03:35:53Z&st=2024-05-09T19:35:53Z&spr=https&sig=SPucZ0LhY4PWAiIyzIhATo5sydM5WPyph%2Bl2javSi9k%3D`

> Dentro da conta de armazenamento selecionada anteriormente, basta acessar "Assinatura de acesso compartilhado", preencha todos os campos em "Tipos de recurso permitidos" e clique em **Gerar a cadeia de conexão e de SAS**. <br><br> Copie o **Token SAS** criado e cole nessa variável.

##### 9. Python

###### 9.1 Instalar dependências

```bash
cd python

poetry install
```

###### 9.2 Execute o script para N tabelas

```bash
poetry run python elt_sql_n_tabelas.py
```

##### 10. Databricks

###### 10.1 Crie um novo cluster

Dentro de **Compute**, clique em **Create compute**. Digite o nome do seu novo cluster (não precisa de mais configurações).

###### 10.2 Importar os notebooks

Dentro de **Workspace**, clique em **Home**. Clique no ícone de flecha, e selecione **Import**. 

Individualmente, importe todos os arquivos com `.dbc` dentro da pasta `databricks`.

###### 10.3 Execute os notebooks

Execute os notebooks. O dado deve navegar entre as camadas Bronze, Prata e Ouro, começando na Landing-zone e aplicando todas as transformações para cada passo.