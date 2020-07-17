# Recovery Oriented Programming
                               (Extended Abstract)

                            Olga Brukman, Shlomi Dolev

Department of Computer Science, Ben-Gurion University of the Negev, Beer-Sheva,
                 84105, Israel brukman,dolev@cs.bgu.ac.il

## Resumo.

Escrever um código perfeitamente correto é uma tarefa desafiadora e quase impossível. Neste trabalho, sugerimos o paradigma de programação orientada à recuperação, a fim de lidar com eventuais programas bizantinos.
O compositor de especificação do programa aplica as especificações do programa (propriedades de segurança e liberdade) em tempo de execução usando predicados sobre variáveis ​​de entrada e saída. O programador de componentes usará essas variáveis ​​na implementação do programa. Sugerimos o uso da abordagem de "caixa de areia", na qual todas as instruções do programa que alteram uma variável de especificação são executadas primeiro com variáveis ​​temporárias e são para evitar a execução de uma instrução que viola as especificações. Além disso, o monitoramento externo é usado para lidar com falhas transitórias e para garantir a convergência para um estado legal.

A implementação dessas idéias inclui a definição de novas instruções na linguagem de programação com o objetivo de permitir a adição de predicados e ações de recuperação. Sugerimos um design para uma ferramenta que estende a linguagem de programação Java. Além disso, fornecemos um esquema de prova de correção para provar que o código combinado com os predicados e as ações de recuperação é autoestabilizável e, sob a premissa de reinicialização, eventualmente cumpre suas especificações.

Palavras-chave: auto-estabilização, computação autonômica.

## 1. Introdução

Escrever um código perfeitamente correto é uma tarefa desafiadora e quase impossível. Paradigmas, ferramentas e ambientes de programação, incluindo programação estruturada, programação orientada a objetos, padrões de design e outros, foram criados para ajudar o programador a escrever um código gerenciável e correto. Ferramentas que garantem testes durante a fase de programação complementam o esforço acima [2, 5, 13]. Ainda assim, em muitos casos, as especificações do programa não são cumpridas [19] - uma situação que pode causar muitos danos. Em nosso trabalho anterior sobre recuperador autonômico estabilizador [4], sugerimos um framework formal para o paradigma orientado à recuperação [20]. A abordagem sugerida ajustou os pacotes de software existentes (caixa preta), que resultaram em uma sobrecarga alta, pois cada ação de E / S tinha que ser interceptada para detectar um estado defeituoso.
