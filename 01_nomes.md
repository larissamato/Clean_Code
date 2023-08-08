# Nomes Significativos
O mais difícil sobre escolher bons nomes é a necessidade de se possuir boas habilidades
de descrição e um histórico cultural compartilhado. Nomeamos nossas variáveis, funções,
parâmetros, classes e pacotes. Como é uma atividade contínua é importante que façamos isso
muito bem. Esse doc é um resumo do capítulo 2 de cleanCode em que aborda a significância dos
nomes.

## Use nomes que revele seu propósito
O nome de uma variável, função ou classe deve responder a todas as grandes questões.
Ele deve lhe dizer porque existe, o que faz e como é usado. Se um nome requer um comentário,
então ele não revela seu propósito.

**Observe como a escolha dos nomes facilita o entendimento do propósito desse código:**

```java
#Nomes sem propósito
public List<int[]> getThem() {
    List<int[]> list1 = new ArrayList<>();
    for (int[] x : theList) {
        if (x[0t] == 4) {
            list1.add(x);
        }
    }
    return list1;
}
```
```java
#Renomeação
public List<int[]> getFlaggedCells() {
    List<int[]> flaggedCells = new ArrayList<>();
    for (int[] cell : gameBoard) {
        if (cell[STATUS_VALUE] == FLAGGED) {
            flaggedCells.add(x);
        }
    }
    return flaggedCells;
}
```
```java
# Adicionada uma função ao invés de usar um vetor
public List<int[]> getFlaggedCells() {
    List<int[]> flaggedCells = new ArrayList<>();
    for (int[] cell : gameBoard) {
        if (cell.isFlagged()) {
            flaggedCells.add(x);
        }
    }
    return flaggedCells;
}
```
## Evite informações erradas
Devemos evitar palavras cujos significados podem se desviar daquilo que desejamos. 
Por exemplo: usar accountList sendo que o que armazena as contas não é do tipo Lista.
Portanto, accountGroup ou buchOfAccounts ou apenas accounts seria melhor. Evite também usar
nomes muito parecidos.

## Faça distinções significativas
Se os nomes precisam ser diferentes, então também devem ter significados distintos.
Usar números sequenciais em nomes (a1, a2 ... an) não oferece informação alguma. 
Palavras muito comuns são outra forma de distinção que nada expressam, ou então são redundantes.
O nome de uma variável jamais deve conter a palavra 'variável', o nome de uma Tabela jamais
deve conter a palavra Tabela.
## Use nomes pronunciáveis
Crie nomes pronunciáveis, pois programação é uma atividade social. Se você não consegue 
pronuncia-los não terá como discutir sobre algum aspecto do código sem parecer um idiota.
Então troque:

```java
class DtaRcrd102 {
    private Date genymdhms;
    private Date modymdhms;
    private final String pszqint = "102";
};
```
Por: 
```java 
class Customer {
    private Date generationTimestamp;
    private Date modificationTimestamp;
    private final String recordId = "102";
};
```
## Use nomes passíveis de busca
Nomes de uma letra possuem um problema em particular por não ser fácil de localizá-los.
Pode-se usar facilmente um grep para `MAX_CLASSES_PER_STUDENT`, porém buscar um `7` poderia 
ser mais complicado. Devido a isso nomes longos se sobressaem aos curtos. Então compare: 

```java 
for (int j=0; j<34; j++){
    s += (t[j]*4)/5
}
```
com:
```java
int realDaysPerIdealDay = 4;
const int WORK_DAYS_PER_WEEK = 5;
int sum = 0;

for (int j=0; j<NUMBER_OF_TASKS; j++){
    int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
    int realTaskWeeks = (realTaskDays / WORK_DAYS_PER_WEEK);
    sum += realTaskWeeks;
}
```
## Nomes de classes
Classes e objetos devem ter nomes com substantivo, como Client, PageWiki e Account. 
Evite palavras como Manager, Data ou Info no nome de uma classe, que também não deve ser um verbo.

