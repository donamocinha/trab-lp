# Perl

**"A silver tape da Internet"**

### Legibilidade

### Redigibilidade

### Confiabilidade

### Eficiência

### Facilidade de Aprendizado

### Ortogonalidade 

### Reusabilidade

### Modificabilidade

### Portabilidade





## Tipos de dados

### Escalar
Um escalar é declarado precedindo um identificador com um `$`. Ele pode ser dividido em 3 diferentes tipos:

#### Número
Um número pode ser declarado e interpretado das seguintes formas:
- Inteiro (positivo ou negativo): `1`, `23`, `-456`
- Ponto flutuante: `1.`, `2.f`, `5.31f`, `4.73`
- Notação científica: `1E2`, `1.E4`, `4.23E7`
- Inteiro hexadecimal: `0x1D107`, `0xfaceb00c`
- Inteiro octal: `0577`, `034122`

#### String
Strings podem ser declaradas entre aspas simples (`'`) ou duplas (`"`). Strings encapsuladas em aspas duplas permitem interpolação de variáveis, onde qualquer identificador de variável no seu interior é substituído pelo valor da variável **no momento da atribuição**.

Por outro lado, strings encapsuladas em aspas simples são interpretadas literalmente, e identificadores de variável em seu interior são mantidos em verbatim.

```perl
#!/usr/bin/perl

$str1 = "This is a test of $a.\n";
$a = 10;
$str2 = "This is a test of $a.\n";
$str3 = 'This is a test of $a.\n';
print $str1;
print $str2;
print $str3;
```

O resultado da execução do script acima é o seguinte:
```
This is a test of.
This is a test of 10.
This is a test of $a.

```

#### Referência (ponteiro para outra variável)
Uma referência é nada mais que um ponteiro implícito para outra variável.

### Array
Um array é uma lista ordenada de escalares acessados com um índice numérico. A declaração dele é precedida por um `@`.

Um array pode ser inicializado com uma lista de elementos:

```perl
@array = (4, 8, 15, 16, 23, 42);
```

Também pode ser usado o operador `qw//` - ele parseia e retorna uma lista de strings, que são colocadas entre as barras `/ /` e delimitadas por caracteres whitespace.

```perl
@texts = qw/This is a test./;
@names = qw/
Smith
Jones
Blake
Clark/;
```

Para acessar um array, precedemos o identificador com um `$`, com o índice numérico do elemento entre colchetes. Note que um índice negativo indexa o array a partir do final.

```perl
@names = qw/
Smith
Jones
Blake
Clark/;

print "$names[0], $names[1], $names[-1], $names[-2]\n"
```

```
Smith, Jones, Clark, Blake

```

### Hash (array associativo, hashmap, dicionário)
Um hash é uma lista não-ordenada de chaves e valores (escalares), acessados usando as chaves. A declaração dele é precedida por um `%`.

## Sintaxe básica de Perl

### Variáveis
Variáveis podem ser declaradas e acessadas conforme abaixo:

```perl
#!/usr/bin/perl

$var = 20;
@nums = (4, 8, 15, 16, 23, 42);
%hashmap = {"Bruno Lopes" => "Legal", "Perl" => 10, "Vocês" => "Amores", "Konata" => "Te amo!"};
print "$var\n";
print "$nums[5]\n";
print "$hashmap{'Perl'}\n";
```

A saída do programa acima é:
```
20
42
10

```

### Strings multi-linha (documentos "here")
O Perl oferece uma forma bastante confortável de definir strings multi-linha, tais como mensagens a serem impressas na tela.

Para definir uma string multi-linha, é usado o operador unário `<<` junto a um identificador terminador - ele pode ser uma string ou apenas um identificador simples. Não podem haver espaços entre o operador, o identificador e o ponto-e-vírgula terminando a linha.

As linhas seguintes contém o texto da string multi-linha, e o compilador vai adicionando o texto das linhas à string até encontrar o terminador especificado. Identificadores de variáveis podem ser utilizados no texto e o conteúdo será substituído na string em tempo de execução.

```perl
#!/usr/bin/perl

$help = <<'end';
foo - a utility for illustrating an example - Copyleft Victor Carneiro 2018
This program is released under public domain.

Usage: $0

This program ignores any command line arguments.
end
print $help;
```

