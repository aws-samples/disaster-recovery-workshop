---
hidden: true
---

#### Crie uma função AWS Lambda

1. Acesse a console do AWS Lambda: [https://console.aws.amazon.com/lambda](https://console.aws.amazon.com/lambda).
   <br>Para este exemplo, escolha a região **us-east-1 (Virginia do Norte)**.
2. No painel de navegação, no lado esquerdo do console, selecione **Functions (Funções)**.
3. Escolha **Create Function (Criar função)**.
   ![create function](/images/lambda-create-function.png)
   - Selecione **Author from scratch (criar do zero)** 
   - Em **Function name (nome da função)** insira **multi-region-test-function**. Em Runtime, escolha **Node.js 14.x**.
   - Para criar a função, selecione **Create (Criar)**. 
   ![create function](/images/lambda-create-function-form.png)
   - A função será criada e você será redirecionado para o editor de funções do console. Em **Function Code (Código da função)** Copie e cole o código abaixo:

   ```javascript
   exports.handler = async (event) => {
      console.log("chamando função lambda...")
      const response = {
           statusCode: 200,
           body: JSON.stringify('Testando o import e export de funções Lambda!'),
      };
      return response;
   };
   ```

   - Clique em **Test (testar)** no canto superior direito. Um pop-up irá aparecer solicitando a configuração de um evento de teste.
   ![create function](/images/lambda-test-button.png)
   - Selecione **Create new test event (Criar novo evento de teste)**, dê o nome **eventdata** no campo **Event name (nome do evento)** e clique em **Create (Criar)**
   - Clique novamente em **Test (Testar)**. A mensagem **Execution result: succeeded** deve aparecer. Ao clicar em detalhes, um *payload* JSON deve aparecer com o texto * Hello from Lambda!*.
   ![create function](/images/lambda-success.png)
4. Sua função Lambda foi criada com sucesso! Iremos agora exportá-la da região corrente para posterior importação em outra região.

#### Exporte a função Lambda criada
1. Acesse a console do AWS Lambda: [https://console.aws.amazon.com/lambda](https://console.aws.amazon.com/lambda).
   <br>Lembrando que, para este exemplo, estamos utilizando por enquanto a região **us-east-1 (Virginia do Norte)**.
2. Na lista de funções, selecione a função **multi-region-test-function** criada anteriormente. A aba Configuration (Configuração) aparecerá.
3. Clique em Actions (Ações) -> Export Function (Exportar Função)
   ![create function](/images/lambda-export.png)
   - Um pop-up abrirá oferecendo duas opções para download: Download AWS SAM file (Baixar arquivo SAM) ou Download deployment package (Baixar pacote de instalação).
   - Selecione Deployment package (Pacote de instalação) e aguarde o download. Repare que o arquivo baixado é um arquivo zip, porém sem extensão. Após o download, renomeie o arquivo com a extensão .zip para podermos utilizá-lo posteriormente.

#### Importe a função Lambda na segunda região
1. Acesse a console do AWS Lambda: [https://console.aws.amazon.com/lambda](https://console.aws.amazon.com/lambda).
2. Selecione a região no combo do canto superior direito do console. 
<br/>Para este exemplo, utilizarei a região **sa-east-1 - South America (São Paulo)**
3. Caso a função não exista:
   - Execute os passos 1,2 e 3 no item 3 da seção **Crie uma função AWS Lambda** acima.
4. Na lista de funções, selecione a função **multi-region-test-function** criada. A aba Configuration (Configuração) aparecerá.
   - Em Function Code (código da função), clique em Actions (Ações), upload a .zip file (Enviar um arquivo .zip). 
      ![upload](/images/lambda-upload.png)
      - Você pode também importar sua função através de um bucket S3. para isto, selecione Upload a file from Amazon S3 (Enviar à partir do S3) e informe a URL onde o .zip se encontra.
   - Selecione o arquivo zip baixado anteriormente (não esqueça de renomeá-lo com a extensão .zip) e clique em Save (Salvar).
   - (Opcional) Caso queira testar sua função, configure o payload de teste como fizemos nos passos 5,6 e 7 do item 3 em **Crie uma função Lambda** acima.
5. Pronto! Sua função Lambda foi copiada de uma região para a outra.

#### Apagando os recursos
1. Apague as funções lambda criadas em ambas as regiões.