## Nomes de métodos
Os nomes de métodos devem ser verbos, como deleteNotification, fetchDevices ou handleButtonLogin.

## Selecione uma palavra por conceito
Escolha uma palavra para cada conceito abstrato e fique com ela. Por exemplo, é confuso 
ter `catch` `get` `retrieve` como métodos equivalentes de classes diferentes. Um léxico
consistente é uma grande vantagem aos programadores que precisem usar seu código.

## Não faça trocadilhos
Evite usar a mesma palavra para dois propósitos, isso pode confudir quem está lendo 
seu código e atrapalha na consistência dele.

## Use nomes a partir do domínio da solução
Pode usar termos da informática, nomes de algoritmos, nomes de padrões, termos matemáticos etc.
O que não pode ocorrer é pensar no nome a partir do domínio do problema, pois quem lerá 
o seu código serão programadores e não o cliente. Selecionar nomes técnicos, é geralmente,
o método mais adequado.

## Use nomes de domínios do problema
Quando não houver uma solução com um termo da programação, pode usar o nome do domínio
do problema. Nesse caso o programador que fizer a manutenção do código tem a opção de
perguntar a um especialista de tal domínio o que significa.
## Adicione um contexto significativo
É importante que os nomes tenha um contexto para o leitor. Para isso você coloca classes,
funções e namespaces bem nomeados. Se nada disso funcionar, então talvez como último recurso
seja necessário adicionar prefixos ao nome, como por exemplo addrName. É claro que a melhor solução
seria uma classe chamada Address. Abaixo está uma Listagem com variáveis sem contexto:

```java
# Variáveis com contexto obscuro

private void printGuessStatistics (char candidate, int count) {
    String number;
    String verb;
    String pluralModifier;
    if (count == 0) {
        number = "no";
        verb = "Existem";
        pluralModifier = "s";
    } else if (count == 1) {
        number = "1";
        verb = "Existe";
        pluralModifier = "";
    }else {
        number = Integer.toString(count);
        verb = "Existem";
        pluralModifier ="s";
    }
    String guessMessage = String.format("There %s %s %s %s", verb, number, candidate, pluralModifier);    print(guessMessage);  
}
```
```java 
# Variáveis com contexto

public class GuessStaticsMessage {
    private String number;
    private String Verb;
    private String pluralModifier;

    public String make (char candidate, int count) {
        createPluralDependentMessageParts(count);
        return String.format( "There %s %s %s %s", verb, number, candidate, pluralModifier);
    }

    private void createPluralDependentMessageParts (int count) {
        if (count == 0) {
            thereAreNoLetters();
        } else is (count == 1) {
            thereIsOneLetter();
        } else {
            thereAreManyLetters(count);
        }
    }
    private void thereAreManyLetters(int count) {
        number = Integer.toString(count);
        verb = "Existem";
        pluralModifier = "s";
    }
    private void thereAreOneLetter() {
        number = "1";
        verb = "Existe";
        pluralModifier = "";
    }
    private void thereAreNoLetters() {
        number = "no";
        verb = "Existem";
        pluralModifier = "s";
    }
}
```

## Não adicione contextos desnecessários
Não adicione prefixos desnecessários, como por exemplo: Um aplicativo chamado "Gas Station Deluxe" e o nome das classes sempre iniciam com GSD. Imagine como ficaria o autocomplete ou as pesquisas?
Nomes curtos geralmente são melhores contanto que sejam claros. Então, se não houver necessidade de mais um tipo de contexto, não adicione.
Outro exemplo seria, os nomes accountAddress e customerAddres estão bons para instâncias da classe Address, mas seriam ruins para nomes de classes. Address está bom para uma classe.
Se precisar diferenciar entre endereços MAC, postal e web, uma ideia seria PostalAddress, MAC e URI. 
