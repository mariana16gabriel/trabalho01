# TRABALHO 01:  Hydro Economizer
Trabalho desenvolvido durante a disciplina de Banco de Dados do Integrado

# Sumário

### 1. COMPONENTES<br>
Integrantes do grupo<br>
Mariana_Tassan: marianatassan19@gmail.com<br>
Gabriel_Borges: gmarinho14@gmail.com<br>

### 2.INTRODUÇÃO E MOTIVAÇAO<br>
Este documento contém a especificação do projeto do banco de dados Hydro Economizer 
<br>e motivação da escolha realizada. <br>

> Nos dias de hoje temos uma possibilidade cada vez mais colocada em consideração: O fim da água potável no mundo. Estamos cada vez desperdiçando mais e mais água potável, esta que não é um recurso infinito e é vital para o ser humano, isso pode causar consequências horríveis para a humanidade e até mesmo condenar sua existência. Por isso, pensando em economizar o máximo de água o possível, criamos o Hydro-Economizer.
 

### 3.MINI-MUNDO Novo<br>

>O Hydro-Economizer armazenará em cada cadastro o nome do proprietário de uma residência, seu CPF, o endereço de tal residência e o número de moradores da residência. Ele também armazena o consumo de água de cada residência, mostrando a data, o número de cômodos e o consumo de água atualizado diariamente (tal dado é obtido através de sensores implantados na tubulação. O sistema armazena as informações dos sensores utilizados, mostrando seu número identificador (ID), cômodo em que o sensor se localiza e o tipo de sensor (sensor de tubulação de água, sensores de portas, etc). Por fim, o sistema mostra o consumo mensal, mostrando a data, o consumo total da casa, o consumo por residente médio, o consumo por cômodo médio, e mostra o nível do consumo (“Econômico”, “Bom”, “Normal”, “Excessivo”, “Muito Exagerado”).

### 4.RASCUNHOS BÁSICOS DA INTERFACE (MOCKUPS)<br>
Neste ponto a codificação não e necessária, somente as ideias de telas devem ser desenvolvidas. O princípio aqui é pensar na criação da interface para identificar possíveis informações a serem armazenadas e/ou descartadas <br>

Sugestão: https://balsamiq.com/products/mockups/<br>

![Alt text](https://github.com/discipbd1/trab01/blob/master/balsamiq.png?raw=true "Title")
![Arquivo PDF do Protótipo Balsamiq feito para Empresa Devcom](https://github.com/discipint/trabalho01/blob/master/arquivos/EmpresaDevcom.pdf?raw=true "Empresa Devcom")

#### 4.1 QUAIS PERGUNTAS PODEM SER RESPONDIDAS COM O SISTEMA PROPOSTO?

O projeto Hydro Economizer precisa inicialmente dos seguintes relatórios:

1º) Relatório contendo as informações da casa cadastrada, ou seja, o endereço e o número de cômodos;

2°) Relatório contendo as informações do usuário, ou seja, nome e cpf;

3°) Relatório contendo informações relacionadas aos sensores, ou seja, o tipo de sensor, cômodo que está localizado e um código de identificação;

4°) Relatório contendo informações relacionadas ao consumo, como a quantidade de litros captada pelo sensor em um determinado período de tempo;


#### 4.2 TABELA DE DADOS DO SISTEMA:
[Exemplo de Tabela de dados do Hydro Economizer]
https://github.com/mariana16gabriel/trabalho01/blob/master/tabela_de_dados.xlsx

### 5.MODELO CONCEITUAL<br>
   ![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/conceitual_print.png?raw="true" "little")
#### 5.1 Validação do Modelo Conceitual
    Grupo01: Caio Lessa e Lucas Tejada
    Grupo02: Beatriz Auer e Júlia Suzano
    
