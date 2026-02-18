# IaC-Alura

Repositório de estudo baseado no curso **“Infraestrutura como código: preparando máquinas na AWS com Ansible e Terraform”** (Alura).

A ideia aqui é treinar o fluxo clássico de IaC:

- **Terraform**: provisiona infraestrutura na AWS (uma instância EC2 Ubuntu).
- **Ansible**: configura a máquina depois de criada (ex.: criar um HTML e subir um servidor HTTP simples).

> Este projeto é intencionalmente simples: o foco é acompanhar as aulas e praticar a integração Terraform + Ansible.

---

## O que este repo faz hoje

- Cria uma **instância EC2** do tipo `t2.micro` na região `us-west-2`.
- Busca uma AMI **Ubuntu 24.04** (mais recente) via `data "aws_ami"`.
- Usa um inventário do Ansible com um grupo chamado `terraform-ansible`.
- Executa um playbook que:
  - cria `/home/ubuntu/index.html`
  - sobe um servidor web simples na porta `8080` (via `busybox httpd`)
  - (rascunho) tem tarefas para instalar Python/virtualenv e iniciar um projeto Django

---

## Estrutura do repositório

- `terraform.tf` — definição do provider AWS + data source da AMI + criação da instância EC2
- `hosts.yml` — inventário do Ansible com o IP público da instância
- `playbook.yml` — tarefas de configuração aplicadas na instância

---

## Pré-requisitos

- Terraform instalado (>= 1.2)
- Ansible instalado
- Conta na AWS com credenciais configuradas (via variáveis de ambiente ou AWS CLI)
- Uma **Key Pair** existente na AWS com o nome configurado em `key_name` (no código atual: `iac-alura`)
- Liberação de rede (Security Group) para:
  - **SSH (22)** a partir do seu IP
  - **TCP 8080** (se você quiser acessar o servidor HTTP pelo navegador)

Referências oficiais:
- Terraform AWS provider / EC2:
  - https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance
  - https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/ami
- Credenciais no Terraform (AWS):
  - https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-create
  - https://docs.aws.amazon.com/cli/v1/userguide/cli-configure-envvars.html
- Ansible (inventário e módulos):
  - https://docs.ansible.com/projects/ansible/latest/inventory_guide/intro_inventory.html
  - https://docs.ansible.com/projects/ansible/latest/collections/ansible/builtin/copy_module.html
