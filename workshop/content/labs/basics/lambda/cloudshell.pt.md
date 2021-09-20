---
hidden: true
---

#### Crie uma função AWS Lambda

*Caso você já tenha uma função Lambda criada, você pode pular para a próxima etapa, em **Exporte a função Lambda***

1. Acesse a console do AWS CloudShell: [https://console.aws.amazon.com/cloudshell/home](https://console.aws.amazon.com/cloudshell/home).
   <br>Para este exemplo, escolha a região **us-east-1 (Virginia do Norte)**. O CloudShell ainda não está disponível em todas as regiões.
2. Com o prompt de comando pronto, precisamos **criar a role** para permitir acesso de execução da função Lambda:

    ```bash
    aws iam create-role --role-name lambda-execution --assume-role-policy-document '{"Version": "2012-10-17","Statement": [{ "Effect": "Allow", "Principal": {"Service": "lambda.amazonaws.com"}, "Action": "sts:AssumeRole"}]}'
    ```

    É necessário agora da permissão para que a função possa ser executada. Para isto, adicione a política *AWSLambdaBasicExecutionRole* à role criada acima:
    ```bash
    aws iam attach-role-policy --role-name lambda-execution --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
    ```

3. Criaremos agora a função lambda via CLI. para isto, execute o comando abaixo: 
    ```bash
    echo "exports.handler = async (event) => {
        console.log('chamando função lambda...', event);
        const response = {
            'statusCode': 200,
            'body': JSON.stringify('Testando o import e export de funções Lambda!'),
        };
        return response;
    };" > index.js
    ```

4. O comando acima criou o arquivo **index.js** contendo o código-fonte da nossa função Lambda. Vamos agora empacotar a função em um arquivo .zip para podermos registrá-la no serviço AWS Lambda da região em questão:
    ```bash
    zip multi-region-test-function index.js
    ```
5. Podemos agora registrar a função através do comando abaixo:
    ```bash
    export ACC_ID=$(aws sts get-caller-identity --query Account --output text)
    aws lambda create-function --function-name multi-region-test-function --zip-file fileb://multi-region-test-function.zip --handler index.handler --runtime nodejs14.x --role arn:aws:iam::$ACC_ID:role/lambda-execution
    ```
   - Em **Function name (nome da função)** insira **multi-region-test-function**. Em Runtime, escolha **Node.js 12x**.
   - Para criar a função, selecione **Create (Criar)**. A função será criada e você será redirecionado para o editor de funções do console.
   - Copie e cole o código abaixo:

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
6. *(Opcional)* Caso a função já exista no serviço AWS Lambda, é possível atualizar o código fonte da função através do comando abaixo:
    ```bash
    aws lambda update-function-code --function-name multi-region-test-function --zip-file fileb://multi-region-test-function.zip
    ```

7. Vamos agora testar a função criada. Utilize o comando abaixo para invocar a função Lambda através da linha de comando:
    ```bash
    aws lambda invoke --function-name multi-region-test-function --payload $(echo '{ "key_sample": "value_sample" }' | base64) --log-type Tail --query 'LogResult' --output text out | base64 -d
    ```
    Temos uma linha INFO contendo a mensagem do console dentro da função juntamente com o payload enviado, o que indica que a função foi executada com sucesso.
8. Pronto! Sua função Lambda foi criada com sucesso via AWS CLI com o CloudShell.

#### Exporte a função Lambda criada
1. Acesse a console do AWS CloudShell: [https://console.aws.amazon.com/cloudshell/home](https://console.aws.amazon.com/cloudshell/home).
   <br>Para este exemplo, escolha a região **us-east-1 (Virginia do Norte)**.
2. *(Opcional)* Com o prompt de comando pronto, iremos **listar as funções** Lambda existentes em cada região. Para isto, utilize o comando abaixo, substituindo a região caso queira:
    ```bash
    aws lambda list-functions --region sa-east-1
    ```
3. Neste lab, já sabemos qual é o nome da função que criamos anteriormente. Desta forma, iremos exportá-la e gerar um .zip com o código fonte através do comando:
    ```bash
    aws lambda get-function --function-name multi-region-test-function --query 'Code.Location' | xargs wget -O multi-region-test-function-exp.zip
    ```
    *É possível extrair os demais metadados da função Lambda como as configurações de timeout ou variáveis de ambiente por exemplo. Para este lab, focamos apenas no código fonte para a simplicidade do lab*
4. Pronto. Já temos o código fonte de nossa função e podemos importá-la em outra região.

#### Importe a função Lambda na segunda região
*Iremos atualizar uma função existente em outra região. Neste exemplo, utilizaremos a região **sa-east-1 (North America - São Paulo)**. Caso a funcão ainda não exista nesta região, execute os passos da etapa **Crie uma função Lambda** descrita no início deste lab.*
1. Acesse a console do AWS CloudShell: [https://console.aws.amazon.com/cloudshell/home](https://console.aws.amazon.com/cloudshell/home).
   <br>Para este exemplo, escolha a região **us-east-1 (Virginia do Norte)**. 
2. Iremos atualizar a função **multi-region-test-function** na região **sa-east-1 (South America - São Paulo)**. Para isto, execute o seguinte comando:
    ```bash
    aws lambda update-function-code --function-name multi-region-test-function --region sa-east-1 --zip-file fileb://multi-region-test-function-exp.zip
    ```
3. Vamos agora testar a importação fazendo a chamada da função. Para isto:
    ```bash
    aws lambda invoke --function-name multi-region-test-function --payload $(echo '{ "key_sample": "value_sample" }' | base64) --log-type Tail --query 'LogResult' --region sa-east-1 --output text out | base64 -d
    ```
    <br/>Podemos validar que a função foi atualizada pois a linha de log INFO agora escreve também o payload recebido, o que não acontecia com a função criada no início deste lab.
4. Pronto! Sua função Lambda foi criada em outra região.

#### Apagando os recursos
1. Apague as funções lambda criadas em ambas as regiões com os seguintes comandos:
    ```bash
    aws lambda delete-function --function-name multi-region-test-function --region us-east-1
    aws lambda delete-function --function-name multi-region-test-function --region sa-east-1
    ```