#### 5.2 DESCRIÇÃO DOS DADOS 

    USUARIO: tabela que armazena as informações cadastrais relativas ao usuário:
    
    cpf: campo que armazena o número de Cadastro de Pessoa Física para cada usuário;
    nome: campo que armazena o nome do usuário;
    codigo: campo de identificação;
    
    RESIDENCIA: tabela que armazena as informações relativas à residência:
    
    endereço: campo que armazena o endereço de cada moradia cadastrada;
    numero_comodos: campo que armazena a quantidade de cômodos da residência;
    codigo: campo de identificação;
    
    SENSOR: tabela que armazena as informações reativas aos sensores:
    
    tipo_sensor: campo contendo o tipo de sensor utilizado;
    local_da_casa: campo contendo o cômodo onde o sensor está localizado;
    codigo: campo de identificação de cada sensor;
    
    DADO: tabela que armazena as informações relativas ao consumo geradas pelos sensores: 
    
    data_hora: campo que armazena a data e/ou hora de captura do sensor;
    valor: campo que armazena a quantidade de água captada pelo sensor;
    codigo: campo de identificação.
    

### 6	MODELO LÓGICO<br>
   ![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/logico.png?raw="true" "little")
    

### 7	MODELO FÍSICO<br>

         CREATE TABLE USUARIO (
             codigo integer PRIMARY KEY,
             nome varchar(50),
             cpf integer
         );

         CREATE TABLE RESIDENCIA (
             codigo integer PRIMARY KEY,
             endereco varchar(100),
             numero_comodos integer
         );

         CREATE TABLE SENSOR (
             codigo integer PRIMARY KEY,
             tipo_sensor varchar(30),
             local_da_casa varchar(20),
             FK_RESIDENCIA_codigo integer
         );

         CREATE TABLE DADO (
             valor float,
             data_hora timestamp,
             codigo integer PRIMARY KEY,
             FK_SENSOR_codigo integer
         );

         CREATE TABLE Usuario_residencia (
             fk_USUARIO_codigo integer,
             fk_RESIDENCIA_codigo integer
         );

         ALTER TABLE SENSOR ADD CONSTRAINT FK_SENSOR_2
             FOREIGN KEY (FK_RESIDENCIA_codigo)
             REFERENCES RESIDENCIA (codigo)
             ON DELETE RESTRICT;

         ALTER TABLE DADO ADD CONSTRAINT FK_DADO_2
             FOREIGN KEY (FK_SENSOR_codigo)
             REFERENCES SENSOR (codigo)
             ON DELETE RESTRICT;

         ALTER TABLE Possui ADD CONSTRAINT FK_Possui_1
             FOREIGN KEY (fk_USUARIO_codigo)
             REFERENCES USUARIO (codigo)
             ON DELETE RESTRICT;

         ALTER TABLE Possui ADD CONSTRAINT FK_Possui_2
             FOREIGN KEY (fk_RESIDENCIA_codigo)
             REFERENCES RESIDENCIA (codigo)
             ON DELETE RESTRICT;         

