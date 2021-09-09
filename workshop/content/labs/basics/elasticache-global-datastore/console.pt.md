---
hidden: true
draft: true
---

#### Crie uma tabela do Amazon DynamoDB

1. Acesse a console do AWS DynamoDB: [https://console.aws.amazon.com/dynamodb](https://console.aws.amazon.com/dynamodb).
   <br>Para este exemplo, escolha a região **us-east-2 (Leste dos EUA (Ohio))**.
2. No painel de navegação, no lado esquerdo do console, selecione **Tables (Tabelas)**.
3. Escolha **Create Table (Criar tabela)**.
   <br>Em **Table name (Nome da tabela)**, insira **Music**.
   <br>Em **Partition key (Partição primária)** insira **Artist**. Em **Sort key (Chave de classificação)** e insira **Song**. (Artist e Song devem ser strings). ![Create Table](/images/dynamodb-create-table.png?classes=shadow")
4. Para criar a tabela, selecione **Create table (Criar tabela)**. Essa tabela serve como a primeira tabela-réplica em uma nova tabela global. Ela é o protótipo de outras tabelas-réplica que serão criadas posteriormente.


#### Crie uma réplica da tabela em outra região AWS
1. Após a criação da tabela no passo anterior, selecione a tabela **Music**. Acesse a aba **Global Tables (Tabelas globais)** e selecione **Create replica (Criar réplica)**.
2. Em **Available replication Regions (Regiões de replicação disponíveis)**, selecione **Oeste dos EUA (Oregon)**. ![Create Replica](/images/dynamodb-create-replica-oregon.png?classes=shadow")
   <br>Selecione **Create Replica (Criar réplica)**, isso inicia o processo de criação da tabela em **Oeste dos EUA (Oregon)**.
   <br>A guia **Global Table (Tabela global)** da tabela selecionada (e de qualquer outra tabela-réplica) mostra que a tabela foi replicada e suas regiões.  
   <br>O console verifica que existem tabelas com o mesmo nome em outras regiões. Se existir uma tabela com o mesmo nome, será necessário excluir a tabela existente para criar outra tabela-réplica nessa região.
   ![Global Tables List](/images/dynamodb-global-tables-list.png?classes=shadow")

#### Insira dados de exemplo

1. Você ainda deve estar usando o Console de gerenciamento da AWS na região **us-east-2 (Leste dos EUA (Ohio))**. Para a tabela **Music**, selecione a guia **Items (Itens)** e escolha **Create Item (Criar item)**. Em **Artist**, insira **John Lennon**. Em Song, insira Song **Imagine**. 
   <br>Para gravar o item, escolha **Create item (Criar item)**. ![Create Item](/images/dynamodb-create-item-1.png?classes=shadow")
2. O item será replicado para as outras regiões com tabelas réplicas. Para confirmar isso, no seletor de regiões no canto superior direito do console, selecione **Oeste dos EUA (Oregon)**. 
   Selecione a tabela **Music** e na aba items verifique se o item criado foi replicado.
   ![Item replicated](/images/dynamodb-item-replicated.png?classes=shadow")

#### (Opcional) Crie mais items através da tabela réplica

1. Você ainda deve estar usando o Console de gerenciamento da AWS na região da réplica **(Oeste dos EUA (Oregon))**. Para a tabela **Music**, selecione a guia **Items (Itens)** e escolha **Create Item (Criar item)**. Ao lado direito, selecione a opção de entrada **JSON**, copie o conteúdo do quadro abaixo.
      
2. Em **Artist**, insira **U2**. Em Song, insira Song **One**. 
   <br>Para gravar o item, escolha **Create item (Criar item)**. ![Create Item](/images/dynamodb-create-item-2.png?classes=shadow")

Os items são replicados para todas as regiões réplicas, independente de qual a região o item foi inserido.


#### Apagando os recursos
1. Na Console de gerenciamento da AWS na região **(Oeste dos EUA (Oregon))**. Para a tabela **Music**, selecione a tabela e clique em **Delete (Apagar)**
   Confirme digitando **Delete** e depois selecionando **Delete table (apagar tabela)**.
2. Repita o passo anterior para a região **us-east-2 (Leste dos EUA (Ohio))**.

