# AWS-Lambda-com-Aliases-e-API-Gateway-com-Stages
Gerenciando múltiplos ambientes (como desenvolvimento e produção) de forma organizada e segura em aplicações serverless, utilizando as funcionalidades de versionamento e direcionamento do Lambda e API Gateway.

Passo 1 => Criação da Função AWS Lambda
Acessar Console Lambda: Faça login no Console AWS e navegue até o serviço Lambda. Criar Função Lambda: Clique em Criar função e em seguida, criar do zero: Selecione essa opção. Nome da função: Insira um nome descritivo, ex: minha-funcao-proxy-lab-seunomesobrenome. Runtime (Tempo de execução): Selecione Python 3.9. Em Arquitetura, Selecionar : x86_64. Alterar a função de execução padrão de execução: Selecione "Criar uma nova função com permissões básicas do Lambda".
Em seguida, clique em Criar função.

![image](https://github.com/user-attachments/assets/2927174b-78eb-4956-bf77-8b1d9c7adfe3)




Na página da função Lambda recém-criada, role para baixo até a seção
Código (Origem do código). 

![image](https://github.com/user-attachments/assets/2fd866be-a053-4fb2-8f96-2b49fa484352)


Substitua o código padrão no editor (lambda_function.py) pelo Código em Python em Anexo para o ambiente de Desenvolvimento, copiar o código abaixo, colar e clicar em Deploy.

** Buscar em anexo o código Python de Desenvolvimento


  Testar a Função Lambda (versão $LATEST):
  Clique na aba Testar (ao lado da aba Código). Em Evento de teste, selecione Criar novo evento. Nome do evento: teste-simulado. Modelo: Selecione API Gateway AWS Proxy. No JSON do evento, localize a chave requestContext e dentro dela, a chave stage (você a localizará na linha 100).
Altere o valor encontrado na linha 100, por "test-stage". Clique em Salvar e depois em Testar. Verifique o resultado em Resultado da execução e em seguida, em detalhes.

Parte 2 => Publicação de Versões e Criação de Aliases

  Publicar a Versão 1 da Função Lambda: Na página da sua função Lambda, vá para a aba Versões. Clique em Publicar nova versão. Descrição da versão (opcional): Versão inicial da função de desenvolvimento. Clique em Publicação ou Publicar. Criar Alias "dev": Volte para a sua Função lambda "minha-funcao-proxy-lab-seunomesobrenome". Vá para a aba Aliases. Clique em Criar alias. Nome do alias: dev. Descrição do alias (opcional): Alias para ambiente de desenvolvimento. Versão: Selecione 1.

 Copie o ARN e guarde-o no bloco de notas, você vai precisar posteriormente.
 
 Modificar o Código da Função Lambda (para versão "prod"): Volte para a sua Função lambda "minha-funcao-proxy-lab-seunomesobrenome"
Volte para a aba Código. Substitua o código no editor (lambda_function.py) pelo: Código Python Em Anexo: Código py do ambiente de PRODUÇÃO! Em seguida, clique em Deploy. 

** Buscar em anexo o código Python de Produção

  Teste novamente a versão $LATEST (usando o mesmo evento teste-simulado). Clique na aba Testar. Clique no botão Testar. Verifique o resultado em Resultado da execução -> Detalhes.
  
 Publicar a Versão 2 da Função Lambda:
 Na aba Versões, clique em Publicar nova versão. Descrição da versão (opcional): Versão para ambiente de produção. Clique em Publicação ou Publicar. Criar Alias "prod": Volte para a sua Função lambda "minha-funcao-proxy-lab-seunomesobrenome". Na aba Aliases, clique em Criar alias. Nome do alias: prod. Descrição do alias (opcional): Alias para ambiente de produção. Versão: Selecione 2. Clique em Salvar.
 Copie o ARN e guarde-o no bloco de notas, você vai precisar  posteriormente.

![image](https://github.com/user-attachments/assets/dd67308f-356d-4a59-b3ba-1bb5d4876e18)

 

Parte 3 => Criação e Configuração do API Gateway
Acessar Console API Gateway: Navegue até o serviço API Gateway.
Clique em Criar uma API. Escolher um tipo de API: Localize a caixa API REST e clique em Compilar. Atenção: Não é a opção API REST privada. Criar nova API: Deixe "API nova" selecionado. Nome da API: Insira um nome, ex: minha-api-proxy-lab-seunomesobrenome. Descrição (opcional): API Gateway (proxy) para lab de Lambda com Aliases. Tipo de endpoint de API: "Regional". Clique em Criar API. Criar um Recurso (Path) na API: No painel esquerdo ("Recursos"), com a raiz / selecionada, clique em Criar recurso. Nome do recurso: hello. Clique em Criar recurso. Criar Método GET no Recurso /hello (com Proxy). Para o ambiente de desenvolvimento: Selecione o recurso /hello recém-criado. Clique em Criar método. Tipo de método: Selecione GET. Tipo de integração: Selecione Função Lambda.
Integração do proxy do Lambda: Habilite. Região da Lambda: Selecione a região onde sua função Lambda foi criada (já deve esta selecionada). Função Lambda: Cole o ARN do Alias Dev. Atenção: Esse é o meu exemplo. Você não deve colar esse ARN, Você deverá colar o ARN do do seu alias "dev". (arn:aws:lambda:us-east-1:IDde12digitosdaAWS:function:minha-funcao-proxy-lab-seunomesobrenome:dev (Substibuir o ID de 12 digitos da AWS por seu ID da conta da AWS, encontrado na parte superior à direita, no console da AWS). Clique em Criar método. Agora vamos implantar criando o Estágio para o ambiente de Desenvolvimento: Clique em Implantar API.

![image](https://github.com/user-attachments/assets/65dc28b3-2e95-4f6c-b1c2-f108f486898e)


 Em Estágio, selecione "Novo estágio"
 Nome do estágio: Desenvolvimento. Descrição da implantação: API/Lambda - Versao Desenvolvimento. Implantar. Vamos implantar criando o Estágio para o ambiente de Produção: Volte em Recursos, selecione o método GET, selecione Solicitação de integração e clique em Editar. Em Função Lambda, troque pelo ARN do alias "prod". Cole o ARN do alias prod. Você também pode editar, selecionando seu ARN padrão do sistema, onde aparecerá no final seu nome e sobrenome e acrescentar :prod, Será algo parecido com: arn:aws:lambda:us-east-1:237482015336:function:minha-funcao-proxy-lab-seunomesobrenome:prod e em seguida, Salvar.
 
 Agora vamos implantar criando o Estágio para o ambiente de Produção:
 Clique em Implantar API. Em Estágio, selecione "Novo estágio". Nome do estágio: Producao. Descrição da implantação: API/Lambda - Producao. E em seguida, Implantar.

![image](https://github.com/user-attachments/assets/7225f830-6c62-41db-a1a7-252b887c06d4)


Parte 4 =>Testes
 Agora é hora de testar se a ligação da sua função Lambda com o API Gateway funcionou. Caso dê algum erro, volte nos passos anteriores e confira cada passo. Para o teste, volte em Estágios. Clique no sinal de "+" (Desenvolvimento) para expandir, novamente e novamente, até você ver o método GET. Selecione o GET, você verá a URL "Invocar URL".
Copie e cole no seu navegador (Google/Firefox/Edge…) Estando tudo ok, você verá um resultado parecido com as imagens abaixo:

![image](https://github.com/user-attachments/assets/0e4bea9c-2229-42aa-bb52-197d2da86353)



Selecione a caixinha "Estilo de formatação"
Repita o procedimento para testar a base de Produção.
Se tudo ocorreu bem, o resultado será igual as imagens abaixo:

![image](https://github.com/user-attachments/assets/6c320393-9ffe-41a4-a065-1a160fbef8c0)


Parte 6 => Limpeza

Limpeza de Recursos (Importante para evitar custos desnecessários): Excluir Stages API Gateway: No console API Gateway, selecione sua API,
selecione a sua API e clique em "Excluir". Confirme a exclusão. Após conferir a exclusão, pode fechar e sair do console da AWS.

=> Laboratório desenvolvido no curso de AWS Developer da Escola da Nuvem

https://www.linkedin.com/school/escola-da-nuvem/

=>Laboratório realizado por Patrícia Sousa

https://github.com/PattySousa/AWS-Lambda-com-Aliases-e-API-Gateway-com-Stages
https://www.linkedin.com/in/patricia--sousa/

Aprendizado prático na Nuvem AWS ☁️ |AWS Lambda com Aliases e API Gateway com Stages