### 8	INSERT APLICADO NAS TABELAS DO BANCO DE DADOS<br>
#### 8.1 DETALHAMENTO DAS INFORMAÇÕES
        INSERT INTO RESIDENCIA (codigo, endereco, numero_comodos) VALUES 
        (1264932, 'R. das Goiabas, 296 - qd-1 lt-14, Morada de Laranjeiras - Serra, ES', 10),
        (2896907, 'Av Paulo Pereira Gomes, 803 - LJ 1, Morada de Laranjeiras - Serra, ES', 8), 
        (9876544, 'R Joaquim Lírio, 753, Praia Canto - Vitória, ES', 7),
        (1234567, 'R Cândido Portinari, 27 - lj-1, Santa Luíza - Vitória, ES', 11),
        (1234561, 'R Fortunato Ramos, 30 - s-309, Santa Lúcia - Vitória, ES', 10),
        (5467801, 'R Misael Pedreira da Silva, 48, Santa Lúcia - Vitória, ES', 6),
        (1523799, 'R Henrique Laranja, 318, Centro - Vila Velha, ES', 15),
        (9999999, 'Av Champagnat, 304 - s-101, Centro - Vila Velha, ES', 20),
        (5554441, 'R José Ricardo, 210, Ibes - Vila Velha, ES', 12),
        (1115489, 'R Carolina Leal, 107, Olaria - Vila Velha, ES', 8);

        INSERT INTO SENSOR (codigo, tipo_sensor, local_da_casa, FK_RESIDENCIA_codigo) VALUES
        (1462779, 'tubulação', 'lavabo', 1264932),
        (9993451, 'tubulação', 'banheiro', 2896907),
        (1114333, 'tubulação', 'quintal', 9876544),
        (8830881, 'tubulação', 'cozinha', 1234567),
        (1244565, 'tubulação', 'cozinha', 1234561),
        (1166598, 'tubulação', 'lavabo', 5467801),
        (2456702, 'tubulação', 'quintal', 1523799),
        (4684225, 'tubulação', 'banheiro', 9999999),
        (2253688, 'tubulação', 'banheiro', 5554441),
        (1156855, 'tubulação', 'cozinha', 1115489);

        INSERT INTO DADO (codigo, data_hora, valor, FK_SENSOR_codigo) VALUES
        (1234567, '2019-04-02 00:00:00', 10, 1462779),
        (7654321, '2019-04-01 13:20:00', 1, 9993451),
        (9876545, '2018-12-19 15:20:00', 13, 1114333),
        (3456789, '2017-06-01 01:00:00', 0.5, 8830881),
        (5678901, '2019-09-01 07:45:00', 0.3, 1244565),
        (8901209, '2019-04-14 08:13:00', 12, 1166598),
        (4587129, '2016-05-30 12:32:00', 1, 2456702),
        (8877554, '2015-10-27 16:07:00', 2, 4684225),
        (4474799, '2019-11-17 06:00:00', 4, 2253688),
        (1235467, '2018-09-14 20:10:00', 6.8, 1156855);

        INSERT INTO USUARIO (codigo, nome, cpf) VALUES
        (2345222, 'Mariana Tassan', 12345678910),	
        (8709431, 'Gabriel Marinho', 23456789101),
        (1254673, 'Augusto Silva', 34567891011),
        (9908466, 'Juliana Nogueira', 45678910111),	
        (1252705, 'Felipe Souza', 56789101112),
        (4527869, 'Emanuel Andrade', 67891011121),	
        (1574806, 'Suzana Pereira', 78910111213),	
        (3368907, 'Marcos Ferraz', 89101112131),
        (6774322, 'Priscila Pinto', 91011121314),	
        (2566788, 'João Ferreira', 10111213141);	

        INSERT INTO Usuario_residencia (fk_USUARIO_codigo, fk_RESIDENCIA_codigo) VALUES
        (2345222, 1264932),
        (8709431, 2896907),
        (1254673, 9876544),
        (9908466, 1234567),
        (1252705, 1234561),
        (4527869, 5467801),
        (1574806, 1523799),
        (3368907, 9999999),
        (6774322, 5554441),
        (2566788, 1115489);

#### 8.2 INCLUSÃO DO SCRIPT PARA CRIAÇÃO DE TABELA E INSERÇÃO DOS DADOS
        a) Junção dos scripts anteriores em um único script 
        (create para tabelas e estruturas de dados + dados a serem inseridos)
        b) Criar um novo banco de dados para testar a restauracao 
        (em caso de falha na restauração o grupo não pontuará neste quesito)
        c) formato .SQL
#### 8.3 INCLUSÃO DO SCRIPT PARA EXCLUSÃO DE TABELAS EXISTENTES, CRIAÇÃO DE TABELA NOVAS E INSERÇÃO DOS DADOS
        a) Junção dos scripts anteriores em um único script 
        (Drop table + Create de tabelas e estruturas de dados + dados a serem inseridos)
        b) Criar um novo banco de dados para testar a restauracao 
        (em caso de falha na restauração o grupo não pontuará neste quesito)
        c) formato .SQL

