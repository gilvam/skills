# Error Handling

Tratamento de erros excessivo, ausente, escondido ou inadequado.

## Smells nesta categoria

- [Overuse of Try-Catch](#overuse-of-try-catch)
- [Excessive Exceptions](#excessive-exceptions)
- [Invalid Error Handling](#invalid-error-handling)
- [Improper Resource Handling](#improper-resource-handling)
- [Incomplete Exception Handling](#incomplete-exception-handling)
- [Unchecked Exceptions](#unchecked-exceptions)
- [Improper Null Handling](#improper-null-handling)
- [Missing Validation](#missing-validation)

---

## Overuse of Try-Catch

Ocorre quando o bloco de código usa a estrutura try-catch de maneira excessiva, mesmo para situações que não requerem tratamento de exceções. Isso pode gerar uma sobrecarga no código, diminuir a legibilidade e prejudicar o desempenho. Usar try-catch de maneira inadequada pode ocultar falhas inesperadas ou criar um fluxo de controle não claro.

```typescript
interface Processable { ... }

function processData(data: Processable): string {
  try {
    let processedData = parseData(data); // não precisa do try-catch here
    return `Processed Data: ${processedData}`;
  } catch (error) {
    console.error('Error during data processing', error);
    return 'Error during processing';
  }

  try {
    let result = computeResult(data); // não precisa
    return `Result: ${result}`;
  } catch (error) {
    console.error('Error during result computation', error);
    return 'Error during computation';
  }

  try {
    let formattedResult = formatResult(data); // não precisa
    return `Formatted Result: ${formattedResult}`;
  } catch (error) {
    console.error('Error during result formatting', error);
    return 'Error during formatting';
  }
}

// solução

function processData(data: Processable): string {
  const processedData = parseData(data);
  const result = computeResult(data);
  const formattedResult = formatResult(data);
  return `Processed Data: ${processedData}, Result: ${result}, Formatted Result: ${formattedResult}`;
}
```

## Excessive Exceptions

Ocorre quando uma aplicação lança um número excessivo de exceções, muitas vezes por situações que podem ser tratadas de outras maneiras mais eficazes ou sem o uso de exceções. Lançar exceções frequentemente pode tornar o código mais difícil de entender, diminuir a performance e tornar o gerenciamento de erros mais complexo do que realmente deveria ser.
Exceções são projetadas para situações excepcionais e imprevistas, e não para controle normal de fluxo ou para eventos que podem ser previstos e tratados de forma mais apropriada. O uso excessivo de exceções pode gerar sobrecarga no código e no desempenho da aplicação, especialmente em linguagens como JavaScript/TypeScript, onde as exceções podem ser caras em termos de desempenho.

```typescript
class OrderProcessor {
  processOrder(orderId: string) {
    try {
      if (orderId === '') {
        throw new Error('Order ID cannot be empty');
      }
      if (orderId === 'invalid') {
        throw new Error('Order ID is invalid');
      }
      if (orderId === 'unavailable') {
        throw new Error('Order ID is unavailable');
      }
      ...
      console.log(`Order ${orderId} processed successfully`);
    } catch (error) {
      console.error('Error processing order:', error.message);
    }
  }
}
// solução

class OrderProcessor {
  processOrder(orderId: string) {
    if (orderId === '') {
      console.error('Error: Order ID cannot be empty');
      return;
    }
    if (orderId === 'invalid') {
      console.error('Error: Order ID is invalid');
      return;
    }
    if (orderId === 'unavailable') {
      console.error('Error: Order ID is unavailable');
      return;
    }

    ...

    console.log(`Order ${orderId} processed successfully`);
  }
}
```

## Invalid Error Handling

Ocorre quando o tratamento de erros é inadequado ou mal implementado, causando comportamentos inesperados, perda de informações importantes ou dificultando a depuração. Exemplos comuns incluem:
Uso excessivo de catch genérico.
Supressão de erros sem registro ou notificação.
Lançar erros genéricos em vez de específicos.
Uso indevido de estruturas de controle para mascarar erros.

```typescript
class FileManager {
  readFile(filePath: string): string {
    try {
      if (!filePath) {
        throw new Error("File path is missing");
      }
      return "File content"; // simulação de retorno do arquivo
    } catch (error) {
      console.log("An error occurred");
      return ""; // Retorna um valor vazio sem tratar o problema
    }
  }
}
// solução

class FileManager {
  readFile(filePath: string): string {
    try {
      if (!filePath) {
        throw new Error("File path is missing");
      }
      return "File content";
    } catch (error) {
      // Registra o erro com detalhes
      console.error(`Error reading file: ${(error as Error).message}`);
      throw new Error(
        "Failed to read file. Please check the file path and try again."
      );
    }
  }
}
```

## Improper Resource Handling

Ocorre quando recursos como arquivos, conexões de rede, ou conexões de banco de dados não são gerenciados corretamente, levando a vazamentos de recursos, bloqueios ou outros problemas de desempenho. Recursos precisam ser abertos e fechados de forma apropriada para garantir que sejam liberados corretamente, mesmo em caso de erros.

```typescript
readFile(filePath: string): string {
  const fileHandle = openFile(filePath); // abre o arquivo
  const data = fileHandle.read();
  // não fecha o arquivo, o que pode causar vazamentos de recursos
  return data;
}
// solução

readFile(filePath: string): string {
  const fileHandle = openFile(filePath);
  try {
    return fileHandle.read();
  }
  catch(err) {
    ...
  }
  finally {
    fileHandle.close(); // fecha o arquivo, mesmo em caso de erros
  }
}
```

## Incomplete Exception Handling

Ocorre quando um programa lança ou captura exceções de forma incompleta, sem tratá-las adequadamente. Isso pode ocorrer quando as exceções são capturadas sem uma lógica clara de tratamento ou quando exceções são lançadas mas não são manipuladas corretamente. Isso pode levar a falhas no sistema, mensagens de erro genéricas ou comportamento inesperado do programa.

```typescript
readFile(fileName: string): string {
  try {
    const fs = require('fs');
    return fs.readFileSync(fileName, 'utf8');
  } catch (error) {
    // a exceção é capturada mas sem tratar adequadamente
    console.error('Erro ao ler o arquivo.');
    return '';
  }
}
// solução

readFile(fileName: string): string {
  const fs = require('fs');

  try {
    return fs.readFileSync(fileName, 'utf8');
  } catch (error) {
    // fornecendo informações detalhadas sobre o erro
    console.error(`Erro ao ler o arquivo: ${error.message}`);
    // lançando novamente a exceção, assim o chamador cria o tratamento
    throw new Error(`Não foi possível ler o arquivo ${fileName}`);
  }
}
```

## Unchecked Exceptions

_Também conhecido como: Empty Catch Block._

Ocorrem quando exceções são lançadas ou propagadas sem serem verificadas ou tratadas adequadamente, muitas vezes sem a devida tentativa de captura ou de fornecimento de informações suficientes para o diagnóstico do problema.
Essas exceções são chamadas de "unchecked" porque o compilador não exige que elas sejam tratadas explicitamente em um bloco try-catch ou que sejam declaradas como parte de uma assinatura de função, como ocorre com exceções verificadas (checked exceptions) em outras linguagens, como Java.

```typescript
divide(a: number, b: number): number {
  if (b === 0) {
    throw new Error("Divisão por zero não é permitida.");
  }
  return a / b;
}

process(): void {
  // a exceção não é tratada, o código vai quebrar aqui
  const result = divide(10, 0);
}
// solução

divide(a: number, b: number): number {
  if (b === 0) {
    throw new Error("Divisão por zero não é permitida.");
  }
  return a / b;
}

process() {
  try {
    const result = divide(10, 0);  // a exceção é capturada aqui
    ...
  } catch (error) {
    // feedback claro para o usuário
    console.error("Erro ao processar a divisão: ", error.message);
    ...
  }
}
```

## Improper Null Handling

Ocorre quando o código não lida corretamente com valores null ou undefined, o que pode causar falhas inesperadas em tempo de execução. Esse code smell geralmente é associado a verificações insuficientes para valores nulos ou à falta de tratamento adequado de objetos que podem ser null ou undefined.

```typescript
interface User {
  name: string;
  address?: { street: string; city: string };
}

function printCity(user: User): void {
  // Erro: Cannot read property 'city' of undefined
  console.log(user.address.city);
}

// exemplo de uso
const user: User = { name: "Alice" };
printCity(user); // o código quebra porque `address` é `undefined`
// solução

class Address {
  constructor(public street = '', public city = '') {}

  get isEmpty(): boolean {
    return !(this.street && this.city);
  }
}

class User {
  constructor(public name: string, public address = new Address()) {}

  printCity(): void {
    if (!this.address.isEmpty) {
      console.log(this.address.city);
    } else {
      console.warn("Endereço não disponível.");
    }
  }
}

// exemplo de uso
const user = new User("Alice");
user.printUserCity();  // Saída: "Endereço não disponível"

const userWithAddress = new User("Bob", new Address("123 Main St", "New York"));
userWithAddress.printCity();  // Saída: "New York"
```

## Missing Validation

Ocorre quando o código não realiza a validação adequada dos dados de entrada. A validação de dados é essencial para garantir que os dados recebidos em um sistema estejam no formato esperado e que não causem comportamentos inesperados, erros ou falhas de segurança. Falhar em validar entradas pode resultar em falhas de segurança, erros inesperados e problemas de integridade de dados.
Validar os dados de entrada pode incluir verificações de tipo, comprimento, formato, valores válidos e até mesmo a prevenção de ataques, como injeções de SQL ou XSS (Cross-Site Scripting).
Problemas principais:
Erros no processamento: Dados inválidos podem levar a falhas, exceções ou comportamentos inesperados no sistema.
Vulnerabilidades de segurança: A falta de validação pode permitir ataques como injeção de código, XSS, ou SQL Injection.
Problemas de integridade de dados: Dados incorretos ou malformados podem comprometer a integridade da base de dados e dos sistemas que dependem dessas informações.
Experiência do usuário comprometida: O sistema pode apresentar resultados inesperados ou falhas para o usuário final.

```typescript
registerUser(user: { name: string; email: string }) {
  const userDatabase = [];
  // adicionando diretamente o usuário sem verificar o formato do email ou nome
  userDatabase.push(user);
  console.log('User registered successfully!');
}

// exemplo de uso
const user = new User('Alice', 'alice@example.com');
registerUser(user);
// solução

isValidEmail(email: string): boolean {
  const emailRegex = /^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,6}$/;
  return emailRegex.test(email);
}

isValidName(name: string): boolean {
  return name.trim().length > 0;
}

registerUser(user: User) {
  if (!isValidName(user.name)) { // validação do nome
    console.warn('Invalid name.');
    return;
  }

  if (!isValidEmail(user.email)) { // validação do email
    console.warn('Invalid email format.');
    return;
  }

  const userDatabase = [];
  userDatabase.push(user);  // só registra se os dados forem válidos
  console.log('User registered successfully!');
}

// exemplo de uso
const user = new User('Alice', 'alice@example.com');
registerUser(user);  // registro bem-sucedido

const invalidUser = new User('', 'invalid');
registerUser(invalidUser);  // exibe 'Invalid name.' e 'Invalid email format.'
```
