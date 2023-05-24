# Eng-Software_CEFETMG_Hexagonal-Architecture
Trabalho 1 de Laboratório de Engenharia de Software - 2023/1 - Arquitetura Hexagonal
# Arquitetura Hexagonal - Engenharia de Software

# 1 O que é uma Arquitetura Hexagonal?

## 1.1 Introdução

O conceito de Arquitetura Hexagonal foi proposto por Alistair Cockburn, em meados dos anos 90, em um [artigo](http://wiki.c2.com/?HexagonalArchitecture) postado na primeira wiki que foi desenvolvida, chamada WikiWikiWeb (cujos artigos tratavam principalmente de temas relacionados com Engenharia de Software).

Os objetivos de uma Arquitetura Hexagonal são parecidos com os de uma Arquitetura Limpa, tal como descrevemos em um outro [artigo](https://engsoftmoderna.info/artigos/arquitetura-limpa.html). Mas, para reforçar, a ideia é construir sistemas que favorecem reusabilidade de código, alta coesão, baixo acoplamento, independência de tecnologia e que são mais fáceis de serem testados.

Uma Arquitetura Hexagonal divide as classes de um sistema em dois grupos principais:

- Classes de domínio, isto é, diretamente relacionadas com o negócio do sistema.
- Classes relacionadas com infraestrutura, tecnologias e responsáveis pela integração com sistemas externos (tais como bancos de dados).

Além disso, em uma Arquitetura Hexagonal, **classes de domínio não devem depender de classes relacionadas com infraestrutura, tecnologias ou sistemas externos**. A vantagem dessa divisão é desacoplar esses dois tipos de classes.

Assim, as classes de domínio não conhecem as tecnologias – bancos de dados, interfaces com usuário e quaisquer outras bibliotecas – usadas pelo sistema. Consequentemente, mudanças de tecnologia podem ser feitas sem impactar as classes de domínio. Talvez ainda mais importante, as classes de domínio podem ser compartilhadas por mais de uma tecnologia. Por exemplo, um sistema pode ter diversas interfaces (Web, mobile, etc).

Em uma arquitetura hexagonal, a comunicação entre as classes dos dois grupos é mediada por **adaptadores**.

Visualmente, a arquitetura é representada por meio de dois hexágonos concêntricos. No hexágono interno, ficam as classes do domínio (ou classes de negócio, se você preferir). No hexágono externo, ficam os adaptadores. Por fim, as classes de interface com o usuário, classes de tecnologia ou de sistemas externos ficam fora desses dois hexágonos.

![arquitetura-hexagonal](arquitetura-hexagonal.svg)

## 1.2 Adaptadores e Portas

Em uma Arquitetura Hexagonal, o termo **porta** designa as interfaces usadas para comunicação com as classes de domínio (veja que interface aqui significa interface de programação; por exemplo, uma **interface** de Java).

Existem dois tipos de portas:

- **Portas de entrada:** são interfaces usadas para comunicação de fora para dentro, isto é, quando uma classe externa precisa chamar um método de uma classe de domínio. Logo, essas portas declaram os serviços providos pelo sistema, isto é, serviços que o sistema oferece para o mundo exterior.
- **Portas de saída:** são interfaces usadas para comunicação de dentro para fora, isto é, quando uma classe de domínio precisa chamar um método de uma classe externa. Logo, essas portas declaram os serviços requeridos pelo sistema, isto é, serviços do mundo exterior que são necessários para o funcionamento do sistema.

O importante é que **as portas são independentes de tecnologia**. Portanto, elas estão localizadas no hexágono interior.

Por outro lado, os sistemas externos, normalmente, usam alguma tecnologia, seja ela de comunicação (REST, gRPC, GraphQL, etc), de bancos de dados (SQL, noSQL, etc), de interação com o usuário (Web, mobile, etc), etc.

Daí a necessidade de componentes localizados no hexágono mais externo da arquitetura – os **adaptadores** –, os quais atuam de um dos dois modos a seguir:

- Eles recebem chamadas de métodos vindas de fora do sistema e
  encaminham essas chamadas para métodos adequados das portas de entrada.
- Eles recebem chamadas vindas de dentro do sistema, isto é, das classes de domínio, e as direcionam para um sistema externo, tais como um banco de dados, um outro sistema da organização ou mesmo de terceiros.
![hex-ports-adapters](img.png)
## 1.3 Exemplo de implementação


```java
@RestController
@RequestMapping(path = "/api")
public class AccountResource {
@Autowired
private AccountService accountService;
@GetMapping(path = "/account/{accountNumber}", produces =
"application/json")
public ResponseEntity<Account>
getAccountInfo(@PathVariable("accountNumber") String accountNumber)
{
return
ResponseEntity.ok(accountService.getAccountInfo(accountNumber));
}
}

```

