# Funções 
Como fazer uma função transmitir seu propósito? Quais atributos dar às nossas funções que permitirão um leitor comum deduzir o tipo de programa ali contido? Essas perguntas serão respondidas nesse doc, sendo ele um resumo do capítulo 3 de Clean Code.

##  Pequenas
A primeira regra para funções é que elas devem ser pequenas, elas devem ter no máximo 20 linhas. Porém, o ideal seria 3 a 4 linhas. A refatoração abaixo mostra como chegar nesse ideal:

```java 
# Listagem 3.1
public static String renderPageWithSetupAndTeardowns(
    PageData PageData, boolean isSuite)throws Exception {
    boolean isTestPage = pageData.getWikiPage();
    if (isTestPage) {
        WikiPage testPage = pageData.getWikiPage();
        StringBuffer newPageContent = new StringBuffer();
        includeSetupPages(testPage, newPageContent, isSuite);
        newPageContent.append(pageData.getContent());
        includeTeardownPages(testPage, newPageContent, isSuite);
        pageData.setContent(newPageContent.toString())
    }
    return pageData.getHtml();
}
```
```java
public static String renderPageWithSetupAndTeardowns(
    PageData pageData, boolean isSuite) throws Exception {
    if(isTestPage(pageData))
        includeSetupAndTeardownPages(pageData, isSuite);
    return pageData.getHtml();
}
```
## Blocos de endentação
Os blocos de instrução `if`, `else`, `while` e outros devem ter apenas uma linha. Possivelmente uma chamada de função. Além de manter a função pequena, isso adiciona um valor significativo, pois a função chamada de dentro o bloco pode receber um nome descritivo. Isso também implica que as funções não devem ser grandes e ter estruturas aninhadas.

## Faça apenas uma coisa 
As funções devem fazer uma coisa. Devem fazê-la bem. Devem fazer apenas ela. Mas o que seria apenas uma coisa? Na listagem 3.1 é fácil dizer que ela faz três:
- Determina se a página é de teste.
- Se for, inclui SetUps e TearDowns
- Exibe a página em html

Então, uma ou três? Se uma função faz apenas aqueles passos em um nível abaixo do nome da função, então ela está fazendo uma só coisa, como ocorre na renderPageWithSetupAndTeardowns.
Outra forma de saber se uma função faz mais de "uma coisa" é se você pode extrair outra função dela a partir de seu nome que não seja apenas uma reformulação.

## Um nível de abstração por função
A fim de confirmar se as funções fazem só uma coisa, precisamos verificar se todas as instruções estão em um mesmo nível de abstração. Por exemplo, existem  conceitos que estão em um alto nível de abstração como getHtml() no java  e outros com um nível mais baixo como o .append(). Ou então um gerenciamento de estado (intermediário) e a utilização de refs (baixo nível) no react.  Vários níveis dentro de uma função gera confusão.

## Ler o código de cima para baixo: Regra decrescente
É importante estruturar o código de forma que ele seja lido de cima para baixo, como uma narrativa, com níveis de abstração decrescentes à medida que o código progride. Isso torna o código mais claro, legível e fácil de entender para outros desenvolvedores. Abaixo um exemplo:

```java
public class RectangleAreaCalculator {

    public static void main(String[] args) {
        double width = 5.0;
        double height = 3.0;

        double area = calculateRectangleArea(width, height);

        displayRectangleArea(width, height, area);
    }

    // Função de alto nível para calcular a área do retângulo
    public static double calculateRectangleArea(double width, double height) {
        return width * height;
    }

    // Função intermediária para exibir a área do retângulo
    public static void displayRectangleArea(double width, double height, double area) {
        System.out.println("Retângulo com largura " + width + " e altura " + height);
        System.out.println("Área do retângulo: " + area);
    }
}
```
## Estrutura Switch
É difícil criar uma estrutura switch pequena, pois mesmo uma com apenas dois cases é maior que o desejado no bloco ou função. Porém, podemos nos certificar se cada switch está em uma classe de baixo nível e nunca é repetido. Para isso usamos o poliformismo.
Veja a listagem abaixo, ela mostra apenas uma das operações que podem depender do tipo Employee:

```java
public Money calculatePay(Employee e)
throws InvalidEmployeeType  {
    switch (e.type) {
        case COMMISSIONED:
            return calculateCommissionedPay(e);
        case HOURLY:
            return calculateHourlyPay(e);
        case SALARIED:
            return calculateSalariedPay(e);
        default:
            throw new InvalidEmployeeType(e.type);
    }
}
```
Essa função tem vários problemas. Primeiro, ela é grande, e pode crescer mais ainda. Segundo, ela faz mais de uma coisa. Terceiro, ela viola o Princípio de Responsabilidade  Única(SRP) por haver mais de um motivo para alterá-la. Quarto, ela viola o Princípio de Aberto-Fechado(OCP) pois precisa ser modificada sempre que novos tipos forem adicionados.

