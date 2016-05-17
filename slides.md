
class: center, middle

# Aspectos Práticos de Serverless

Aldrin Leal, &lt;aldrin@leal.eng.br&gt;

---

# Agenda

 * Premissas
 * Seu querido palestrante
 * Então você quer fazer serverless, né?
 * Os desafios para o Serverless - e como tratá-los
 * Conclusão

---

# Premissas

  * Eu irei apresentar menos a plataforma da AWS do que eu gostaria
    * [Sugiro o whitepaper da AWS](https://d0.awsstatic.com/whitepapers/AWS_Serverless_Multi-Tier_Architectures.pdf)
    * e a [apresentação do Tim Wagner](https://chariotsolutions.com/wp-content/uploads/2016/04/Serverless-Design-Patterns-with-AWS-Lambda-Philly-ESE.pdf)
  * Sem perguntas, please!
  * Irei ministrar demos no corredor depois
    * E contar histórias interessantíssimas :)

---

# Seu querido palestrante

 * 99% Brasileiro, mas aquele 1% paraense
 * Release Engineer, mas q foi pro Big Data
 * Hobby: AWS... e Maven!
   * beanstalker
   * aws-sdk-typescript
   * jenkins: awseb-deployment-plugin /
   * serverless: lambada
   * IRL: Curso de AWS da SOA|Expert (agora Sciensa)
 * Como e por que? <3 Windows e Mercurial
 * (Obviamente, amo tipos)

---

# E antes que você me pergunte...

> "-Meu nome é Enéas!"

---

# Então você quer criar seu app serverless?

*Eu endosso, mas recomendo fortemente que você veja o ecossistema como um todo!*

  * Lambda
  * todaviaentretantocontudo:
    * API Gateway | Mobile SDK
    * Cognito
    * CloudWatch
      * Events
      * Logs
      * Metrics
    * SNS / SES
    * IOT
  * fora os outros serviços!

---

# Desafios

*"-Obrigado por perguntar!"*

  * O Runtime
  * Payloads
  * Configuração
  * Desenvolvimento Local
  * Identidade e Segurança
  * Logging
  * Testes
  * Recursos Agregados
  * Deployment / Publicação

---

# O Runtime do Lambda

 * OpenJDK 1.8, x64
 * Você *NÃO* é root
 * 50MB seu código, 512MB no /tmp
 * Bin Packing, ciclo de vida, conexões bloqueadas
 * mas mas mas quem sou eu?

```shell
$ curl checkip.amazonaws.com # ou o ifconfig
```

 * Entra json, sai qqr coisa mddc
 * Baixando os fontes

---

## Payload

 * JSON
 * Enriquecimento: API Gateway / CloudWatch Events
 * O Misterioso campo "Client Context"
   * Objeto JSON, em Base64

```kotlin
val lambdaClient = AWSLambdaClient()

val clientContext = Base64.getEncoder()
  .encode(MAPPER.writeValueAsString(mapOf(
        "custom" to mapOf(
                "warm" to "true"
        )
))
  .toByteArray())
  .toString(Charset.defaultCharset())

val invokeRequest = InvokeRequest()
  .withFunctionName("s2p_handle")
  .withClientContext(clientContext)
```

---

# Configuração

  * Importante, né?
  * Java?
    * .properties
      * Opa, redeploy!
    * Lembra do Enriquecimento?
    * Tabelas DynamoDB (ou até SimpleDB)
    * etcd + versão

---

# Como Desenvolver Local?

  * JUnit / Stubs
  * [ngrok!](https://ngrok.com/)
  * mvn lambada:serve

---

# Identidade / Segurança

  * IAM
    * Grupos
    * Permissões (ec2:*Network para VPC)
    * Perfis de Execução
  * Lambda Execution Role
  * API Gateway permite Delegação
  * Usuário Deployer
  * Cognito

---

# Logging

  * Em Java: Log4J
  * Variável para o Request Id no Logger
  * System.out *PODE* ir para o Log

---

# Como testar?

  * public static void main...
  * ngrok!
  * Testes Unitários / Mocks
  * Isolamento de Ambientes:
    * Variáveis / Prefixos
    * Versões / Aliases
    * Outra conta AWS
      * Consolidated Billing

---

# Empacotando Recursos

  * CloudFormation
    * Casos Extremos: Terraform
  * API Gateway: Swagger
  * Seu próprio Código

---

# Como publicar?

 * S3 na raça
 * S3 do Elastic Beanstalker
   * Bucket Default for Usuário
 * codecommit-deployer

---

# Meu Momento Jabá

  * https://github.com/ingenieux/lambada/
  * https://ingenieux.gitbooks.io/lambada-book/
  * Blueprints em https://github.com/ingenieux/ (ou /aldrinleal):
    * Bot para Telegram
    * Kotlin/Facebook Messenger Bot
    * SNS (Alertas CloudWatch) para [Pushover](https://pushover.net/) 

---

# Concluindo

  * Serverless é ótimo pro bolso e pra sanidade mental
    * Mas há aspectos para analisar
  * Tudo isso foram apenas anotações pessoais, baseados na minha experiência
  * De concreto, apenas recomendo: Use Filtro Solar :D

---

# Obrigado

> aldrin@leal.eng.br / https://about.me/aldrinleal

> https://ingenieux.gitbooks.io/lambada-book/
