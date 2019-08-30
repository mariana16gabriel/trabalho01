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

> O desperdício de água potável é um grande problema no mundo, já que esta é um recurso finito e vital para os seres humanos. Pensando em uma forma de evitar esse deperdício no meio doméstico, criamos o sistema de monitoramento de água em residências, o Hydro Economizer, diferentemente dos concorrentes presentes no mercado, o Hydro Economizer não controla o uso de água, realizando cortes indesejados, pois acreditamos que a conscientização deve vir de cada usuário.
 

### 3.MINI-MUNDO Novo<br>

>O sistema de monitoramento de consumo de água Hydro-Economizer conta com a instalação de sensores na tubulação de qualquer residência, de onde é obtido informações relativas ao consumo, como a quantidade de água captada em determinado período de tempo. Tais informações ficam armazenadas no banco de dados do sistema, sendo possível consultá-las a fim de gerar relatórios que auxiliem o usuário no controle do uso da água. 

### 4.RASCUNHOS BÁSICOS DA INTERFACE (MOCKUPS)<br>


#### 4.1 QUAIS PERGUNTAS PODEM SER RESPONDIDAS COM O SISTEMA PROPOSTO?

O projeto Hydro Economizer precisa inicialmente dos seguintes relatórios:

1º) Relatório contendo a hora em que o consumo é mais elevado por residência no intervalo de 1 semana;

2º) Relatório contendo a quantidade de sensores existentes num bairro;

3º) Relatório contendo o número de usuários num município;

4º) Relatório contendo o número de sensores por usuário;

5º) Relatório contendo a quantidade de água consumida numa rua/avenida num período de 1 semana.



#### 4.2 TABELA DE DADOS DO SISTEMA:
[Exemplo de Tabela de dados do Hydro Economizer]
https://github.com/mariana16gabriel/trabalho01/blob/master/tabela_de_dados.xlsx

### 5.MODELO CONCEITUAL<br>
   ![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/conceitual_finalll.png?raw="true" "little")
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
   ![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/logico_finalll.png?raw="true" "little")
    

### 7	MODELO FÍSICO<br>

/* FÍSICO: */

CREATE TABLE USUARIO (
    codigo varchar(8) PRIMARY KEY,
    nome varchar(50),
    cpf varchar(20)
);

CREATE TABLE SENSOR (
    codigo varchar(8) PRIMARY KEY,
    FK_RESIDENCIA_codigo varchar(8)
);

CREATE TABLE RESIDENCIA (
    codigo varchar(8) PRIMARY KEY,
    tipo_logradouro varchar(20),
    nome_logradouro varchar(50),
    numero int,
    complemento varchar(50),
    FK_ESTADO_codigo varchar(8),
    FK_MUNICIPIO_codigo varchar(8),
    FK_BAIRRO_codigo varchar(8)
);

CREATE TABLE ESTADO (
    nome varchar(20),
    codigo varchar(8) PRIMARY KEY
);

CREATE TABLE BAIRRO (
    nome varchar(20),
    codigo varchar(8) PRIMARY KEY,
    FK_MUNICIPIO_codigo varchar(8)
);

CREATE TABLE MUNICIPIO (
    codigo varchar(8) PRIMARY KEY,
    nome varchar(20),
    FK_ESTADO_codigo varchar(8)
);

CREATE TABLE DADO (
    codigo varchar(8) PRIMARY KEY,
    data_hora timestamp,
    valor float,
    FK_SENSOR_codigo varchar(8)
);

CREATE TABLE COMODO (
    codigo varchar(8) PRIMARY KEY,
    nome_complemento varchar(20),
    FK_RESIDENCIA_codigo varchar(8)
);

CREATE TABLE Usuario_residencia (
    fk_USUARIO_codigo varchar(8),
    fk_RESIDENCIA_codigo varchar(8)
);
 
ALTER TABLE SENSOR ADD CONSTRAINT FK_SENSOR_2
    FOREIGN KEY (FK_RESIDENCIA_codigo)
    REFERENCES RESIDENCIA (codigo)
    ON DELETE RESTRICT;
 
ALTER TABLE RESIDENCIA ADD CONSTRAINT FK_RESIDENCIA_2
    FOREIGN KEY (FK_ESTADO_codigo)
    REFERENCES ESTADO (codigo)
    ON DELETE RESTRICT;
 
ALTER TABLE RESIDENCIA ADD CONSTRAINT FK_RESIDENCIA_3
    FOREIGN KEY (FK_MUNICIPIO_codigo)
    REFERENCES MUNICIPIO (codigo)
    ON DELETE RESTRICT;
 
ALTER TABLE RESIDENCIA ADD CONSTRAINT FK_RESIDENCIA_4
    FOREIGN KEY (FK_BAIRRO_codigo)
    REFERENCES BAIRRO (codigo)
    ON DELETE RESTRICT;
 
ALTER TABLE BAIRRO ADD CONSTRAINT FK_BAIRRO_2
    FOREIGN KEY (FK_MUNICIPIO_codigo)
    REFERENCES MUNICIPIO (codigo)
    ON DELETE RESTRICT;
 
ALTER TABLE MUNICIPIO ADD CONSTRAINT FK_MUNICIPIO_2
    FOREIGN KEY (FK_ESTADO_codigo)
    REFERENCES ESTADO (codigo)
    ON DELETE RESTRICT;
 
ALTER TABLE DADO ADD CONSTRAINT FK_DADO_2
    FOREIGN KEY (FK_SENSOR_codigo)
    REFERENCES SENSOR (codigo)
    ON DELETE RESTRICT;
 
