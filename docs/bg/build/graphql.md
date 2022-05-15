# GraphQL схема

## Дефиниране на обекти

Файлът `schema.graphql` дефинира различните схеми на GraphQL. Поради начина, по който езикът за заявки GraphQL работи, файлът със схемата по същество диктува формата на вашите данни от SubQuery. За да научите повече за това как да пишете на езика на схемите GraphQL, препоръчваме да разгледате [Схеми и типове](https://graphql.org/learn/schema/#type-language).

**Важно: Когато правите промени във файла на схемата, моля, уверете се, че регенерирате директорията си с типове със следната команда `yarn codegen`**

### Oбекти
Всеки обект трябва да дефинира своите задължителни `id` полета с типа `ID!`. Използва се като първичен ключ и e уникален сред всички обекти от един и същи тип.

Не-нулеви полета в обекта са обозначени с `!`. Моля, вижте примера по-долу:

```graphql
type Example @entity {
  id: ID! # id полето е винаги задължително и трябва да изглежда така
  name: String! # Това е задължително поле 
  address: String # Това е задължително поле 
}
```

### Поддържани скалари и типове

Понастоящем поддържаме следните типове скалари:
- `ID`
- `Int`
- `String`
- `BigInt`
- `Float`
- `Date`
- `Boolean`
- `<EntityName>` за вече вложени, свързани обекти, можете да използвате името на дефинирания обект като едно от полетата. Моля, вижте във [Свързване на обектите](#entity-relationships).
- `JSON` може алтернативно да съхранява структурирани данни, моля, вижте [JSON type](#json-type)
- `<EnumName>` типовете, са специален вид скалар, който е ограничен до определен набор от разрешени стойности. Моля, вижте [Graphql Enum](https://graphql.org/learn/schema/#enumeration-types)

## Индексиране по поле, което не е първичен ключ

За да подобрите производителността на заявката, индексирайте поле на обект, просто като внедрите анотацията `@index` в поле, което не е първичен ключ.

Въпреки това, ние не позволяваме на потребителите да добавят анотация `@index` към всеки [JSON](#json-type) обект. По подразбиране индексите се добавят автоматично към външни ключове и за JSON полета в базата данни, но само за подобряване на производителността на услугата за заявки.

Ето един пример.

```graphql
type User @entity {
  id: ID!
  name: String! @index(unique: true) # уникален може да бъде зададен на true или false
  title: Title! # Индексите се добавят автоматично към полето за външен ключ 
}

type Title @entity {
  id: ID!  
  name: String! @index(unique:true)
}
```
Ако приемем, че знаем името на този потребител, но не знаем точната стойност на идентификатора, вместо да извлечем всички потребители и след това да филтрираме по име, можем да добавим `@index` зад полето за име. Това прави заявките много по-бързи и можем допълнително да придадем `unique: true` за да гарантираме уникалност.

**Ако полето не е уникално, максималният размер на набора от резултати е 100**

Когато се стартира генерирането на код, това автоматично ще създаде`getByName` под `User` модела, а полето за външен ключ `title` ще създаде `getByTitleId` метод, чрез който и двата могат да бъдат директно достъпни във функцията за съпоставяне.

```sql
/* Подгответе запис за обект на заглавие */
INSERT INTO titles (id, name) VALUES ('id_1', 'Captain')
```

```typescript
// Манипулатор във функция за съпоставяне
import {User} from "../types/models/User"
import {Title} from "../types/models/Title"

const jack = await User.getByName('Jack Sparrow');

const captainTitle = await Title.getByName('Captain');

const pirateLords = await User.getByTitleId(captainTitle.id); // List of all Captains
```

## Взаимоотношения на субекта

Един обект често има заложени връзки с други обекти. Задаването на стойността на полето на обект с друго име ще дефинира връзка едно към едно между тези два обекта по подразбиране.

Различни връзки на обекти (едно към едно, едно към много и много към много) могат да бъдат конфигурирани с помощта на примерите по-долу.

### Взаимоотношения едно към едно

Взаимоотношенията едно към едно са по подразбиране, когато само един обект е преобразуван в друг.

Пример: Паспортът ще принадлежи само на един човек и човекът има само един паспорт (в този пример):

```graphql
type Person @entity {
  id: ID!
}

type Passport @entity {
  id: ID!
  owner: Person!
}
```

or

```graphql
type Person @entity {
  id: ID!
  passport: Passport!
}

type Passport @entity {
  id: ID!
}
```

### Взаимоотношения едно към много

Можете да използвате квадратни скоби, за да посочите, че даден тип поле включва множество обекти.

Пример: Едно лице може да има няколко акаунта.

```graphql
type Person @entity {
  id: ID!
  accounts: [Account]! @derivedFrom(field: "person") #This is virtual field 
}

type Account @entity {
  id: ID!
  person: Person!
}
```

### Взаимоотношения много към много
Връзка много към много може да бъде постигната чрез внедряване на обект на преобразуване, за свързване на другите две единици.

Пример: Всеки човек е част от множество групи (PersonGroup) и групите имат множество различни хора (PersonGroup).

```graphql
type Person @entity {
  id: ID!
  name: String!
}

type PersonGroup @entity {
  id: ID!
  person: Person!
  Group: Group!
}

type Group @entity {
  id: ID!
  name: String!
}
```

Също така е възможно да се създаде връзка на един и същ обект в множество полета на средния обект.

Например, една сметка може да има множество преводи и всеки превод има изходен и целеви акаунт.

Това ще установи двупосочна връзка между два акаунта (от и до) чрез таблицата за прехвърляне.

```graphql
type Account @entity {
  id: ID!
  publicAddress: String!
}

type Transfer @entity {
  id: ID!
  amount: BigInt
  from: Account!
  to: Account!
}
```

### Обратни търсения

За да активирате обратното търсене на обект към дадена зависимост, прикачете `@derivedFrom` към полето и посочете неговото поле за обратно търсене на друг обект.

Това създава виртуално поле на обекта, което може да бъде запитано.

Прехвърлянето „от“ акаунт е достъпно от обекта на акаунта, като зададете изпратения трансфер или получен трансфер като стойността им, извлечена от съответните полета от или до.

```graphql
type Account @entity {
  id: ID!
  publicAddress: String!
  sentTransfers: [Transfer] @derivedFrom(field: "from")
  receivedTransfers: [Transfer] @derivedFrom(field: "to")
}

type Transfer @entity {
  id: ID!
  amount: BigInt
  from: Account!
  to: Account!
}
```

## Тип JSON

Поддържаме записването на данни като тип JSON, което е бърз начин за съхраняване на структурирани данни. Ние автоматично ще генерираме съответните JSON интерфейси за запитване на тези данни и ще ви спестим време за дефиниране и управление на обекти.

Препоръчваме на потребителите да използват типа JSON в следните сценарии:
- Когато съхраняването на структурирани данни в едно поле е по-управляемо, отколкото създаването на множество отделни обекти.
- Запазване на произволни потребителски предпочитания за ключ/стойност (където стойността може да бъде булева, текстова или числова и не искате да имате отделни колони за различни типове данни)
- Схемата е непостоянна и се променя често

### Дефинирайте JSON директива
Дефинирайте свойството като тип JSON, като добавите анотацията `jsonField` в обекта. Това автоматично ще генерира интерфейси за всички JSON обекти във вашия проект под `types/interfaces.ts`, и можете да получите достъп до тях във вашата функция за преобразуване.

За разлика от стандартния обект, обектът на директивата `id` не изисква никакво поле за идентификатор. JSON обект също може да се влага с други JSON обекти.

````graphql
въведете  AddressDetail @jsonField {
  street: String!
  district: String!
}

type ContactCard @jsonField {
  phone: String!
  address: AddressDetail # Nested JSON
}

type User @entity {
  id: ID! 
  contact: [ContactCard] # Store a list of JSON objects
}
````

### Запитване на JSON полета

Недостатъкът на използването на JSON типове е леко въздействие върху ефективността на заявката при филтриране, тъй като всеки път, когато извършва текстово търсене, това е върху целия обект.

Въпреки това, въздействието все още е приемливо в нашата услуга за запитвания. Ето пример за това как да използвате оператора `contains` в заявката GraphQL в JSON поле, за да намерите първите 5 потребители, които притежават телефонен номер, който съдържа '0064'.

```graphql
#За да намерите първите 5 потребители, със собствени телефонни номера, които съдържат '0064'.

query{
  user(
    first: 5,
    filter: {
      contactCard: {
        contains: [{ phone: "0064" }]
    }
}){
    nodes{
      id
      contactCard
    }
  }
}
```