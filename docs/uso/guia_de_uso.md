# Guia de uso

Aqui você encontra detalhes sobre como utilizar cada ferramenta disponibilizada na API.

## Conteúdo
- 1 - [Como funciona a comunicação](#1-comunicação)
- 2 - [CRUD](#2-crud)
    - 2.1 - [Morador](#morador)
        - 2.1.1 - [Criando um morador](#criando-um-morador)
        - 2.1.2 - [Modificando os dados de um morador](#2.1.2-modificando-os-dados-de-um-morador)
        - 2.1.3 - [Deletando um morador](#2.1.3-deletando-um-morador) 

## 1 - Comunicação

A comunicação com a API é baseada em GraphQL e utiliza a rota padrão  **/graphql**. Você pode encontrar mais detalhes sobre o GraphQL [aqui](https://graphql.org/).

## 2 - CRUD

A aplicação conta com um mecanismo de ***crud*** para:
- [Morador](#morador)
- [Visitante](#visitante)
- [Serviços](#serviço)
- [Bloco](#bloco)
- [Apartamento](#apartamento)

## Autênticação

A aplicação conta com um sistema de autênticação do morador por biometria de voz. [Veja mais](#autenticação).

## Logs de entrada
A aplicação ainda possui um sistema de rastreamento de entradas. [Veja mais](#entradas).

---

### Morador

Um morador é uma entidade que está vinculada a um bloco e a um apartamento e que possui direito de se autenticar a fim de realizar entradas. Por padrão, um morador, em nível de dados, é representado pelo seguinte conjunto de variaveis:
> ***completeName***: o nome completo do morador
> ***email***: o email do morador
> ***phone***: o número de telefone do morador
> ***cpf***: o número do CPF do morador
> ***apartment***: o apartamento no qual o morador pertence
> ***block***: o bloco no qual o morador pertence
> ***voiceData***: o vetor do audio do morador dizendo a frase de autenticação

<a name="criando-um-morador"></a>
#### 2.1.1 Criando um morador

```graphql
mutation createResident(
            $completeName: String!,
            $email: String!,
            $phone: String!,
            $cpf: String!,
            $apartment: String!,
            $block: String!,
            $voiceData: String,
            ){
            createResident(
                completeName: $completeName,
                email: $email,
                cpf: $cpf,
                phone: $phone,
                apartment: $apartment,
                block: $block,
                voiceData: $voiceData
            ){
                resident{
                    completeName
                    email
                    cpf
                    phone
                    apartment{
                        number
                        block{
                            number
                        }
                    }
                }
            }
        }
```
> Note que ***voiceData*** é passado como uma string, neste caso, deve-se enviar o vetor do áudio como um JSON.

#### 2.1.2 - Modificando os dados de um morador
```graphql
mutation updateResident (
    $completeName: String,
    $email: String,
    $phone: String,
    $cpf: String,
    $apartment: String,
    $block: String,
    $password: String,
    $voiceData: String,
    ){
        updateResident (
            completeName: $completeName,
            email: $email,
            phone: $phone,
            cpf: $cpf,
            apartment: $apartment,
            block: $block,
            password: $password,
            voiceData: $voiceData
        ){
            resident {
                completeName
                email
                phone
                cpf
                apartment{
                    number
                    block{
                        number
                    }
                }
            }
        }
}
```
#### 2.1.3 - Deletando um morador 

```graphql
mutation deleteResident ($email: String!) {
    residentEmail: $email
}
```

*Alguma dúvida? Confira os [exemplos](#exemplos)*!

### Visitante

Um visitante é uma entidade que representa exatamente o que o seu próprio nome sugere, um visitante. Essa entidade existe para possibilitar a interação de visitantes com o sistema da portaria a fim de garantir rastreabilidade e segurança. Por padrão um visitante é representado pelo seguinte conjunto de variaveis:
> ***completeName***: o nome completo do visitante
> ***cpf***: o cpf do visitante

#### Criando um visitante

```graphql
mutation createVisitor(
    $completeName: String,
    $cpf: String,
    ){
        createVisitor(
            completeName: $completeName,
            cpf: $cpf
        ){
            visitor{
                completeName
                cpf
            }
        }
    }
```

#### Atualizando os dados de um visitante
```graphql
mutation updateVisitor(
    $completeName: String,
    $cpf: String!,
    $new_cpf: String,
    ){
        updateVisitor(
            completeName: $completeName,
            cpf: $cpf,
            new_cpf: $new_cpf
        ){
            visitor{
                completeName
                cpf
            }
        }
    }
```
> Note que o CPF atual deve ser passado além do possível novo nome ou CPF.

#### Deletando um visitante
```graphql
mutation deleteVisitor ($cpf: String!) {
    deleteVisitor (cpf: $cpf) {
        cpf
    }
}
```
*Alguma dúvida? Confira os [exemplos](#exemplos)*!

### Serviço

Um *serviço* é uma entidade que serve para representar pessoas físicas ou jurídicas cujo interesse é unicamente a prestação de serviços ao condomínio ou a um morador em específico. 

A nível de dados, um serviço é representado pelo seguintes atributos:
> ***completeName***: o nome do funcionário ou da empresa
> ***email***: o email do funcionário ou da empresa
> ***password***: a senha do funcionário ou da empresa 

#### Criando um funcionário
> Disponível apenas para **superuser**
```graphql
mutation createService (
    $completeName: String!,
    $email: String!,
    $password: String!,
    ){
        createService (
            completeName: $completeName,
            email: $email,
            password: $password
        ){
            service{
                completeName
                email
            }
        }
    }
```


#### Atualizando os dados de um *serviço*
> Necessário **login** como serviço
```graphql
mutation updateService(
        $completeName: String,
        $email: String,
        $password: String,
    ){
        updateService(
            completeName: $completeName,
            email: $email,
            password: $password
        ){
            service{
                completeName
                email
            }
        }
    }
```
Neste caso a busca pelo *serviço* em específico será feita através dos **dados de login**. Os **valores originais** de cada atributo do serviço serão **substituídos pelos valores fornecidos** através da mutation. Caso não seja desejável mudar um atributo em específico, apenas o omita na mutation.

#### Deletando um funcionário
> Disponível apenas para **superuser**
```graphql
mutation deleteService ($email: String!) {
        deleteService (email: $email) {
            serviceEmail
        }
    }
```

*Alguma dúvida? Confira os [exemplos](#exemplos)*!

### Bloco

Um bloco é uma entidade que representa uma certa área do condomínio. O bloco por padrão, a nível de dados, é representado pelo atributo *número*.

#### Criando um bloco
> Dispon;ivel apenas para **superuser**
```graphql
mutation createBlock ($number: String) {
        createBlock (number: $number) {
            number
        }
    }
```
Note que o atributo *number* pode conter letras e símbolos em sua composição.

#### Atualizando os dados de um bloco
> Disponível apenas para **superuser**
```graphql
mutation updateBlock (
    $number: String!,
    $blockNumber: String!
    ){
    updateBlock (
        number: $number,
        blockNumber: $blockNumber
        ){
        number
    }
}
```
Neste caso a variável ***blockNumber*** será utilizada para procurar o bloco desejado, e o valor do atributo ***number*** será mudado para o valor da variável **$number** enviada através da mutation.

#### Deletando um bloco
> Disponível apenas para **superuser**
```graphql
mutation deleteBlock ($blockNumber: String!) {
    deleteBlock (blockNumber: $blockNumber) {
        blockNumber
    }
}
```
*Alguma dúvida? Confira os [exemplos](#exemplos)*!

---

### Apartamento

Um *apartamento* é uma entidade que representa uma unidade de moradia do condomínio. Um apartamento é representado pelo atributo *número* e está diretamente associado a um bloco.

#### Criando um apartamento
> Disponível apenas para **superuser**
```graphql
mutation createApartment(
    $number: String!,
    $blockNumber: String,
    ){
        createApartment(
            number: $number,
            blockNumber: $blockNumber
        ){
            number
            blockNumber
            block{
                number
            }
        }
    }
```
Note que o atributo número pode conter letras e símbolos em sua composição. Você pode associar um apartamento a um bloco simplesmente informando o número do bloco desejado.

#### Atualizando os dados de um apartamento
```graphql
mutation updateApartment(
    $number: String!,
    $apartmentNumber: String!,
    ){
        updateApartment(
            number: $number,
            apartmentNumber: $apartmentNumber
        ){
            number
            apartmentNumber
            apartment{
                number
                block{
                    number
                }
            }
        }
    }
```
Neste caso, a variável ***apartmentNumber*** será usada para procurar o apartamento desejado e mudará o valor do atributo ***number*** para o valor da variável **$number** enviada através da mutation.

#### Deletando um apartamento
> Disponível apenas para **superuser**
```graphql
mutation deleteApartment ($apartmentNumber: String!) {
    deleteApartment (apartmentNumber: $apartmentNumber) {
        apartmentNumber
    }
}
```

---

## Entradas

*Entradas* é uma entidade que visa garantir **rastreabilidade na movimentação do condomínio**, guardando as **informações da entrada**. Com entradas, é possível saber **quem entrou, quando entrou e para onde foi**.

Entradas são divididas em dois tipos: ***Entradas de morador*** e ***Entradas de visitante***.

Para entradas de morador, cada entrada está associada a um morador do condomínio, já para entradas de visitante, um visitante será associado.

Em ambos os tipos existe um destino associado, um apartamento específico.

> *Obs*: entradas de visitante precisam ser diretamente aprovadas pelo morador

#### Criando entradas de morador
```graphql
mutation createEntry(
    $residentCpf: String,
    $apartmentNumber: String,
    ){
        createEntry(
            residentCpf: $residentCpf,
            apartmentNumber: $apartmentNumber
        ){
            resident {
                completeName
                email
                phone
                cpf
                apartment{
                    number
                    block{
                        number
                    }
                }
            }
            apartment{
                number
                block{
                    number
                }
            }
            residentCpf
            apartmentNumber
        }
    }
```

## Autenticação