## Marco de Entrega 08 em: (29/05/2019)<br>

### 9	TABELAS E PRINCIPAIS CONSULTAS<br>
    OBS: Incluir para cada tópico as instruções SQL + imagens (print da tela) mostrando os resultados.<br>
#### 9.1	CONSULTAS DAS TABELAS COM TODOS OS DADOS INSERIDOS (Todas) <br>
#### 9.2	CONSULTAS DAS TABELAS COM FILTROS WHERE (Mínimo 4)<br>
#### 9.3	CONSULTAS QUE USAM OPERADORES LÓGICOS, ARITMÉTICOS E TABELAS OU CAMPOS RENOMEADOS (Mínimo 11)
    a) Criar 5 consultas que envolvam os operadores lógicos AND, OR e Not
    b) Criar no mínimo 3 consultas com operadores aritméticos 
    c) Criar no mínimo 3 consultas com operação de renomear nomes de campos ou tabelas
#### 9.4	CONSULTAS QUE USAM OPERADORES LIKE E DATAS (Mínimo 12) <br>
    a) Criar outras 5 consultas que envolvam like ou ilike
    b) Criar uma consulta para cada tipo de função data apresentada.


    
#### 9.5	ATUALIZAÇÃO E EXCLUSÃO DE DADOS (Mínimo 6)<br>


#### 9.6	CONSULTAS COM JUNÇÃO E ORDENAÇÃO (Mínimo 6)<br>
        a) Uma junção que envolva todas as tabelas possuindo no mínimo 3 registros no resultado
        b) Outras junções que o grupo considere como sendo as de principal importância para o trabalho
        

### ATUALIZAÇÃO DA DOCUMENTAÇÃO DOS SLIDES PARA APRESENTAÇAO SEMESTRAL (Mínimo 6 e Máximo 10)<br>


#### 9.7	CONSULTAS COM GROUP BY E FUNÇÕES DE AGRUPAMENTO (Mínimo 6)<br>

#### 9.8	CONSULTAS COM LEFT E RIGHT JOIN (Mínimo 4)<br>
#### 9.9	CONSULTAS COM SELF JOIN E VIEW (Mínimo 6)<br>
        a) Uma junção que envolva Self Join
        b) Outras junções com views que o grupo considere como sendo de relevante importância para o trabalho
#### 9.10	SUBCONSULTAS (Mínimo 3)<br>

#### 9.11	LISTA DE CODIGOS DAS FUNÇÕES E TRIGGERS<br>
        Detalhamento sobre funcionalidade de cada código.
        a) Objetivo
        b) Código do objeto (função/trigger)
        c) exemplo de dados para aplicação
        d) resultados em forma de tabela/imagem
<br>


#### 9.12	GERACAO DE DADOS (MÍNIMO DE 100 MIL REGISTROS PARA PRINCIPAL RELAÇAO)<br>
        a) principal tabela do sistema deve ter no mínimo 100 mil registros
        b) tabelas diretamente relacionadas a tabela principal 10 mil registros
        c) tabelas auxiliares de relacao multivalorada mínimo de 10 registros
        d) registrar o tempo de inserção em cada uma das tabelas do banco de dados
        e) especificar a quantidade de registros inseridos em cada tabela
        Para melhor compreensão verifiquem o exemplo na base de testes:<br>
        https://github.com/discipbd2/base-de-testes-locadora
        

#### 9.13	Backup do Banco de Dados<br>
        Detalhamento do backup.
        a) Tempo
        b) Tamanho
        c) Teste de restauração (backup)
        d) Tempo para restauração
        e) Teste de restauração (script sql)
        f) Tempo para restauração (script sql)
<br>

Data de Entrega: (Data a ser definida)
<br>