Uma solução é inserir a estrutura switch no fundo de uma ABSTRACT FACTORY e jamais que alguém veja. A factory usará o switch para criar instâncias apropriadas derivadas de Employee, e as funções, como calculatePay, isPayday e deliveryPay, serão enviadas de forma polimórfica através da interface Employee:

```java
public abstract class Employee {
    public abstract boolean isPayday();
    public abstract Money calculatePay();
    public abstract void deliveryPay(Money pay);
}
```
Vamos usar um exemplo mais simples para ilustrar como o padrão Abstract Factory  e o polimorfismo que podem ser aplicados sem a necessidade de usar um switch.

1. **Definindo a classe abstrata PaymentProcessor:**

```java
// AbstractProduct - PaymentProcessor (Classe abstrata)
abstract class PaymentProcessor {
    public abstract void processPayment(double amount);
}
```

2. **Criando as subclasses concretas para cada meio de pagamento:**

```java
// ConcreteProduct - CreditCardPaymentProcessor (Subclasse de PaymentProcessor)
class CreditCardPaymentProcessor extends PaymentProcessor {
    public void processPayment(double amount) {
        // Implementação específica para processar pagamento com cartão de crédito
        // Pode envolver comunicação com gateway de pagamento, verificar informações do cartão, etc.
        System.out.println("Pagamento com cartão de crédito processado. Valor: " + amount);
    }
}

// ConcreteProduct - BoletoPaymentProcessor (Subclasse de PaymentProcessor)
class BoletoPaymentProcessor extends PaymentProcessor {
    public void processPayment(double amount) {
        // Implementação específica para processar pagamento com boleto bancário
        // Pode gerar código de barras, enviar o boleto para o cliente, etc.
        System.out.println("Pagamento com boleto bancário processado. Valor: " + amount);
    }
}
```

3. **Criando a classe abstrata PaymentProcessorFactory:**

```java
// AbstractFactory - PaymentProcessorFactory
abstract class PaymentProcessorFactory {
    public abstract PaymentProcessor createPaymentProcessor();
}
```

4. **Criando as subclasses concretas de PaymentProcessorFactory para cada meio de pagamento:**

```java
// ConcreteFactory - CreditCardPaymentProcessorFactory
class CreditCardPaymentProcessorFactory extends PaymentProcessorFactory {
    public PaymentProcessor createPaymentProcessor() {
        return new CreditCardPaymentProcessor();
    }
}

// ConcreteFactory - BoletoPaymentProcessorFactory
class BoletoPaymentProcessorFactory extends PaymentProcessorFactory {
    public PaymentProcessor createPaymentProcessor() {
        return new BoletoPaymentProcessor();
    }
}
```

5. **Exemplo de uso do padrão Abstract Factory:**

```java
public class Client {
    public static void main(String[] args) {
        PaymentProcessorFactory factory;

        // Suponha que, com base em alguma lógica, decidimos usar o processador de pagamento com cartão de crédito
        factory = new CreditCardPaymentProcessorFactory();

        // Criar o processador de pagamento com base na factory escolhida
        PaymentProcessor processor = factory.createPaymentProcessor();

        // Processar o pagamento com o processador adequado
        double amount = 100.0;
        processor.processPayment(amount);
    }
}
```

Neste exemplo, usamos o padrão Abstract Factory para criar diferentes processadores de pagamento  sem a necessidade de usar um switch. O código cliente (classe `Client`) interage apenas com as classes abstratas `PaymentProcessor` e `PaymentProcessorFactory`, sem se preocupar com a implementação específica dos processadores de pagamento. Dessa forma, podemos adicionar novos processadores de pagamento criando novas subclasses concretas de `PaymentProcessorFactory` e `PaymentProcessor`, sem modificar o código existente.

## Use nomes descritivos
Não tenha medo de criar nomes extensos, pois eles são melhores do que um pequeno e enigmático.

Selecionar nomes descritivos esclarecerá o modelo do módulo.

## Parâmetros de funções
A quantidade ideal de parâmetros para uma função é zero(nulo). Depois vem um (mónade), seguindo de dois(diáse). Sempre que possível devem-se evitar três (tríade) ou mais parâmetros(políade).

