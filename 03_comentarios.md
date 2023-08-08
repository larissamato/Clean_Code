# Comentários
Nada pode ser tão útil quanto um comentário bem colocado. Nada consegue amontoar
um módulo mais do que comentários dogmáticos e supérfluos. Nada npode ser tão prejudicial
quanto um velho comentário mal feito que dissemina mentiras e informações incorretas.
    O uso adequado de comentários é compenssar nosso fracasso em nos expressar no código.
Coemntários são sempre fracassos. Devemos usá-los porque nem sempre encontramos uma forma de nos expressar sem eles, mas seu uso não é motivo para comemoração.
## Comentários Compensam um código ruim
Uma das motivações mais comuns para criar comentários é um código ruim. Ao invés de gastar
seu tempo criando comentários para explicar a bagunça que você fez, use-o para limpar
essa zona.

## Explique-se no código
O que você preferiria ver? Isso:
```java
// Verifica se o funcionário tem direito a todos os benefícios
if ((employee.flags & HOURLY_FLAG) && (employee.age> 65))
```

Ou isso?
```java
if (employee.isEligibleForFullBenefits())
```
Só é preciso alguns segundos de pensamento para explicar a maioria de sua intenção no
código. Em muitos casos, é simplismente uma questão de criar uma função cujo nome diga
a mesma coisa que você deseja colocar no comentário.

# Comentários bons
Às vezes, nossos padrões de programação corporativa nos forçam a escrever certos
comentários por questão legais. Por exemplo, direitos autorais.
```java
//Direitos autorais (C) 2003, 2004, 2005 por Object Mentor, Inc. Todos os direitos
reservados.
// Distribuido sob os termos das versão 2 ou posterior da Licenca Publica Geral da GNU.
```
Comentários como esse não devem ser contratos ou tomos legais. Onde for possível, faça
referência a uma licença padrão ou outro documento externo em vez de colocar todos os
termos e condições no mesmo comentário.

## Explicando a intenção
Às vezes, um comentário fornece a intenção por trás de uma decisão como por exemplo:
```java
//Essa e a nossa melhor tentativa para conseguir uma condição corrida.
//Para isso criamos um grande numero de threads.
```
por mais que a solução não seja a melhor, pelo menos você sabe a intenção do programador.

## Destaque
Pode-se usar um comentário para destacar a importância de algo que talvez pareça
irrelevante.

# Comentários ruins
A maioria dos comentários cai nessa categoria. Geralmente eles são suportes ou desculpas
para um código de baixa qualidade ou justificativas para a falta de decisões. Um comentário
mal feito deixa o código ainda mais confuso, ou então cria uma redundância ou um
ruído desnecessário. Poucas práticas são tão condenáveis quanto explicar o código
nos comentários.
