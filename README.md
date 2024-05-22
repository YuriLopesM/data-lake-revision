# Data Lake Revision 🏞️
Repo to revisit the core conceptions of Data Lake, study for Data Engineer test. 

Based on the [teacher repo](https://github.com/jlsilva01/adls-azure) and other recurses offered in class.

Visit [README-ptbr.md](./README-ptbr.md) to the Portuguese version.

## Table of Contents

- [Pre-requisites](#📚-pre-requisites)
- [Steps](#👣-steps)


## 📚 Pre-requisites

- Docker
- SQL Server Management Studio (SSMS)
- Python/pip, PyEnv, Pipx, Poetry (use the [teacher guide](https://storage.satc.edu.br/arquivos/docentes/4906/20241/files/ED/Python%20ED/Python%20para%20Engenharia%20de%20Dados.pdf) for completed install explication)
- Azure CLI
- Visual Studio Code _(or any editor)_
- Terraform
- Microsoft Learning account (using active Sandbox)
- [Databricks Community](https://community.databricks.com/) Account

## 👣 Steps

##### 1. Database configuration

###### 1.1 Create a Docker container with SQL Server
```bash
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=satc@2023" -p 1433:1433 --name satc-sql-server --hostname satc-sql-server -d mcr.microsoft.com/mssql/server:2022-latest
```

###### 1.2 Connect to SSMS 

```markdown
# Credentials

server name: localhost
user:        sa
password:    satc@2023
```

##### 1.3 Execute the database creation script

In SSMS, execute `create_cars_database.sql` on `scripts` folder

##### 2. Active a test subscription
[MS Learn Sandbox (Restrict Area)](https://learn.microsoft.com/pt-br/training/modules/develop-test-deploy-azure-functions-with-core-tools/5-exercise-publish-function-core-tools?ns-enrollment-type=learningpath&ns-enrollment-id=learn.create-serverless-applications) - Concierge Subscription _(4 hours limit)_ 

##### 3. Azure CLI

###### 3.1 Login

```bash  copy
az login
```

###### 3.2 Get Resource Group name of Concierge Subscription account

```bash copy
az group list -o table
```

##### 4. Adjust `variables.tf` with the result of previous step

```terraform
variable "resource_group_name" {
  default = "learn-9b3e07c7-1725-4464-9b6a-b0fee4cb4c0a"
}
```

##### 5. Create Azure resources
###### 5.1 - Terraform init

```bash
terraform init
```

###### 5.2. Validate code on `.tf` files

```bash
terraform validate
```

###### 5.3. Adjust formatation on `.tf` files

```bash
terraform fmt
```

###### 5.4. Create an execution plan

```bash 
terraform plan
```

###### 5.5. Apply Terraform to cloud

```bash
terraform apply
```

##### 6. Check ADLS deploy

Log in [portal.azure.com](https://portal.azure.com/) and check the created ADLS.

##### 7. Destroy created recurses 

**⚠️ Caution:** Only use this if you will not continue to the next steps ⚠️

```bash
terraform destroy
```

> <b>Note:</b> To use `apply` and `destroy` without confirmation, use `-auto-approve` tag (use for your own risk).

##### 8. Env

###### 8.1. Copy env file
```bash
# Windows
copy .env.example .env

# Linux
cp .env.example .env
```

###### 8.2. Fill the following `.env` variables:


`ADLS_ACCOUNT_NAME=datalakebf6f21398b44ea7b`

> Available in https://portal.azure.com/#view/HubsExtension/BrowseResource/resourceType/Microsoft.Storage%2FStorageAccounts

`ADLS_SAS_TOKEN=sv=2022-11-02&ss=bfqt&srt=sco&sp=rwdlacupyx&se=2024-05-10T03:35:53Z&st=2024-05-09T19:35:53Z&spr=https&sig=SPucZ0LhY4PWAiIyzIhATo5sydM5WPyph%2Bl2javSi9k%3D`

> Within the previously selected storage account, simply access "Shared access signature", fill in all the fields under "Allowed resource types" and click **Generate connection and SAS string**. <br><br> Copy the **SAS Token** created and paste it into this variable

##### 9. Python

###### 9.1 Install dependencies

```bash
cd python

poetry install
```

###### 9.2 Run the N tables script

```bash
poetry run python elt_sql_n_tabelas.py
```

##### 10. Databricks

###### 10.1 Create a new cluster

On the menu item **Compute**, click in **Create compute**. Type a name to your new cluster (no need of more configurations).

###### 10.2 Import the notebooks

On the menu item **Workspace**, click in **Home**. Click in the arrow icon, and select **Import**. 

Individually, import all `.dbc` files under the folder `databricks`.

###### 10.3 Run the notebooks

Run the notebooks. The data has to go through Bronze, Silver and Gold layers, starting on the Landing-zone and applying all the transformations for each step.

Don't forget to, in each notebook, adjust the variables of `storageAccountName` and `sasToken`