ALTER TABLE COMODO ADD CONSTRAINT FK_COMODO_2
    FOREIGN KEY (FK_RESIDENCIA_codigo)
    REFERENCES RESIDENCIA (codigo)
    ON DELETE RESTRICT;
 
ALTER TABLE Usuario_residencia ADD CONSTRAINT FK_Usuario_residencia_1
    FOREIGN KEY (fk_USUARIO_codigo)
    REFERENCES USUARIO (codigo)
    ON DELETE RESTRICT;
 
ALTER TABLE Usuario_residencia ADD CONSTRAINT FK_Usuario_residencia_2
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
        (1115489, 'R Carolina Leal, 107, Olaria - Vila Velha, ES', 8),
        (5738383, 'Av Getúlio Vargas, 36, Vila Rubim - Vitória, ES', 11),
        (0165942, 'Av Getúlio Vargas, 34, Vila Rubim - Vitória, ES', 9),
        (0000001, 'R da Prainha, 29, Grande Vitória - Vitória, ES', 12),
        (2222228, 'R 8 de Julho, 110, Grande Vitória - Vitória, ES', 14),
        (7767798, 'R 11 de Janeiro, 46, Grande Vitória - Vitória, ES', 4);

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
        (1156855, 'tubulação', 'cozinha', 1115489),
        (1111145, 'tubulação', 'lavabo', 5738383),
        (5637669, 'tubulação', 'banheiro', 0165942),
        (2364767, 'tubulação', 'cozinha', 0000001),
        (1237345, 'tubulação', 'quintal', 2222228),
        (4002892, 'tubulação', 'banheiro', 7767798);

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
        (1235467, '2018-09-14 20:10:00', 6.8, 1156855),
        (9999946, '2019-09-14 20:10:00', 0.6, 1111145),
        (4574337, '2019-09-14 14:10:00', 0.1, 5637669),
        (8089555, '2018-06-01 01:10:00', 80, 2364767),
        (9999992, '2018-06-01 01:10:00', 70, 1237345),
        (5456562, '2010-08-04 00:14:00', 45, 4002892);

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
        (2566788, 'João Ferreira', 10111213141),	
        (4444121, 'Mariana Pinto', 11739825458),
        (5553331, 'Mariana Ferraz', 98754482288),
        (9990003, 'Mariana Gabriel', 33387497825),
        (2572557, 'Mariana Júlia', 75395128642),
        (5462222, 'Gabriel Rego', 14725896495);

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
        (2566788, 1115489),
        (4444121, 5738383),
        (5553331, 0165942),
        (9990003, 0000001),
        (2572557, 2222228),
        (5462222, 7767798),
        (2345222, 0000001),
        (9908466, 1234567),
        (1252705, 0000001),
        (2566788, 1264932),
        (8709431, 5467801),
        (4527869, 0000001),
        (3368907, 1234561),
        (6774322, 0165942),
        (5553331, 2222228),
        (1574806, 2896907),
        (9990003, 9999999),
        (4444121, 1115489),
        (2345222, 2896907),
        (2572557, 5467801),
        (9990003, 5738383);
        
#### 8.2 INCLUSÃO DO SCRIPT PARA CRIAÇÃO DE TABELA E INSERÇÃO DOS DADOS
        
        CREATE TABLE USUARIO (
             codigo integer PRIMARY KEY,
             nome varchar(50),
             cpf bigint
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

         ALTER TABLE Usuario_residencia ADD CONSTRAINT FK_Possui_1
             FOREIGN KEY (fk_USUARIO_codigo)
             REFERENCES USUARIO (codigo)
             ON DELETE RESTRICT;

         ALTER TABLE Usuario_residencia ADD CONSTRAINT FK_Possui_2
             FOREIGN KEY (fk_RESIDENCIA_codigo)
             REFERENCES RESIDENCIA (codigo)
             ON DELETE RESTRICT; 
             
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
        
        
#### 8.3 INCLUSÃO DO SCRIPT PARA EXCLUSÃO DE TABELAS EXISTENTES, CRIAÇÃO DE TABELA NOVAS E INSERÇÃO DOS DADOS
         
         DROP TABLE USUARIO;
         
         DROP TABLE RESIDENCIA;
         
         DROP TABLE SENSOR;
         
         DROP TABLE DADO;
         
         DROP TABLE Usuario_residencia;
         
         CREATE TABLE USUARIO (
             codigo integer PRIMARY KEY,
             nome varchar(50),
             cpf bigint
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

         ALTER TABLE Usuario_residencia ADD CONSTRAINT FK_Possui_1
             FOREIGN KEY (fk_USUARIO_codigo)
             REFERENCES USUARIO (codigo)
             ON DELETE RESTRICT;

         ALTER TABLE Usuario_residencia ADD CONSTRAINT FK_Possui_2
             FOREIGN KEY (fk_RESIDENCIA_codigo)
             REFERENCES RESIDENCIA (codigo)
             ON DELETE RESTRICT; 
             
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

## Marco de Entrega 08 em: (29/05/2019)<br>

### 9	TABELAS E PRINCIPAIS CONSULTAS<br>
    OBS: Incluir para cada tópico as instruções SQL + imagens (print da tela) mostrando os resultados.<br>
#### 9.1	CONSULTAS DAS TABELAS COM TODOS OS DADOS INSERIDOS (Todas) <br>
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/residencia.png?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/dado.png?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/sensor.png?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/usuario.png?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/usuario_residencia.png?raw="true" "little")


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


        
        


    