```
foo - a utility for illustrating an example - Copyleft Victor Carneiro 2018
This program is released under public domain.

Usage: ./foo

This program ignores any command line arguments.

```

### V-Strings
Há outra forma de se declarar strings, especialmente aquelas contendo caracteres não-imprimíveis - as v-strings.

Uma v-string começa com o caractere v, e uma lista de códigos decimais separados por pontos.

```perl
#!/usr/bin/perl

$foo = v102.111.111;
$poop = v128169;
$red = v27.91.59.51.49.109;

print "$foo $poop $red Uau!\n"
```

A saída deste programa é a seguinte:
```
foo 💩  Uau!

```
*Nota do editor: O caractere no meio é o emoji de cocô, e o texto "Uau!" é pra estar vermelho na hora de formatar*

### Chamadas de funções
As chamadas são feitas especificando o identificador da função e a lista de argumentos. Parênteses são opcionais!

```perl
#!/usr/bin/perl

print "Hello world\n";
print("Hello world\n")";
```

### Comentários
Comentários de uma só linha são especificados utilizando o caractere `#'.

Comentários de múltiplas linhas são especificados com uma linha que começa com `=`, e terminam com uma linha começando com `=cut`.

```perl
#!/usr/bin/perl
=
This is an example of a multi-line comment.
All content here will be ignored by the compiler.
The comment will end on the line below.
=cut

# This is a single-line comment.
print "Hello world\n";
```

## Características semânticas

### Contexto variável
As variáveis são tratadas de forma diferente de acordo com a situação em que estão sendo usadas.

Há 5 diferentes contextos:

#### Contexto escalar
Contexto em que estamos atribuindo um valor a uma variável escalar. O lado direito da expressão é tratado em contexto escalar.

```perl
$str = "Oin :3";
$scal = 20;
@array = (1, 4, 9);

$scalar0 = $str;
$scalar1 = $scal;
$scalar2 = $array;

print "$scalar0\n$scalar1\n$scalar2\n$scalar3\n"
```

O resultado do programa acima é:
```
Oin :3
20
3

```

#### Contexto de lista
Contexto em que estamos atribuindo um valor a uma variável do tipo array ou hash. O lado direito da expressão é tratado em contexto de lista.

#### Contexto booleano
Contexto em que estamos avaliando uma expressão com o objetivo de saber se ela é verdadeira ou falsa.

#### Contexto interpolativo
Contexto em que estamos interpolando uma string - ou seja, obtendo valores de uma variável para substituir em parte de uma string.

#### Contexto void
Contexto em que não interessa o valor de retorno.

## Operações

### Operações com escalares

#### Aritmética

#### Strings
`.`: Concatena dois escalares e retorna uma string
```perl
#!/usr/bin/perl

$str = "hello " . "world";
$strnum = "hi" . 1;
$numstr = 4 . "chan.org";
$numnum = 65 . 0 . 2;

print "$str\n$strnum\n$numstr\n$numnum\n";
```

```
hello world
hi1
4chan.org
6502

```

#### Arrays
Funções `push`, `pop`, `shift`, `unshift`: Adicionam e removem elementos no final ou início de um array, respectivamente

Operador range (`..`): Gera uma sequência de elementos dentro de um range. Os literais especificando o range devem ser do mesmo tipo, e podem ser numerais, caracteres ou strings.

```perl
#!/usr/bin/perl

@a = 0..4;
@b = 130..139;
@c = "aa".."cc";
@d = a..c;
print "@a\n@b\n@c\n@d\n";
```

```
0 1 2 3 4
130 131 132 133 134 135 136 137 138 139
aa ab ac ba bb bc ca cb cc
a b c

```

Slices: Extrair elementos de um array:

```perl
#!/usr/bin/perl

@a = (23, 999, 1, 42);
@b = @a[0, 2];
@c = @a[0..2];
print "@a\n@b\n@c\n";
```

```
23 999 1 42
23 1
23 999 1

```

#### Hashes

Acessando elementos de um hash:

```perl
#!/usr/bin/perl

%h = {}
```
