# Naming & Readability

Nomes confusos, comentários ruins e formatação inconsistente que prejudicam a leitura do código.

## Smells nesta categoria

- [Inconsistent Naming](#inconsistent-naming)
- [Confusing Method Signatures](#confusing-method-signatures)
- [Boolean Trap](#boolean-trap)
- [Boolean Variable Naming](#boolean-variable-naming)
- [Non-Descriptive Names](#non-descriptive-names)
- [Imperative Comments](#imperative-comments)
- [Outdated Comments](#outdated-comments)
- [Inconsistent Indentation](#inconsistent-indentation)
- [Poor Error Messages](#poor-error-messages)

---

## Inconsistent Naming

_Também conhecido como: Repetitive Naming._

Inconsistência nos nomes ou repetição desnecessária do nome da classe, variáveis ou funções, causando redundância e dificultando a manutenção do código

```typescript
// má prática:

class Book {
  // repetição desnecessária de 'Book' nos parâmetros e variáveis
  constructor(public bookName: string, public bookAuthor: string) {
    this.bookName = bookName;
    this.bookAuthor = bookAuthor;
  }

  getBookName(): string {  // repetição desnecessária de 'Book'
    return this.bookName;
  }

  getBookAuthor(): string {  // repetição...
    return this.bookAuthor;
  }

  setBookName(bookName: string): void {  // repetição...
    this.bookName = bookName;
  }

  setBookAuthor(bookAuthor: string): void {  // repetição...
    this.bookAuthor = bookAuthor;
  }
}

// exemplo de uso
const book = new Book('name', 'Jhon');
book.getBookName(); // já sabemos que é um book
// solução

class Book {

  constructor(public name: string, public author: string) {
    this.name = name;
    this.author = author;
  }

  getName(): string {
    return this.name;
  }

  getAuthor(): string {
    return this.author;
  }

  setName(name: string): void {
    this.name = name;
  }

  setAuthor(author: string): void {
    this.author = author;
  }
}

// exemplo de uso
const book = new Book('name', 'Jhon');
book.getName();
```

## Confusing Method Signatures

Similar ao Long Parameter List, ocorre quando os métodos de uma classe têm assinaturas que são difíceis de entender, seja por serem muito longas, terem parâmetros mal nomeados, ou uma ordem de parâmetros confusa. Isso torna o código difícil de usar e compreender, aumentando a chance de erros e dificultando a manutenção.

```typescript
class ProductService {

  // 1. muitos parâmetros sem clareza
  addProduct(id: number, name: string, price: number, stock: number, category: string, discount: number, color: string, weight: number, size: string, brand: string): void { ... }

  // 2. ordem dos parâmetros confusa
  calculateDiscount(price: number, discount: number, quantity: number, category: string, customerType: string, promoCode: string): number {
      // lógica para calcular o desconto
      return price * (1 - discount) * quantity;
  }

  // 3. nomes de parâmetros genéricos
  processOrder(a: number, b: string, c: boolean, d: any, e: string): void { ... }

  // 4. parâmetros com diferentes tipos, sem contexto
  updateProduct(id: string, price: number, availability: boolean, date: Date, description: string): void { ... }

  // 5. método com muitos parâmetros, alguns não claros
  handleTransaction(transactionId: number, userId: number, amount: number, transactionType: string, paymentMethod: string, status: string, date: Date): void { ... }

  // 6. parâmetros do tipo "boolean" com nomes não informativos
  createAccount(userId: string, active: boolean, verified: boolean, subscription: boolean): void { ... }

  // 7. parâmetros com nomes confusos
  setOrderStatus(orderTId: number, flg: boolean, ste: string, ttamp: Date): void { ... }

  // 8. muitos parâmetros para um simples cálculo
  calculateTotalCost(price: number, taxRate: number, shippingCost: number, promoCode: string, discountRate: number, isGift: boolean, membershipStatus: string, quantity: number): number {
      return price * quantity + shippingCost + (price * taxRate) - discountRate;
  }

  // 9. parâmetros não relacionados juntos
  generateReport(startDate: Date, endDate: Date, format: string, includeCharts: boolean, userId: string, reportType: string): void { ... }

  // 10. uso de um parâmetro sem contexto claro
  updateUserSettings(userId: string, settings: any, type: string, active: boolean, saveImmediately: boolean): void { ... }
}
// solução

class ProductDetails { ... }
class Details { ... }
class Transaction { ... }
class AccountStatus { ... }
class Order { ... }
class ReportDetails { ... }
class Settings { ... }

class ProductService {

  // 1. simplificando com objeto
  addProduct(product: ProductDetails): void { ... }

  // 2. melhorando a clareza dos parâmetros
  calculateDiscount(price: number, discount: number, quantity: number): number {
      // lógica para calcular o desconto
      return price * (1 - discount) * quantity;
  }

  // 3. parâmetros mais descritivos
  processOrder(orderId: number, customerName: string, isUrgent: boolean, orderDetails: any): void { ... }

  // 4. melhorando a clareza dos parâmetros
  updateProduct(productId: string, updatedDetails: Details): void { ... }

  // 5. usando objetos para transações
  handleTransaction(transaction: Transaction): void { ... }

  // 6. nomeando parâmetros com mais clareza
  createAccount(userId: string, accountStatus: AccountStatus): void { ... }

  // 7. parâmetros com nomes mais claros
  setOrderStatus(orderId: number, status: string, timestamp: Date): void { ... }

  // 8. agrupando parâmetros de custo em objeto
  calculateTotalCost(order: Order): number {
    // lógica para calcular o custo total
    return order.price * order.quantity + order.shippingCost + (order.price * order.taxRate) - order.discountRate;
  }

  // 9. agrupando parâmetros relacionados ao relatório
  generateReport(reportDetails: ReportDetails): void { ... }

  // 10. usando objeto para configurações do usuário
  updateUserSettings(userId: string, settings: Settings): void { ... }
}
```

## Boolean Trap

Ocorre quando se usa uma variável booleana de forma inadequada, levando a um código confuso, redundante ou propenso a erros. Esse problema ocorre, principalmente, quando a variável booleana é usada em expressões ou condições complexas de maneira que causa ambiguidades ou torna o código difícil de entender.
A principal questão aqui é que, ao usar variáveis booleanas de maneira inadequada, pode-se criar armadilhas lógicas, onde o comportamento do código não é claro ou é inesperado.

```typescript
class User {
    constructor(private isActive: boolean, private isSuspended: boolean) {}

    checkUserStatus(): boolean {
        // armadilha booleana: a negação dupla não faz sentido
        return !(this.isActive && !this.isSuspended); 
    }
}

// exemplo de uso
const user = new User(true, false);
console.log(user.checkUserStatus());
// solução

class User {
    constructor(private isActive: boolean, private isSuspended: boolean) {}

    public isUserActive(): boolean {
        // Solução: A lógica é clara e direta
        return this.isActive && !this.isSuspended;
    }
}

// exemplo de uso
const user = new User(true, false);
console.log(user.isUserActive());
```

## Boolean Variable Naming

_Também conhecido como: Boolean Method Naming._

Ocorre quando uma variável booleana ou um método booleano não segue a convenção de nomenclatura esperada, o que pode causar confusão no código. Para tornar o código mais legível, é recomendado que variáveis e métodos booleanos comecem com os prefixos is, has, can, ou outros prefixos semelhantes, para indicar que o valor ou a operação é do tipo booleano e para expressar claramente o que está sendo verificado ou afirmado.

```typescript
class User {
    // nome não apropriado para uma variável booleana
    constructor(private flag: boolean) {}

    checkStatusActive(): boolean {
        return this.flag;
    }
}

const user = new User(true);
console.log(user.checkStatusActive()); // Não é claro que "flag" é um boleano
// solução

class User {
    // nome de variável booleana com prefixo 'is'
    constructor(private isActive: boolean) {}

    isActive(): boolean {
        return this.isActive;
    }
}

const user = new User(true);
console.log(user.isActive());  // Agora é claro que "isActive" booleano
```

## Non-Descriptive Names

Ocorre quando nomes de variáveis, funções, classes ou outros elementos do código são vagos, confusos ou não representam claramente sua finalidade. Isso torna o código difícil de entender, mantendo e propenso a erros.

```typescript
class A {
  private x: number;
  private y: number;

  constructor(a: number, b: number) {
    this.x = a;
    this.y = b;
  }

  public doThing(): number {
    return this.x + this.y;
  }
}

// exemplo de uso
const obj = new A(5, 10);
console.log(obj.doThing()); // o que "doThing" faz?
// solução

class Point {
  private xCoordinate: number;
  private yCoordinate: number;

  constructor(xCoordinate: number, yCoordinate: number) {
    this.xCoordinate = xCoordinate;
    this.yCoordinate = yCoordinate;
  }

  public calculateSum(): number {
    return this.xCoordinate + this.yCoordinate;
  }
}

// exemplo de uso
const point = new Point(5, 10);
console.log(`Sum of coordinates: ${point.calculateSum()}`);
```

## Imperative Comments

Ocorre quando os comentários no código são usados para instruir como o código deve funcionar em vez de simplesmente explicar ou documentar. Isso é um problema porque os comentários podem se tornar desatualizados ou desnecessários, especialmente quando o código é alterado. Além disso, um código bem escrito deve ser claro por si só, com comentários sendo usados apenas para explicar o "porquê", e não o "como".
Sem exemplos de código.

## Outdated Comments

Ocorre quando os comentários no código não refletem mais o comportamento ou o propósito do código. Isso pode causar confusão, levar a suposições erradas e dificultar a manutenção. Geralmente, isso acontece quando o código é alterado, mas os comentários não são atualizados.
Sem exemplos de código.

## Inconsistent Indentation

Ocorre quando o código tem indentação inconsistente, o que torna o código difícil de ler, entender e manter. Isso pode acontecer por várias razões, como misturar espaços e tabulações, usar diferentes estilos de indentação (por exemplo, 2 espaços em alguns lugares e 4 espaços em outros), ou simplesmente não seguir um padrão claro para toda a base de código.

```typescript
class User {
   name: string;
 age: number;

  constructor(name: string,age: number) {
          this.name = name;
 age = age;
}
    getName():string {
 return this.name;
  }
    }
// solução

class User {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    age = age;
  }

  getName(): string {
    return this.name;
  }
}
```

## Poor Error Messages

São aquelas que fornecem informações insuficientes, vagas ou confusas sobre o que deu errado em uma aplicação. Isso dificulta a depuração e a solução de problemas, tanto para desenvolvedores quanto para usuários finais.
Problemas principais:
Dificuldade para identificar a causa raiz do erro: Mensagens vagas ou incompletas não ajudam os desenvolvedores a entender rapidamente o problema.
Frustração do usuário: Se um usuário encontra uma mensagem de erro incompreensível, isso pode gerar frustração e dificultar a resolução do problema.
Aumento no tempo de resolução de problemas: Erros com mensagens imprecisas podem levar mais tempo para serem diagnosticados e corrigidos.

```typescript
calculatePrice(price: number, tax: number): number {
  if (price < 0 || tax < 0) {
    throw new Error('Error');
  }
  return price + (price * tax);
}

// exemplo de uso
try {
  const total = calculatePrice(-10, 0.15); // valor negativo, o que gera erro
} catch (error) {
  console.log(error.message); // exibe apenas 'Error', sem detalhes úteis
}
// solução

calculatePrice(price: number, tax: number): number {
  if (price < 0) {
    throw new Error(`Invalid price: ${price}. Price must be a non-negative number.`);
  }
  if (tax < 0) {
    throw new Error(`Invalid tax: ${tax}. Tax must be a non-negative number.`);
  }
  return price + (price * tax);
}

// exemplo de uso
try {
  const total = calculatePrice(-10, 0.15); // valor negativo, o que gera erro
} catch (error) {
  console.log(error.message); // mensagem de erro mais clara e útil
}
```