#### 9.14	APLICAÇAO DE ÍNDICES E TESTES DE PERFORMANCE<br>
    a) Lista de índices, tipos de índices com explicação de porque foram implementados nas consultas 
    b) Performance esperada VS Resultados obtidos
    c) Tabela de resultados comparando velocidades antes e depois da aplicação dos índices (constando velocidade esperada com planejamento, sem indice e com índice Vs velocidade de execucao real com índice e sem índice).
    d) Escolher as consultas mais complexas para serem analisadas (consultas com menos de 2 joins não serão aceitas)
    e) As imagens do Explain devem ser inclusas no trabalho, bem como explicações sobre os resultados obtidos.
    f) Inclusão de tabela mostrando as 10 execuções, excluindo-se o maior e menor tempos para cada consulta e 
    obtendo-se a media dos outros valores como resultado médio final.
<br>
    Data de Entrega: (Data a ser definida)
<br>   

### 10	ATUALIZAÇÃO DA DOCUMENTAÇÃO DOS SLIDES PARA APRESENTAÇAO FINAL (Mínimo 6 e Máximo 10)<br>
<br>
    Data de Entrega: (Data a ser definida)
<br>

### 11 Backup completo do banco de dados postgres 
    a) deve ser realizado no formato "backup" 
        (Em Dump Options #1 Habilitar opções Don't Save Owner e Privilege)
    b) antes de postar o arquivo no git o mesmo deve ser testado/restaurado por outro grupo de alunos/dupla
    c) informar aqui o grupo de alunos/dupla que realizou o teste.

    
### 12	TUTORIAL COMPLETO DE PASSOS PARA RESTAURACAO DO BANCO E EXECUCAO DE PROCEDIMENTOS ENVOLVIDOS NO TRABALHO PARA OBTENÇÃO DOS RESULTADOS<br>
        a) Outros grupos deverão ser capazes de restaurar o banco 
        b) executar todas as consultas presentes no trabalho
        c) executar códigos que tenham sido construídos para o trabalho 
        d) realizar qualquer procedimento executado pelo grupo que desenvolveu o trabalho

### 13	DIFICULDADES ENCONTRADAS PELO GRUPO<br>  

    
Data de Entrega final: (Data a ser definida)
<br>

       
### 14  FORMATACAO NO GIT: https://help.github.com/articles/basic-writing-and-formatting-syntax/
<comentario no git>
    
##### About Formatting
    https://help.github.com/articles/about-writing-and-formatting-on-github/
    
##### Basic Formatting in Git
    
    https://help.github.com/articles/basic-writing-and-formatting-syntax/#referencing-issues-and-pull-requests
   
    
##### Working with advanced formatting
    https://help.github.com/articles/working-with-advanced-formatting/

#### Mastering Markdown
    https://guides.github.com/features/mastering-markdown/

### OBSERVAÇÕES IMPORTANTES

#### Todos os arquivos que fazem parte do projeto (Imagens, pdfs, arquivos fonte, etc..), devem estar presentes no GIT. Os arquivos do projeto vigente não devem ser armazenados em quaisquer outras plataformas.
1. Caso existam arquivos com conteúdos sigilosos, comunicar o professor que definirá em conjunto com o grupo a melhor forma de armazenamento do arquivo.

#### Todos os grupos deverão fazer Fork deste repositório e dar permissões administrativas ao usuário deste GIT, para acompanhamento do trabalho.

#### Os usuários criados no GIT devem possuir o nome de identificação do aluno (não serão aceitos nomes como Eu123, meuprojeto, pro456, etc). Em caso de dúvida comunicar o professor.


Link para BrModelo:<br>
http://sis4.com/brModelo/brModelo/download.html
<br>


Link para curso de GIT<br>
![https://www.youtube.com/curso_git](https://www.youtube.com/playlist?list=PLo7sFyCeiGUdIyEmHdfbuD2eR4XPDqnN2?raw=true "Title")


        
        


    





