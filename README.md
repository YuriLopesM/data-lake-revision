# Data Lake Revision 🏞️
Repo to revisit the core conceptions of Data Lake, study for Data Engineer test. 

Based on the [teacher repo](https://github.com/jlsilva01/adls-azure) and other recurses offered in class.

## Table of Contents

- [Pre-requisites](#pre-requisites)
- [Steps](#steps)
- []()


## Pre-requisites

- Docker
- SQL Server Management Studio (SSMS)
- Azure CLI
- Visual Studio Code _(or any editor)_
- Terraform
- Microsoft Learning account (using active Sandbox)
- [Databricks Community](https://community.databricks.com/) Account

## Steps

##### 1. Database configuration

###### 1.1 Create a Docker container with SQL Server
```bash
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=satc@2023" -p 1433:1433 --name satc-sql-server --hostname satc-sql-server -d mcr.microsoft.com/mssql/server:2022-latest 5750e36a686db1ba82783d97975c061aef10c776eb8463cb93e0128b9a37fff5
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
  default = "learn-877e311a-66ab-401b-9372-06326c9bd083"
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

**⚠️ Caution:** Only if you will not continue to the next steps ⚠️

```bash
terraform destroy
```

> <b>Note:</b> To use `apply` and `destroy` without confirmation, use `-auto-approve` tag (use for your own risk).