## Formas Mônades Comuns
Há duas razões bastante comuns para se passar um único parâmetros a uma função. Pode estar questionando sobre aquele parâmetro como `boolean fileExists("MyFile"). Ou pode trabalhar com o parâmetro, transformando-o em outra coisa e retornando-o. Você deve escolher nomes que torne clara essa distinção. 

Há outra forma menos usual e que deve ser usado com cautela que é quando há um parâmetro de entrada, mas nenhum de saída. O programa em si serve para interpretar a chamada da função como um evento, e usar o parâmetro para alterar o estado do sistema. Nesse caso deve estar claro que se trata de um evento e os nomes e contextos devem ser claros.

## Parâmetros Lógicos
Esses parâmetros são feios. Passar um booleano para uma função é uma prática horrível. Pois ele complica imediatamente a assinatura do método, mostrando explicitamente que a função faz mais de uma coisa. Ela faz uma coisa se o valor for verdadeiro, e outra se for falso!

## Funções Díades
Díades não são ruins, e você certamente terá que usá-las, entretanto é mais difícil de entender do que um mônade. Quando díades são necessárias seus parâmetros são componentes de um único valor como `Point p = new Point(0,0)` de um eixo cartesiano.

## Evite efeitos colaterais
Efeitos colaterais são mentiras. Sua função promete fazer apenas uma coisa, mas ela também faz outras coisas escondida. Considere, por exemplo, a função aparentemente inofensiva na Listagem abaixo. Ela usa um algoritmo padrão para comparar um userName a um password e retorna true ou false. Porém há um efeito colateral:

```java
public class UserValidator {
    private Cryptographer cryptographer;

    public boolean checkPassword(String userName, String password){
        User user = UserGateway.findByName(userName);
        if (user != User.NULL) {
            String codedPhrase = user.getPhraseEncodeByPassword();
            String phrase = cryptographer.decrypt(codedPhrase, password);
            if ("Valid Password".equals(phrase)) {
                Session.initialize();
                return true;
            }
        }
        return false
    }
}
```
O efeito colateral é a chamada ao Session.initialize(). A função deveria apenas verificar a senha e não inicializar a sessão. Esse efeito cria um acoplamento temporário. Isto é, `checkPassword` só poderá ser chamado quando for seguro inicializar a sessão. Se for chamado fora de ordem ou
sem querer os dados da sessão poderão ser perdidos. Se for preciso esses acoplamentos, é preciso  deixar claro no nome da função, nesse caso: `checkPasswordAndInitializeession` embora isso certamente violaria "fazer apenas uma coisa".

## Separação comando-consulta
As funções devem fazer ou responder algo, mas não ambos. Sua função ou altera o estado de um objeto ou retorna informações sobre ele.

## Prefira exceções a retorno de código de erro
Fazer funções retornarem códigos de erros é uma leve violação da separação comando-consulta, pois os comandos são usados como expressões de comparação em estruturas if. O problema gerado é a criação de estruturas aninhadas.

```java 
if (deletePage(page) == E_OK) {
    if (registry.deleteReference(page.name) == E_OK) {
        if(configKeys.deletekey(page.name.makeKey()) == E_OK){
            logger.log("página excluída)
        } else {
            logger.log("configKey não foi excluída")
        }
    } else {
        logger.log("deleteReference não foi excluído do registro");
} else {
        logger.log("a exclusão falhou")
}
```
Por outro lado, se você usar exceções em vez de retornar códigos de erros, então o código de tratamento de erro poderá ficar separado do código e ser simplificado:

```java
try {
    deletePage(page);
    registry.deleteReference(page.name);
    configKeys.deleteKey(page.name.makeKey());
}
catch (Exception e) {
    logger.log(e.getMessage());
}
```
## Extraia os blocos try/catch
Esses blocos não tem direito de serem feios. Eles confundem a estrutura do código e misturam o tratamento de erro com o processamento normal do código. Portanto, é melhor colocar essa estrutura em suas próprias funções.
```java
public void delete(Page page) {
    try {
        deletePageAndAllReferences(page);
    }
    catch (Exception e) {
        logError(e)
    }
}

private void deletePageAndAllReferences(Page page) throws Exception {
    deletePage(page);
    registry.deleteReference(page.name);
    configKey.deleteKey(page.name.makeKey());
}

private void logError(Exception e) {
    logger.log(e.getMessage())
}
```
A função delete acima só faz tratamento de erro, a função deletePageAndAllReferences só trata de processos que excluem toda uma página.

## Evite repetição
A duplicação é um problema, pois ela amontoa o código e serão necessárias várias modificações se o algoritmo mudar. Além de serem oportunidades para a omissão de um erro.

## Conclusão
"Se seguir as regras aqui descritas, suas funções serão curtas, bem nomeadas e bem organizadas. Mas jamais se esqueça de que seu objetivo verdadeiro é contar a história do sistema, e que as funções que você escrever precisam estar em perfeita sincronia e formar uma linguagem clara e precisa para lhe ajudar na narração."
