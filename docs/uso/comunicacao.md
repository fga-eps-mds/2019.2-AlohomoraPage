# Comunicação

A comunicação com a aplicação é baseada em GraphQL e utiliza o caminho  **/graphql**. Você pode encontrar mais detalhes sobre o GraphQL [aqui](https://graphql.org/).

## CRUD

A aplicação conta com um mecanismo de ***crud*** para:
- [Morador](#morador)
- [Visitante](#visitante)
- [Funcionarios](#funcionarios)
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

#### Criando um morador

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

#### Atualizando os dados de um morador
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
#### Deletando um morador 

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

### Funcionarios

Um funcionário é uma entidade que representa tanto os funcionários do condomínio quanto funcionários terceiros, como entregadores de comida e encanadores por exemplo. Um funcionário, a nível de dados, é representado pelo seguinte conjunto de varíaveis
> ***completeName***: o nome do funcionário
> ***email***: o email do funcionário
> ***password***: a senha do funcionário

#### Criando um funcionário

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
> Note que aqui estamos usuando email e senha.

#### Atualizando os dados de um funcionário
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

#### Deletando um funcionário

```graphql
mutation deleteService ($email: String!) {
        deleteService (email: $email) {
            serviceEmail
        }
    }
```

*Alguma dúvida? Confira os [exemplos](#exemplos)*!

### Bloco

Um bloco é uma entidade que representa uma certa área do condomínio. O bloco por padrão, a nível de dados, é representado por um **número**.

#### Criando um bloco

```graphql
mutation createBlock ($number: String) {
        createBlock (number: $number) {
            number
        }
    }
```

#### Atualizando os dados de um bloco
```graphql
mutation updateBlock ($number: String!) {
    updateBlock (number: $number) {
        number
    }
}
```

#### Deletando um bloco
```graphql
mutation deleteBlock ($blockNumber: String!) {
    deleteBlock (blockNumber: $blockNumber) {
        blockNumber
    }
}
```
*Alguma dúvida? Confira os [exemplos](#exemplos)*!

### Apartamento

Um apartamento é uma entidade que representa uma casa ou um apartamento do condomínio. Um apartamento é representado por um **número** e está diretamente associado a um bloco.

#### Criando um apartamento

```graphql
mutation createApartment(
    $number: String,
    $blockNumber: String,
    ){
        createApartment(
            number: $number,
            blockNumber: $blockNumber
        ){
            number
            blockNumber
        }
    }
```

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
        }
    }
```

#### Deletando um apartamento
```graphql
mutation deleteApartment ($apartmentNumber: Int!){
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

