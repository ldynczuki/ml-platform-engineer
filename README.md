# Machine Learning Platform Engineer

O projeto consiste em implementar uma arquitetura completa que consome a Punk Api no
[endpoint](https://api.punkapi.com/v2/beers/random) e ingere em um Kinesis Stream que terá 2 consumidores.



Tabela de conteúdos
=================
<!--ts-->
   * [Sobre](#sobre)
   * [Arquitetura](#arquitetura)
   * [Pré-Requisitos](#pre-requisitos)
   * [Instalação](#instalacao)
   * [Como executar o projeto](#executar-terraform)
   * [Tecnologias](#tecnologias)
   * [Autor](#autor)
   * [Licença](#licenca)
   * [Referências](#referencias)
<!--te-->

<h4 align="center"> 
	🚧 🚀 Em construção...  🚧
</h4>


### Features

- [ ] Criação dos scripts Terraform 
- [x] Levantamento da arquitetura


# <a name="sobre"><a/> 🏢 Sobre

O presente projeto tem como objetivo implementar uma arquitetura completa que consome a [Punk API](https://punkapi.com/) no endpoint
https://api.punkapi.com/v2/beers/random e ingere em um Kinesis Stream que terá 2 consumidores. 

Para isso você será necessário configurar:

   1. Um CloudWatch Event que dispara a cada 5 minutos uma função Lambda para alimentar o Kinesis Stream que terá como saída:
      * Um Firehose agregando todas as entradas para guardar em um bucket S3 com o nome de `raw`.

      * Outro Firehose com um Data Transformation que pega somente os `id`, `name`, `abv`, `ibu`, `target_fg`, `target_og`, `ebc`, `srm` e `ph` das cervejas e guarda em um outro bucket S3 com o nome de `cleaned` em formato **csv**.

   2. Crie uma tabela com os dados do bucket `cleaned`.

   3. Com base nos dados da tabela `cleaned`, treine um modelo de machine learning que classifique as cervejas em seus respectivos ibus.

   4. O Amazon SageMaker será utilizado para integrar o modelo de machine learning à presente arquitetura.


# <a name="arquitetura"><a/> 🏢 Arquitetura

<h1 align="center">
  <img alt="Arquitetura" title="Arquitetura" src="./imagens/arquitetura.png" />
</h1>



# <a name="pre-requisitos"><a/> ☑️ Pré-Requisitos


#### Criar conta na AWS
   * Antes de começar, você vai precisar ter uma conta na AWS, para isso acesse [AWS Console](https://aws.amazon.com/).

#### Criação de usuário/grupo no AWS IAM

   1. Entre no console da AWS e pesquise pelo serviço **IAM**;
   2. No menu à esquerda clique em "users";
   3. Clique no botão "add user";
      3.1. Insira um nome para o usuário, no meu caso foi `admin`;
      3.2. Em `Select AWS access type` marque a primeira caixa `Programmatic access Enables an access key ID and secret access key for the AWS API, CLI, SDK, and other development tools.`;
      3.3. Clique em `Next:Permissions`;
      3.4. Caso não tenha nenhum grupo já criado, clique em `create group`;
      3.5. Na janela que abrir, insira um nome para o grupo, no meu caso foi `admin_group`;
      3.6. Em `Filter Policies` marque a opção `AdministratorAccess` e clique em `Create group`;
      3.7. Clique em `Next: Tags`;
      3.8. Em `Add tags (optional)` não é necessário nenhum procedimento, apenas clique em `Next: Review`;
      3.9. Será apresentado um sumário do usuário e grupo que serão criados, confira as informações e se estiverem de acordo com o desejado clique em `Create user`.


#### Criação de Acess key

Após criado o usuário no passo anterior, realize as seguintes etapas para criar as `acess_key`:

   1. Entre no console da AWS e pesquise pelo serviço **IAM**;
   2. No menu à esquerda clique em "users";
   3. Clique no usuário que você criou;
   4. Na janela que abrir, clique em `Security credentials`;
      4.1. Clique em `Create acess key`;
      4.2. As chaves de acesso serão geradas e deverá clicar para salvar o arquivo, pois a secret não será apresentada novamente;



# <a name="instalacao"><a/> 👨‍💻 Instalação

#### Instalação e Configuração do AWS CLI

   1. Neste projeto, estou utilizando o sistema operacional Linux. Utilize o seguinte roteiro para a instalação [instalação AWS CLI](https://linuxhint.com/install_aws_cli_ubuntu/)
   2. Com o AWS CLI instalado, você deverá configurar suas credenciais:
   ```bash
   # Execute o comando abaixo para iniciar a configuração
   $ aws configure
   ```
      2.1. Insira a `Acess Key` e tecle Enter;
      2.2. Insira a `Secret Key` e tecle Enter;
      2.3. Insira o código da região, no meu caso é `sa-east-1`;
      2.4. No valor formato de saída, pode deixar `None` e tecle Enter.

#### Instalação e Configuração do Terraform

   1. Clique no link para baixar o Terraform de acordo com seu sistema operacional [download Terraform](https://www.terraform.io/downloads.html):
      1.1. No meu caso, estou utilizando Linux 64-bit, após clicar no link um arquivo será baixado.
   2. Descompacte o arquivo e execute os comandos abaixo: 
      2.1. [Roteiro de Instalação](https://learn.hashicorp.com/tutorials/terraform/install-cli?in=terraform/aws-get-started)
      ```bash
      $ echo $PATH

      # Mova o arquivo terraform para o resultado do echo $PATH no comando anterior
      $ mv ~/Downloads/terraform /usr/local/bin/

      # Verifique a instalação do Terraform
      $ terraform -help
      ```

# <a name="executar-terraform"><a/> 🚀 Como executar o projeto (Terraform)

Navegue até o diretório onde os scripts terraform estão para executar os passos abaixo:

```bash
# Inicialize o projeto.
$ terraform init

# Exibe um plano dos serviços que serão criados.
$ terraform plan

# Caso queira salvar o plano de execução dos serviços, substitua "path" por um caminho válido.
$ terraform plan -out=path 

# Realiza a criação dos serviços nos scripts extensão .tf. 
# Quando o Terraform solicitar que você confirme, digite yes e pressione Enter.
$ terraform apply

# Para excluir os serviços, execute terraform destroy.
$ terraform destroy
```


# <a name="tecnologias"><a/> 🛠 Tecnologias

As seguintes linguagens foram usadas na construção do projeto:

- [Python](https://www.python.org/)
- [Terraform](https://www.terraform.io/)

#### Serverless

- [AWS Lambda](https://aws.amazon.com/en/lambda/)
- [Amazon Kinesis](https://aws.amazon.com/en/kinesis/)
- [Amazon Kinesis Data Firehose](https://aws.amazon.com/en/kinesis/data-firehose/)
- [AWS Glue](https://aws.amazon.com/pt/glue/)

#### Plataforma de Machine Learning

- [Amazon SageMaker](https://aws.amazon.com/en/sagemaker/)

#### Scheduler e monitoramento de serviços

- [Amazon Cloudwatch](https://aws.amazon.com/pt/cloudwatch/)


# <a name="autor"><a/> 🤓 Autor

Lucas Dynczuki

Entre em contato! 💚

[![Linkedin Badge](https://img.shields.io/badge/-Lucas-blue?style=flat-square&logo=Linkedin&logoColor=white&link=https://www.linkedin.com/in/lucasdynczuki/)](https://www.linkedin.com/in/lucasdynczuki/) 
[![Outlook Badge](https://img.shields.io/badge/-lucas.dynczuki@outlook.com-blue?style=flat-square&logo=Outlook&logoColor=white&link=mailto:lucas.dynczuki@outlook.com)](mailto:lucas.dynczuki@outlook.com)


# <a name="licenca"><a/>  📝 Licença

Este projeto esta sobe a licença [MIT](./LICENSE).
[![GitHub license](https://img.shields.io/github/license/ldynczuki/MLPlatformEngineer)](https://github.com/ldynczuki/MLPlatformEngineer/blob/main/LICENSE)



# <a name="referencias"><a/>  📚 Referências

https://aws.amazon.com/en/
https://aws.amazon.com/en/cli/
https://aws.amazon.com/en/lambda/
https://aws.amazon.com/en/kinesis/data-streams/
https://aws.amazon.com/en/kinesis/data-firehose/
https://aws.amazon.com/en/glue/
https://aws.amazon.com/pt/cloudwatch/
https://aws.amazon.com/en/sagemaker/
https://www.terraform.io/downloads.html
https://learn.hashicorp.com/tutorials/terraform/install-cli?in=terraform/aws-get-started
https://learn.hashicorp.com/tutorials/terraform/aws-build?in=terraform/aws-get-started
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lambda_function
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/kinesis_stream
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/kinesis_firehose_delivery_stream
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/glue_catalog_database
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/glue_catalog_table
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_event_rule
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_event_target
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_policy
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment
https://linuxhint.com/install_aws_cli_ubuntu/