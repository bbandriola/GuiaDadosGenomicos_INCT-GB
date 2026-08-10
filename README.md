# Guia de processamento de dados genômicos
Este tutorial tem como objetivo desenvolver habilidades básicas na compreensão e no processamento de dados genômicos. 

Os tópicos abordados são os seguintes: 

1. [Introdução a linguagem Linux](./Linux)
2. [Montagem de genomas de novo](./GenomasDeReferência)
3. [Controle de qualidade, Mapeamento e Chamada de Variantes](./QC_Mapeamento_ChamadadeVariantes)
4. Análises de estrutura genômica
5. [Filogenômica](./Filogenômica)
6. [Genômica populacional](./GenômicaPopulacional)
7. Transcriptomas

## Rápida explicação 
Todo o material (ou quase todo) exige conhecimentos básicos de Linux e Bash. Se este for o seu primeiro contato com a análise de dados provenientes de sequenciamento genômico (sejam eles de sequências curtas ou longas), um bom ponto de partida é a seção de [Linux](./Linux). Nessa seção, você compreenderá os conceitos básicos de navegação em um terminal e comandos úteis para executar as análises necessárias. Pode ser desafiador começar, mas garantimos que vale a pena.

Além disso, recomenda-se fortemente conhecimento básico de outras linguagens de programação, como R e Python. Sempre que possível, os tutoriais incluirão links para materiais complementares. Vá além do conteúdo deste repositório :dragon: 

Os demais tópicos são relacionados à análise de genomas. A relevância de cada tópico dependerá dos objetivos do seu projeto. Este repositório não tem como intuito ser seguido de forma linear. Sinta-se à vontade para explorar os materiais de acordo com as suas necessidades. 

## Programas 

Os programas utilizados em análises genômicas possuem diversas versões. Algumas funções estão disponíveis em todas as versões e outras não. Se atente a isso quando iniciar suas análises. Alguns comandos podem não funcionar por esse motivo. Nesse ponto entramos em uma questão muito importante, o gerenciamento do uso de programas ao longo das análises. 

Cada servidor/super computador utilizará o gerenciador de pacote mais adequado ou estabelecido previamento pelo grupo de administradores da máquina. Portanto, converse com o administrador do servidor que você utilizará para saber como você deverá realizar o download de programas para conseguir executá-los da forma correta. Em alguns tutoriais, você verá a menção ao gerenciador de pacote [Conda](https://anaconda.org/). O Conda é um ótimo gerenciador de programas, de fácil implementação e uso. 

Ao longo dos tutoriais, você verá que há uma gama de diferentes programas que realizam as mesmas tarefas (alguns mais rápidos, outros com outras exigências...). Para facilitar a sua procura por um programa que seja adequado às suas necessidades, você pode consultar a tabela [Programas de análise de dados genômicos](https://app.notion.com/p/3626457f0f6d806a85b5ca14bc6f123e?v=3626457f0f6d8012aec0000cfedda990&source=copy_link). Fique à vontade para explorar como achar melhor e fazer sugestões, caso ache pertinente.

## Dicas 
* Os tutoriais apresentam sugestões de linhas de comando, mas links para a documentação oficial de cada programa estarão sempre disponíveis. É encorajado consultar os manuais das ferramentas de interesse e explorar os demais parâmetros disponíveis, muitos deles podem ser úteis no seu caso.
* Todos os comandos são acompanhados de explicações das suas funções :triangular_flag_on_post: Busque sempre compreender o que você está fazendo. Isso facilita na identifiação e resolução de problemas ao executar um comando (o que é bem comum :woman_shrugging:).
* Não se frustre sozinh@. Ficou com dúvida sobre alguma análise e gostaria de discutir com alguém? A aba de discussões está disponível para isso. Coloque sua dúvida lá, a comunidade pode acompanhar a discussão e contribuir.   

Este tutorial está em constante desenvolvimento. E, como tudo na ciência, aqui a construção também é coletiva. Sinta-se à vontade para sugerir modificações, adições ou revisões de qualquer detalhe (**incluindo links quebrados, erros de digitação ou informações desatualizadas** :grimacing:). 

Caso deseje contribuir com alguma modificação, basta editar o arquivo e encaminhar a modificação ao repositório principal. 

:dna::computer: **Boas análises** :computer::dna:

## Reconhecimento 
Este tutorial foi desenvolvido com base em diversos materiais disponíveis online. Todos eles estarão devidamente referenciados ao longo ou no final de cada documento.
