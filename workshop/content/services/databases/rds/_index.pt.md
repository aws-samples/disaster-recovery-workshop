---
title: "Amazon RDS"
menutitle: "Amazon RDS"
weight: 1
chapter: true
pre: "<b></b>"
awsServices: "Amazon RDS"
---

O Amazon Relational Database Service (Amazon RDS) é um web service que facilita a configuração, a operação e o dimensionamento de um banco de dados relacional na Nuvem AWS. Ele fornece capacidade econômica e redimensionável para um banco de dados relacional padrão do setor e gerencia tarefas comuns de administração de banco de dados.

**Instâncias de banco de dados**

O bloco de construção básico do Amazon RDS é a instância do banco de dados. Uma instância de banco de dados é um ambiente de banco de dados isolado na Nuvem AWS. A instância de banco de dados pode conter vários bancos de dados criados pelo usuário. É possível acessar a instância de banco de dados usando as mesmas ferramentas e os mesmos aplicativos usados com uma instância de banco de dados independente. Crie e modifique uma instância de banco de dados usando a AWS Command Line Interface, a API do Amazon RDS ou o Console de gerenciamento da AWS.

Cada instância de banco de dados executa um mecanismo de banco de dados. Atualmente, o Amazon RDS oferece suporte aos mecanismos de banco de dados MySQL, MariaDB, PostgreSQL, Oracle e Microsoft SQL Server. Cada mecanismo de banco de dados tem seus próprios recursos com suporte, e cada versão de um mecanismo de banco de dados pode incluir recursos específicos. Além disso, cada mecanismo de banco de dados tem um conjunto de parâmetros em um parameter group de banco de dados que controlam o comportamento dos bancos de dados que ele gerencia. 

Para mais informações consulte a [documentação oficial](https://docs.aws.amazon.com/pt_br/AmazonRDS/latest/UserGuide/Welcome.html).

---
**Referências:**
- [Relational databases: Performance,
scale, and availability](https://d1.awsstatic.com/events/reinvent/2019/Relational_databases_Performance,_scale,_and_availability_FSI309.pdf)