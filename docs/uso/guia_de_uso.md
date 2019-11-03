# Guia de uso

Aqui você encontra detalhes sobre como utilizar cada ferramenta disponibilizada na API.

## Conteúdo
- 1 - [Como funciona a comunicação](#1-comunicação)
- 2 - [CRUD](#2-crud)
    - 2.1 - [Morador](#21-morador)
        - 2.1.1 - [Criando um morador](#211-criando-um-morador)
        - 2.1.2 - [Modificando os dados de um morador](#212-modificando-os-dados-de-um-morador)
        - 2.1.3 - [Deletando um morador](#213-deletando-um-morador)
    - 2.2 - [Visitante](#22-visitante)
        - 2.2.1 - [Criando um visitante](#221-criando-um-visitante)
        - 2.2.2 - [Modificando os dados de um visitante](#222-modificando-os-dados-de-um-visitante)
        - 2.2.3 - [Deletando um visitante](#223-deletando-um-visitante)
    - 2.3 - [Serviços](#23-serviços)
        - 2.3.1 - [Criando um serviço](#231-criando-um-serviço)
        - 2.3.2 - [Modificando os dados de um serviço](#232-modificando-os-dados-de-um-serviço)
        - 2.3.3 - [Deletando um serviço](#233-deletando-um-serviço)
    - 2.4 - [Bloco](#24-bloco)
        - 2.4.1 - [Criando um bloco](#241-criando-um-bloco)
        - 2.4.2 - [Modificando os dados de um bloco](#242-modificando-os-dados-de-um-bloco)
        - 2.4.3 - [Deletando um bloco](#243-deletando-um-bloco)
    - 2.5 - [Apartamento](#25-apartamento)
        - 2.5.1 - [Criando um Apartamento](#251-criando-um-apartamento)
        - 2.5.2 - [Consultando um apartamento](#252-consultando-um-apartamento)
        - 2.5.3 - [Modificando os dados de um apartamento](#253-modificando-os-dados-de-um-apartamento)
        - 2.5.4 - [Deletando um apartamento](#254-deletando-um-apartamento)
- 3 - 

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

### 2.1 - Morador

Um morador é uma entidade que está vinculada a um bloco e a um apartamento e que possui direito à autenticação a fim de realizar [*entradas*](#entradas). Por padrão, um morador, a nível de dados, é representado pelo seguinte conjunto de atributos:
> ***completeName***: o nome completo do morador
> ***email***: o email do morador
> ***phone***: o número de telefone do morador
> ***cpf***: o número do CPF do morador
> ***apartment***: o apartamento no qual o morador reside
> ***block***: o bloco no qual o morador pertence
> ***voiceData***: o vetor de características [MFCC](https://en.wikipedia.org/wiki/Mel-frequency_cepstrum) da voz
> ***mfccAudioSpeakingName***: o vetor de características [MFCC](https://en.wikipedia.org/wiki/Mel-frequency_cepstrum) do áudio do morador dizendo o próprio nome

#### 2.1.1 - Criando um morador
Para criar um morador, além dos dados básicos como: nome, CPF, email, telefone e etc., serão necessários dois atributos vitais, estes são:
- > ***voiceData***: 
*O vetor de áudio do morador dizendo uma frase comum a todos os moradores do condomínio.*
Aqui é recomendado que todos os áudios enviados tenham a taxa de amostragem igual a 16000. Tenha cuidado também para que os áudios não contenham grandes pausas, partes silenciosas ou cortes durante a fala.
- > ***mfccAudioSpeakingName***:
*O vetor de características MFCC de um áudio do morador dizendo o próprio nome.*
As mesmas recomendações em relação ao atributo *voiceData* devem ser consideradas aqui.

```graphql
mutation createResident(
            $completeName: String!,
            $email: String!,
            $phone: String!,
            $cpf: String!,
            $apartment: String!,
            $block: String!,
            $voiceData: String,
            $mfccData: String,
            $mfccAudioSpeakingName: String,
            $password: String,
            ){
            createResident(
                completeName: $completeName,
                email: $email,
                cpf: $cpf,
                phone: $phone,
                apartment: $apartment,
                block: $block,
                voiceData: $voiceData,
                mfccData: $mfccData,
                mfccAudioSpeakingName: $mfccAudioSpeakingName,
                password: $password
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
> **Observações**:

> Os campos **mfccData e voiceData são mutualmente exclusivos**!
 **mfccData** deve ser usado para enviar o **vetor de MFCC** já computado. **voiceData** deve ser usado para enviar o **vetor de áudio**; nesse caso, o cálculo do MFCC será realizado internamente na API.

> Note que ***voiceData*** e ***mfccAudioSpeakingName*** são passados como uma string, neste caso, deve-se enviar o vetor como um ***JSON***.

#### 2.1.2 - Modificando os dados de um morador
> Necessário **login** como **morador**.

Assuma que todos os campos enviados nesta mutation serão atualizados no registro do morador com os valores fornecidos.
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
    $mfccData: String,
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
> Disponível apenas para **superuser**

```graphql
mutation deleteResident ($email: String!) {
    residentEmail: $email
}
```

*Alguma dúvida? Confira os [exemplos](#exemplos)*!

---
### 2.2 - Visitante

Um visitante é uma entidade que representa exatamente o que o seu próprio nome sugere, um visitante. Essa entidade existe para possibilitar a interação de visitantes com o sistema de portaria a fim de garantir rastreabilidade e segurança. Por padrão, um visitante é representado pelos seguintes atributos:
> ***completeName***: o nome completo do visitante
> ***cpf***: o cpf do visitante

#### 2.2.1 - Criando um visitante
> Disponível apenas para **superuser**

```graphql
mutation createVisitor(
    $completeName: String!,
    $cpf: String!,
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

#### 2.2.2 - Modificando os dados de um visitante
> Disponível apenas para **superuser**

O CPF atual deve ser passado para que seja possível realizar a busca pelo visitante no banco de dados. Caso seja necessário alterar o CPF, envie o novo valor através do campo *newCpf*
```graphql
mutation updateVisitor(
    $cpf: String!,
    $completeName: String,
    $newCpf: String,
    ){
        updateVisitor(
            cpf: $cpf,
            completeName: $completeName,
            newCpf: $newCpf
        ){
            visitor{
                completeName
                cpf
            }
        }
    }
```

#### 2.2.3 - Deletando um visitante
> Disponível apenas para **superuser**
```graphql
mutation deleteVisitor ($cpf: String!) {
    deleteVisitor (cpf: $cpf) {
        cpf
    }
}
```
*Alguma dúvida? Confira os [exemplos](#exemplos)*!

---
### 2.3 - Serviços

Um *serviço* é uma entidade que serve para representar pessoas físicas ou jurídicas cujo interesse é unicamente a prestação de serviços ao condomínio ou a um morador em específico. 

A nível de dados, um serviço é representado pelo seguintes atributos:
> ***completeName***: o nome do funcionário ou da empresa
> ***email***: o email do funcionário ou da empresa
> ***password***: a senha do funcionário ou da empresa 

#### 2.3.1 - Criando um serviço
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


#### 2.3.2 - Modificando os dados de um serviço
> Necessário **login** como serviço

Neste caso a busca pelo *serviço* em específico será feita através dos **dados de login**. O **valor original** de cada atributo do serviço será **substituído pelo valor fornecido** através da mutation. Caso não seja desejável mudar um atributo em específico, apenas o omita na mutation.
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

#### 2.3.3 - Deletando um serviço
> Disponível apenas para **superuser**
```graphql
mutation deleteService ($email: String!) {
        deleteService (email: $email) {
            serviceEmail
        }
    }
```
*Alguma dúvida? Confira os [exemplos](#exemplos)*!

---
### 2.4 - Bloco

Um bloco é uma entidade que representa uma certa área do condomínio. O bloco por padrão, a nível de dados, é representado pelo atributo *número*.

#### 2.4.1 - Criando um bloco
> Disponível apenas para **superuser**
```graphql
mutation createBlock ($number: String!) {
        createBlock (number: $number) {
            number
        }
    }
```
Note que o atributo *number* pode conter letras e símbolos em sua composição.

#### 2.4.2 - Modificando os dados de um bloco
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
        block{
            number
        }
        number
    }
}
```
Neste caso a variável ***blockNumber*** será utilizada para procurar o bloco desejado, e o valor do atributo ***number*** do bloco será mudado para o valor da variável **$number** enviada através da mutation.

#### 2.4.3 - Deletando um bloco
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
### 2.5 - Apartamento

Um *apartamento* é uma entidade que representa uma unidade de moradia do condomínio. Um apartamento é representado pelo atributo *número* e está diretamente associado a um bloco.

#### 2.5.1 - Criando um apartamento
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

#### 2.5.2 - Consultando um apartamento

Para consultar um apartamento em específico:
```graphql
query apartment(
    $number: String!,
    $block: String!,
    ){
        apartment(
            number: $number,
            block: $block
        ){
            apartment{
                number
                block{
                    number
                }
            }
        }
    }
```
Para consultar **todos** os apartamentos:
```graphql
query allApartments{
    allApartments{
        [
            apartment{
                number
                block{
                    number
                }
            }
        ]
    }
}
```

#### 2.5.3 - Modificando os dados de um apartamento
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
Neste caso, a variável ***apartmentNumber*** será usada para procurar o apartamento desejado e mudará o valor do atributo ***number*** do apartamento para o valor da variável **$number** enviada através da mutation.

#### 2.5.4 - Deletando um apartamento
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

