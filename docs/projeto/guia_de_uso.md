# Guia de uso

Aqui você encontra detalhes sobre cada funcionalidade disponibilizada na API, bem como a maneiras de utiliza-las.

## Conteúdo
- 1 - [Como funciona a comunicação](#1-comunicação)
- 2 - [CRUD](#2-crud)
    - 2.1 - [Morador](#21-morador)
        - 2.1.1 - [Criando um morador](#211-criando-um-morador)
        - 2.1.2 - [Consultando um morador](#212-consultando-um-morador)
        - 2.1.3 - [Modificando os dados de um morador](#213-modificando-os-dados-de-um-morador)
        - 2.1.4 - [Deletando um morador](#214-deletando-um-morador)
    - 2.2 - [Visitante](#22-visitante)
        - 2.2.1 - [Criando um visitante](#221-criando-um-visitante)
        - 2.2.2 - [Consultando um visitante](#222-consultando-um-visitante)
        - 2.2.3 - [Modificando os dados de um visitante](#223-modificando-os-dados-de-um-visitante)
        - 2.2.4 - [Deletando um visitante](#224-deletando-um-visitante)
    - 2.3 - [Serviços](#23-serviços)
        - 2.3.1 - [Criando um serviço](#231-criando-um-serviço)
        - 2.3.2 - [Consultando um serviço](#232-consultando-um-serviço)
        - 2.3.3 - [Modificando os dados de um serviço](#233-modificando-os-dados-de-um-serviço)
        - 2.3.4 - [Deletando um serviço](#234-deletando-um-serviço)
    - 2.4 - [Bloco](#24-bloco)
        - 2.4.1 - [Criando um bloco](#241-criando-um-bloco)
        - 2.4.2 - [Consultando um bloco](#242-consultando-um-bloco)
        - 2.4.3 - [Modificando os dados de um bloco](#243-modificando-os-dados-de-um-bloco)
        - 2.4.4 - [Deletando um bloco](#244-deletando-um-bloco)
    - 2.5 - [Apartamento](#25-apartamento)
        - 2.5.1 - [Criando um Apartamento](#251-criando-um-apartamento)
        - 2.5.2 - [Consultando um apartamento](#252-consultando-um-apartamento)
        - 2.5.3 - [Modificando os dados de um apartamento](#253-modificando-os-dados-de-um-apartamento)
        - 2.5.4 - [Deletando um apartamento](#254-deletando-um-apartamento)
- 3 - [Autênticação de usuários](#3-autênticação-de-usuários)
    - 3.1 - [Autênticação de morador](#31-autênticação-de-morador)
- 4 - [Logs de entrada](#4-logs-de-entrada)
    - 4.1 - [Entradas de morador](#41-entradas-de-morador)
        - 4.1.1 - [Criando uma entrada de morador](#411-criando-uma-entrada-de-morador)
        - 4.1.2 - [Consultando uma entrada de morador](#412-consultando-uma-entrada-de-morador)
    - 4.2 - [Entradas de visitante](#42-entradas-de-visitante)
        - 4.2.1 - [Criando uma entrada de visitante](#421-criando-uma-entrada-de-visitante)
        - 4.2.2 - [Consultando uma entrada de visitante](#422-consultando-uma-entrada-de-visitante)
- 5 - [Administração](#5-administração)
    - 5.1 - [Criando e deletando administradores](#51-criando-e-deletando-administradores)
        - 5.1.1 - [Criando um administrador](#511-criando-um-administrador)
        - 5.1.2 - [Deletando um administrador](#512-deletando-um-administrador)
        - 5.1.3 - [Consultando um administrador](#513-consultando-um-administrador)
    - 5.2 - [Gerênciando conta de usuários](#52-gerênciando-conta-de-usuários)
        - 5.2.1 - [Ativando um usuário](#521-ativando-um-usuário)
        - 5.2.2 - [Desativando um usuário](#522-desativando-um-usuário)
        - 5.2.3 - [Consultando usuários desativados](#523-consultando-usuários-desativados)
- 6 - [Exemplos](#6-exemplos)
    - 6.1 - [Criando um morador](#61-criando-um-morador)
    - 6.2 - [Realizando login](#62-realizando-login)
    - 6.3 - [Verificando se o token está correto](#63-verificando-se-o-token-está-correto)
---

# 1. Comunicação

A comunicação com a API é baseada em GraphQL e utiliza a rota padrão  **/graphql**. Você pode encontrar mais detalhes sobre como utilizar o GraphQL [aqui](https://graphql.org/).

---

# 2. CRUD

A aplicação conta com um mecanismo de ***CRUD*** para as entidades vitais do negócio. Com este CRUD é garantida a consistência da lógica do sistema.

---

## 2.1. Morador

Um morador é uma entidade que está vinculada a um bloco e a um apartamento e que possui direito à autenticação a fim de realizar [*entradas*](#4-logs-de-entrada). Por padrão, um morador, a nível de dados, é representado pelo seguinte conjunto de atributos:
> ***completeName***: o nome completo do morador
>
> ***email***: o email do morador
>
> ***phone***: o número de telefone do morador
>
> ***cpf***: o número do CPF do morador
>
> ***block***: o número (ou código) do bloco no qual o morador pertence
>
> ***apartment***: o número do apartamento no qual o morador reside
>
> **audioSpeakingPhrase**: o vetor do áudio do morador dizendo a frase comum de autênticação (todos os moradores deveriam falar a mesma frase). Aqui é recomendado que seja enviado os dados de um áudio de formato **.wav**. Confira essa [função](https://librosa.github.io/librosa/generated/librosa.core.load.html) em *Python* que permite a abertura de um arquivo de áudio .wav como um vetor de dados. 
>
> ***audioSpeakingName***: o vetor do áudio do morador dizendo o próprio nome. As recomendações sobre o formato de áudio são as mesmas em relação ao atributo *audioSpeakingPhrase*.
>
> **audioSamplerate**: a taxa de amostragem dos áudios atribuídos ao morador. Também é fornecido pela função de abertura de áudio citada na descrição do atributo *audioSpeakingPhrase*

### 2.1.1. Criando um morador

```graphql
mutation createResident(
            $completeName: String!,
            $email: String!,
            $phone: String!,
            $cpf: String!,
            $apartment: String!,
            $block: String!,
            $password: String,
            $audioSpeakingPhrase: [Float]!,
            $audioSpeakingName: [Float]!
            $audioSamplerate: Int
            ){
            createResident(
                completeName: $completeName,
                email: $email,
                cpf: $cpf,
                phone: $phone,
                apartment: $apartment,
                block: $block,
                password: $password,
                audioSpeakingPhrase: $audioSpeakingPhrase,
                audioSpeakingName: $audioSpeakingName
                audioSamplerate: $audioSamplerate
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

### 2.1.2. Consultando um morador
> Disponível apenas para **[administrador](#5-administração)**

Para consultar um morador específico
```graphql
query resident(
    $email: String,
    $cpf: String,
    ){
        resident(
            email: $email,
            cpf: $cpf
        ){
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
```

Para consultar todos os moradores
```graphql
query residents{
    residents{
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
```

### 2.1.3. Modificando os dados de um morador
> Necessário **login** como **morador**.

Assuma que todos os campos enviados nesta mutation serão atualizados no registro do morador com os valores fornecidos.
```graphql
mutation updateResident ($residentData: ResidentInput){
        updateResident (residentData: $residentData){
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
Onde o tipo **ResidentInput** é definido como
```graphql
type ResidentInput {
    completeName: String,
    email: String,
    phone: String,
    cpf: String,
    apartment: String,
    block: String,
    password: String,
    voiceData: String,
    mfccData: String
}
```

### 2.1.4. Deletando um morador
> Disponível apenas para **[administrador](#5-administração)**

```graphql
mutation deleteResident ($email: String!) {
    deleteResident(residentEmail: $email){
        residentEmail
    }
}
```


---
## 2.2. Visitante

Um visitante é uma entidade que representa exatamente o que o seu próprio nome sugere, um visitante. Essa entidade existe para possibilitar a interação de visitantes com o sistema da portaria, a fim de garantir rastreabilidade e segurança. Por padrão, um visitante é representado pelos seguintes atributos:
> ***completeName***: o nome completo do visitante
>
> ***cpf***: o cpf do visitante

### 2.2.1. Criando um visitante
> Disponível apenas para **[administrador](#5-administração)**

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

### 2.2.2. Consultando um visitante
> Disponível apenas para **[administrador](#5-administração)**

Consultando um *visitante* específico
```graphql
query visitor($cpf: String,){
    visitor(cpf: $cpf){
        completeName
        cpf
    }
}
```
Consultando todos os visitantes
```graphql
query allVisitors{
    allVisitors{
        completeName
        cpf
    }
}
```

### 2.2.3. Modificando os dados de um visitante
> Disponível apenas para **[administrador](#5-administração)**

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

### 2.2.4. Deletando um visitante
> Disponível apenas para **[administrador](#5-administração)**
```graphql
mutation deleteVisitor ($cpf: String!) {
    deleteVisitor (cpf: $cpf) {
        cpf
    }
}
```

---
## 2.3. Serviços

Um *serviço* é uma entidade que serve para representar pessoas físicas ou jurídicas cujo interesse é unicamente a prestação de serviços ao condomínio ou a um morador em específico.

A nível de dados, um serviço é representado pelo seguintes atributos:
> ***completeName***: o nome do funcionário ou da empresa
>
> ***email***: o email do funcionário ou da empresa
>
> ***password***: a senha do funcionário ou da empresa

### 2.3.1. Criando um serviço
> Disponível apenas para **[administrador](#5-administração)**
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

### 2.3.2. Consultando um serviço
Para consultar um *serviço* específico
```graphql
query service(
    $completeName: String,
    $email: String,
    ){
        service(
            completeName: $completeName,
            email: $email
        ){
            completeName
            email
        }
    }
```

Para consultar todos os serviços
```graphql
query services{
    services{
        completeName
        email
    }
}
```

### 2.3.3. Modificando os dados de um serviço
> Necessário **login** como serviço

Neste caso a busca pelo *serviço* em específico será feita através dos **dados de login**. O **valor original** de cada atributo do serviço será **substituído pelo valor fornecido** através da mutation. Caso não seja desejável mudar um atributo específico, apenas o omita na mutation.
```graphql
mutation updateService(
        $serviceData: ServiceInput,
    ){
        updateService(
          	serviceData: $serviceData
        ){
            service{
                completeName
                email
            }
        }
    }
```
Onde o tipo **ServiceInput** é definido como
```graphql
type ServiceInput {
    completeName: String,
    email: String,
    password: String
}
```

### 2.3.4. Deletando um serviço
> Disponível apenas para **[administrador](#5-administração)**
```graphql
mutation deleteService ($serviceEmail: String!) {
        deleteService (serviceEmail: $serviceEmail) {
            serviceEmail
        }
    }
```

---

## 2.4. Bloco

Um *Bloco* é uma entidade que representa uma certa área do condomínio. O *Bloco* por padrão, a nível de dados, é representado pelo atributo *número*.

### 2.4.1. Criando um bloco
> Disponível apenas para **[administrador](#5-administração)**
```graphql
mutation createBlock ($number: String!) {
        createBlock (number: $number) {
            number
        }
    }
```
Note que o atributo *number* pode conter letras e símbolos em sua composição.

### 2.4.2. Consultando um bloco

Para consultar um bloco específico
```graphql
query block($number: String!){
    block(number: $number){
        number
    }
}
```
Para consultar todos os blocos
```graphql
query allBlocks{
    allBlocks{
        number
    }
}
```

### 2.4.3. Modificando os dados de um bloco
> Disponível apenas para **[administrador](#5-administração)**
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

### 2.4.4. Deletando um bloco
> Disponível apenas para **[administrador](#5-administração)**
```graphql
mutation deleteBlock ($blockNumber: String!) {
    deleteBlock (blockNumber: $blockNumber) {
        blockNumber
    }
}
```

---
## 2.5. Apartamento

Um *Apartamento* é uma entidade que representa uma unidade de moradia do condomínio. Um apartamento é representado pelo atributo *number* e está diretamente associado a um bloco.

### 2.5.1. Criando um apartamento
> Disponível apenas para **[administrador](#5-administração)**
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
Note que o atributo *number* pode conter letras e símbolos em sua composição. Você pode associar um apartamento a um bloco simplesmente informando o número do bloco desejado.

### 2.5.2. Consultando um apartamento

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
            number
            block{
                number
            }
        }
    }
```
Para consultar **todos** os apartamentos:
```graphql
query allApartments{
    allApartments{
        number
        block{
            number
        }
    }
}
```

### 2.5.3. Modificando os dados de um apartamento
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

#### 2.5.4. Deletando um apartamento
> Disponível apenas para **[administrador](#5-administração)**
```graphql
mutation deleteApartment ($apartmentNumber: String!) {
    deleteApartment (apartmentNumber: $apartmentNumber) {
        apartmentNumber
    }
}
```

---

# 3. Autênticação de usuários

*Alohomora* conta com ferramentas que possibilitam a criação de um sistema de autênticação de usuários. Tais ferramentas podem ser utilizadas para compor desde simples sistemas de autênticação (**aceitar/rejeitar**), até sistemas complexos de várias etapas.

## 3.1. Autênticação de morador

A autênticação de morador pode ser realizada através da **biometria de voz**. Tal coisa é possível pois cada morador, a nível de dados, possui os atributos de áudio que permitem reconhecer características da sua voz.

A *query* possui dois campos obrigatórios e dois optativos:

> ***cpf*** (obrigatório): O CPF do morador
>
> ***audioSpeakingPhrase*** (obrigatório): O vetor de áudio do morador dizendo a frase comum dita no ato do cadastro.
>
> **audioSpeakingName**: O vetor de áudio do morador dizendo o próprio nome
>
> **audioSamplerate**: O samplerate dos áudios que serão enviados  

O **comportamento** interno da *query* **pode variar** dependendo dos campos que são fornecidos. Caso sejam fornecidos apenas os obrigatórios, o morador será autênticado apenas com o áudio da frase comum. Caso seja fornecido também o campo *audioSpeakingName*, o morador será autênticado tanto pelo nome, quanto pela frase comum (ambos devem obter 100% de *score*). Caso o campo *audioSamplerate* seja omitido, a taxa de amostragem dos áudios será considerada como 16000.

No fim, a query retorna **True** caso a voz pertence ao morador ou **False** caso contrário
```graphql
query voiceBelongsResident(
    $cpf: String!,
    $audioSpeakingPhrase: [Float]!,
    $audioSpeakingName: [Float],
    $audioSamplerate: Int
    ){
        voiceBelongsResident(
            cpf: $cpf,
            audioSpeakingPhrase: $audioSpeakingPhrase,
            audioSpeakingName: $audioSpeakingName,
            audioSamplerate: $audioSamplerate
        )
    }
```

# 4. Logs de entrada

*Alohomora* fornece uma ferramenta de registro para entradas de pessoas. Uma *Entrada* é uma entidade que possui relacionamento direto com um apartamento e com um morador ou com um apartamento e com um visitante. A *Entrada* também contém a data e a hora em que foi gerada.

## 4.1. Entradas de morador

Uma *Entrada de Morador* associa um morador a um apartamento em um determinado horário. *Entradas de morador* podem servir como histórico de acessos ao apartamento e consequentemente ao condomínio.

### 4.1.1. Criando uma entrada de morador

```graphql
mutation createEntry(
    $residentCpf: String!,
    $apartmentNumber: String!
    ){
        createEntry(
            residentCpf: $residentCpf,
            apartmentNumber: $apartmentNumber
        ){
            resident{
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
> **Obs**: A data registrada é a data do instante em que a *Entrada* for criada.

### 4.1.2. Consultando uma entrada de morador

Consultando todas as entradas
```graphql
query entries{
    entries{
        resident{
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
        date
    }
}
```

## 4.2. Entradas de visitante

A *Entrada de Visitante* possui um atributo extra em relação à *Entrada de Morador*, que é o ***pending***. Este atributo indica se a entrada ainda está pendente.

A *Entrada de Visitante* pode se comportar também como uma solicitação de entrada, uma vez que um visitante não deveria ter o direito de entrar no condomínio a qualquer momento, mas sim apenas quando este for autorizado.  

### 4.2.1. Criando uma entrada de visitante


```graphql
mutation createEntryVisitor(
    $visitorCpf: String,
    $blockNumber: String,
    $apartmentNumber: String,
    $pending: Boolean,
    ){
        createEntryVisitor(
            visitorCpf: $visitorCpf,
            blockNumber: $blockNumber,
            apartmentNumber: $apartmentNumber,
            pending: $pending
        ){
            visitor{
                completeName
                cpf
            }
            apartment{
                number
                block{
                    number
                }
            }
            pending
        }
    }
```
> **Obs**: A data associada representa a hora em que a entrada foi criada, independentemente do valor do atributo *pending*. Ou seja, podem ocorrer discordâncias entre o instante em que a *Entrada* foi criada e o instante em que o visitante realmente entrou.

### 4.2.2. Consultando uma entrada de visitante

Consultando todas as entradas
```graphql
query allEntriesVisitors{
    allEntriesVisitors{
        visitor{
            completeName
            cpf
        }
        apartment{
            number
            block{
                number
            }
        }
        date
    }
}
```

Consultando todas as entradas pendentes para um apartamento
```graphql
query entriesVisitorsPending(
    $blockNumber: String,
    $apartmentNumber: String,
    ){
        entriesVisitorsPending(
            blockNumber: $blockNumber,
            apartmentNumber: $apartmentNumber
            ){
                visitor{
                    completeName
                    cpf
                }
                apartment{
                    number
                    block{
                        number
                    }
                }
                date
            }
    }
```

Consultando todas as entradas de visitante ou de um apartamento.

```graphql
query entriesVisitor(
    $cpf: String,
    $blockNumber: String,
    $apartmentNumber: String
    ){
        entriesVisitor(
            cpf: $cpf,
            blockNumber: $blockNumber,
            apartmentNumber: $apartmentNumber
        ){
            entryVisitor{
                visitor{
                        completeName
                        cpf
                    }
                    apartment{
                        number
                        block{
                            number
                        }
                    }
                    date
                }
            }
        }
    }
```

Aqui o **comportamento** da *query* irá depender dos **argumentos passados**.

Caso você envie o *cpf*, *blockNumber* e o *apartmentNumber* simultâneamente, serão retornadas todas as entradas do visitante detentor do cpf fornecido ao apartamento indicado pelos argumentos *blockNumber* e *apartmentNumber*.

Caso você envie apenas *blockNumber* e *apartmentNumber*, serão retornadas todas as entradas relacionadas ao apartamento descrito por *blockNumber* e *apartmentNumber*.

Caso você envie apenas o *cpf*, serão retornadas todas as entradas relacionadas ao visitante detentor desse CPF.

---

# 5. Administração

Como todo sistema, *Alohomora* possui algumas rotinas de administração de sistema. Um administrador no contexto dessa aplicação deveria ser alguém da equipe de gerência do condomínio.

> Todas as funcionalidades que requerem **[administrador](#5-administração)** são destinadas ao administrador do sistema.

## 5.1. Criando e deletando administradores

Por padrão a API já vem com uma conta de administrador criada. A partir dela, novas contas personalizadas podem ser criadas.

A conta padrão possui as seguintes credenciais:
> .
> .

### 5.1.1. Criando um administrador

> Disponível apenas para **[administrador](#5-administração)**

```graphql
mutation createAdmin($email: String, $password: String) {
    createAdmin(email: $email, password: $password){
        email
        creator{
            email
            password
        }
    }
}
```

### 5.1.2. Deletando um administrador

> Disponível apenas para **[administrador](#5-administração)**

```graphql
mutation deleteAdmin($email: String) {
    deleteAdmin(email: $email){
        email
    }
}
```

### 5.1.3. Consultando um administrador

> Disponível apenas para **[administrador](#5-administração)**

Consultando todos os administradores
```graphql
query allAdmins{
    allAdmins{
        email
    }
}
```
Consultando administradores com filtro
```graphql
query admin($creatorEmail: String, $adminEmail: String) {
    admin(creatorEmail: $creatorEmail, adminEmail: $adminEmail) {
        email
    }
}
```
Aqui você pode consultar todos o administradores criado pelo administrador detentor do email de valor igual a *creatorEmail* ou simplesmente buscar um administrador detentor do email de valor igual a *adminEmail*.

 Você pode também verificar se um determinado administrador criou um outro; nesse caso, basta enviar um valor para *creatorEmail* e para *adminEmail* simultâneamente.

## 5.2. Gerênciando conta de usuários

Um administrador pode decidir se um determinado usuário possui validez no sistema. Caso um usuário não esteja em estado válido, este não poderá interagir com o sistema.

### 5.2.1. Ativando um usuário

> Disponível apenas para **[administrador](#5-administração)**

```graphql
mutation activateUser($userEmail: String) {
    activateUser(userEmail: $userEmail) {
        user{
            isResident
            isVisitor
            isService
            isAdmin
            isActive
            username
            email
        }
    }
}
```

### 5.2.2. Desativando um usuário

> Disponível apenas para **[administrador](#5-administração)**

```graphql
mutation deactivateUser($userEmail: String) {
    deactivateUser(userEmail: $userEmail) {
        user{
            isResident
            isVisitor
            isService
            isAdmin
            isActive
            username
            email
        }
    }
}
```

### 5.2.3. Consultando usuários desativados

> Disponível apenas para **[administrador](#5-administração)**

```graphql
query unactivesUsers {
    unactivesUsers {
        isResident
        isVisitor
        isService
        isAdmin
        username
        email
    }
}
```

# 6. Exemplos

Nessa seção serão demonstrados vários casos de uso de algumas funcionalidades da API. Os códigos foram escritos em Python, assumindo que a aplicação esteja disponível em http://localhost:8000/graphql.

## 6.1. Criando um morador

Aqui estaremos utilizando os módulos [*speaker verification toolkit*](https://pypi.org/project/speaker-verification-toolkit/) e [*librosa*](https://librosa.github.io/librosa/index.html)  para realizar as devidas manipulações sobre áudios.

```python
import librosa
import speaker_verification_toolkit.tools as svt

api_path = 'http://localhost:8000/graphql'

# Abrindo o audio do morador dizendo a frase comum de autenticacao. Sample rate de 16000
phrase_audio, samplerate = librosa.load('resident_phrase.wav', sr=16000, mono=True)

# Abrindo o audio do morador dizendo o proprio nome. Sample rate de 16000
audio_speaking_name = librosa.load('audio_speaking_name.wav', sr=16000, mono=True)

# Extraindo as caracteristicas MFCC
mfcc_data = svt.extract_mfcc(phrase_audio)
mfcc_audio_speaking_name = svt.extract_mfcc(audio_speaking_name)

# Transformando em JSON
mfcc_data = json.dumps(mfcc_data.tolist())
mfcc_audio_speaking_name = json.dumps(mfcc_audio_speaking_name.tolist())

query = '''
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
'''

# Assumindo que os atributos foram previamente preenchidos:
variables = {
        'completeName': complete_name,
        'email': email,
        'phone': phone,
        'cpf': cpf,
        'apartment': apartment,
        'block': block,
        'mfccData': mfcc_data,
        'mfccAudioSpeakingName': mfcc_audio_speaking_name,
        'password': password
    }

# Finalmente realizando a comunicação com a API
response = requests.post(api_path, json={'query':query, 'variables':variables})
```

## 6.2. Realizando login
Será necessário realizar uma mutation
```
mutation tokenAuth($email: String!, $password: String!){
 tokenAuth(email: $email, password: $password){
   token
 }
}
```
Com o token recebido apartir desta mutation deve ser copiado para o header, para fazer isso pode ser utilizado um programa chamado insomnia. Depois de feito essa etapa, já vai ser possível realizar tarefas com o tipo do usuário logado.

## 6.3. Verificando se o token está correto
```
mutation verifyToken($token: String!){
	verifyToken(token: $token){
	payload
	}
}
```
Com o token recebido, é possível verificar qual conta o token está relacionado.
