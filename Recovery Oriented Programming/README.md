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

Escrever um código perfeitamente correto é uma tarefa desafiadora e quase impossível. Paradigmas, ferramentas e ambientes de programação, incluindo programação estruturada, programação orientada a objetos, padrões de design e outros, foram criados para ajudar o programador a escrever um código gerenciável e correto. Ferramentas que garantem testes durante a fase de programação complementam o esforço acima [2, 5, 13]. Ainda assim, em muitos casos, as especificações do programa não são cumpridas [19] - uma situação que pode causar muitos danos. Em nosso trabalho anterior sobre recuperador autonômico estabilizador [4], sugerimos um framework formal para o paradigma orientado à recuperação [20]. A abordagem sugerida ajustou os pacotes de software existentes (caixa preta), que resultaram em uma sobrecarga alta, pois cada ação de E/S tinha que ser interceptada para detectar um estado defeituoso.

>Parcialmente apoiado pelo Centro de Ciências da Computação Lynn e William Frankel, pela concessão da Deutsche Telecom, pelo prêmio da faculdade IBM, pelo Ministério da Ciência de Israel e pela Cadeira Rita Altura Trust em Ciências da Computação.

>Olga Brukman e Shlomi Dolev

Paradigmas de tolerância a falhas e eventual software bizantino. A autoestabilização [9] é uma forte propriedade de tolerância a falhas para sistemas que garante a recuperação automática quando as falhas param de ocorrer. Um sistema autoestabilizante é capaz de iniciar a partir de qualquer configuração possível em que processadores, processos, links de comunicação, buffers de comunicação e quaisquer outros componentes relacionados ao processo estejam em um estado arbitrário (por exemplo, valores de variáveis ​​arbitrários, contador de programa arbitrário). O designer pode apenas assumir que os programas do sistema são executados. Com base nessa premissa, ele ou ela prova que o sistema converge para um estado legal, ou seja, para um estado em que o sistema satisfaz suas especificações. Se o sistema for iniciado a partir de um estado inicial legal, a execução garantirá que o sistema permaneça em um estado legal. Isso é chamado de "propriedade de fechamento". Caso o sistema seja iniciado em um estado ilegal (possivelmente após encontrar falhas transitórias), a execução do programa de autoestabilização garantirá que, eventualmente (em um número finito de etapas), um estado legal seja alcançado. Isso é chamado de "propriedade de convergência". Novamente, quando o programa atingir um estado legal, ele continuará em execução e permanecerá em um estado legal até que uma falha (transitória) ocorra novamente. Um algoritmo autoestabilizador nunca termina. O algoritmo não precisa necessariamente “identificar” a ocorrência da falha e se recuperar, mas continua sendo executado e traz o sistema para um estado legal. A complexidade de tempo de um algoritmo auto-estabilizador é o número de etapas necessárias para que um algoritmo iniciado em um estado arbitrário converja para um estado legal. Observe que quando todos os processadores executam programas incorretos (programas com bugs), eles podem exibir qualquer tipo de comportamento e, portanto, não há garantia de convergência.

