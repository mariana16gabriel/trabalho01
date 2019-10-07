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

#### 4.1 QUAIS PERGUNTAS PODEM SER RESPONDIDAS COM O SISTEMA PROPOSTO?

O projeto Hydro Economizer precisa inicialmente dos seguintes relatórios:

1º) Em que horas na semana o consumo é mais elevado em cada residência?

2º) Qual o consumo médio de um bairro?

3º) Qual a diferença entre o maior e o menor consumo por Estado?

4º) Em quais municípios se encontram mais usuários?

5º) O quanto de água foi consumido numa rua ou avenida em uma semana? 



#### 4.2 TABELA DE DADOS DO SISTEMA:
[Exemplo de Tabela de dados do Hydro Economizer]
https://github.com/mariana16gabriel/trabalho01/blob/master/tabela_de_dados.xlsx

### 5.MODELO CONCEITUAL<br>
   ![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/conceitual_esse.png?raw="true" "little")
#### 5.1 Validação do Modelo Conceitual
    Grupo01: Caio Lessa e Lucas Tejada
    Grupo02: Beatriz Auer e Júlia Suzano
    
    
#### 5.2 DESCRIÇÃO DOS DADOS 

    USUARIO: tabela que armazena as informações cadastrais relativas ao usuário;
    cpf: campo que armazena o número de Cadastro de Pessoa Física para cada usuário;
    nome: campo que armazena o nome do usuário;
    codigo: campo de identificação.
    
    RESIDENCIA: tabela que armazena as informações relativas à residência;
    codigo: campo de identificação;
    tipo_logradouro: campo que define se o endereço é de uma rua ou avenida;
    nome_logradouro: campo que identifica o nome da rua ou avenida;
    numero: campo que armazena o número da residência;
    complemento: se a residência for uma casa, será preenchido com informações adicionais do endereço. Se for um apartamento, ficará vazio.
    
    ESTADO: tabela que armazena o Estado em que a residência cadastrada se encontra;
    codigo: campo de identificação;
    nome: campo que armazena o nome do Estado.
    
    MUNICIPIO: tabela que armazena o município em que a residência cadastrada se encontra;
    codigo: campo de identificação;
    nome: campo que armazena o nome do município.
    
    BAIRRO: tabela que armazena o bairro em que a residência cadastrada se encontra;
    codigo: campo de identificação;
    nome: campo que armazena o nome do bairro.
    
    SENSOR: tabela que armazena as informações relativas aos sensores:
    latitude: campo que armazena a latitude do sensor;
    longitude: campo que armazena a longitude do sensor;
    codigo: campo de identificação de cada sensor;
    
    DADO: tabela que armazena as informações relativas ao consumo obtidas pelos sensores: 
    data_hora: campo que armazena a data e a hora de captura do sensor;
    valor: campo que armazena a quantidade de água captada pelo sensor;
    codigo: campo de identificação.
    
    COMODO: tabela que armazena os cômodos de cada residência;
    codigo: campo de indetificação de cada cômodo;
    nome_complemento: campo que armazena o nome do cômodo.
    

### 6	MODELO LÓGICO<br>
   ![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/logico_esse.png?raw="true" "little")
    

### 7	MODELO FÍSICO<br>

    CREATE TABLE USUARIO (
        codigo varchar(8) PRIMARY KEY,
        nome varchar(50),
        cpf varchar(20)
    );

    CREATE TABLE SENSOR (
        codigo varchar(8) PRIMARY KEY,
        latitude float,
        longitude float,
        FK_RESIDENCIA_codigo varchar(8),
        FK_COMODO_codigo varchar(8)
    );

    CREATE TABLE RESIDENCIA (
        codigo varchar(8) PRIMARY KEY,
        tipo_logradouro varchar(20),
        nome_logradouro varchar(50),
        numero int,
        complemento varchar(50),
        FK_BAIRRO_codigo varchar(8),
        FK_ESTADO_codigo varchar(8),
        FK_MUNICIPIO_codigo varchar(8)
    );

    CREATE TABLE ESTADO (
        nome varchar(20),
        codigo varchar(8) PRIMARY KEY
    );

    CREATE TABLE BAIRRO (
        nome varchar(20),
        codigo varchar(8) PRIMARY KEY
    );

    CREATE TABLE MUNICIPIO (
        codigo varchar(8) PRIMARY KEY,
        nome varchar(20)
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

    ALTER TABLE SENSOR ADD CONSTRAINT FK_SENSOR_3
        FOREIGN KEY (FK_COMODO_codigo)
        REFERENCES COMODO (codigo)
        ON DELETE RESTRICT;

    ALTER TABLE RESIDENCIA ADD CONSTRAINT FK_RESIDENCIA_2
        FOREIGN KEY (FK_BAIRRO_codigo)
        REFERENCES BAIRRO (codigo)
        ON DELETE RESTRICT;

    ALTER TABLE RESIDENCIA ADD CONSTRAINT FK_RESIDENCIA_3
        FOREIGN KEY (FK_ESTADO_codigo)
        REFERENCES ESTADO (codigo)
        ON DELETE RESTRICT;

    ALTER TABLE RESIDENCIA ADD CONSTRAINT FK_RESIDENCIA_4
        FOREIGN KEY (FK_MUNICIPIO_codigo)
        REFERENCES MUNICIPIO (codigo)
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
        INSERT INTO USUARIO (codigo, nome, cpf) VALUES
    ('2345222', 'Mariana Tassan', 12345678910),	
    ('8709431', 'Gabriel Marinho', 23456789101),
    ('1254673', 'Augusto Silva', 34567891011),
    ('9908466', 'Juliana Nogueira', 45678910111),	
    ('1252705', 'Felipe Souza', 56789101112),
    ('4527869', 'Emanuel Andrade', 67891011121),	
    ('1574806', 'Suzana Pereira', 78910111213),	
    ('3368907', 'Marcos Ferraz', 89101112131),
    ('6774322', 'Priscila Pinto', 91011121314),	
    ('2566788', 'João Ferreira', 10111213141),	
    ('4444121', 'Mariana Pinto', 11739825458),
    ('5553331', 'Mariana Ferraz', 98754482288),
    ('9990003', 'Mariana Gabriel', 33387497825),
    ('2572557', 'Mariana Júlia', 75395128642),
    ('5462222', 'Gabriel Rego', 14725896495);
    
    INSERT INTO ESTADO (CODIGO, NOME) VALUES
    ('11333222', 'Rio de Janeiro'),
    ('00008887', 'São Paulo'),
    ('77777444', 'Espírito Santo'),
    ('22222655', 'Bahia'),
    ('78907889', 'Santa Catarina'),
    ('77776655', 'Minas Gerais'),
    ('12596643', 'Acre'),
    ('89766786', 'Amazonas'),
    ('87456655', 'Roraima'),
    ('09876452', 'Pernambuco'),
    ('11663335', 'Ceará'),
    ('76578853', 'Rondônia'),
    ('36666676', 'Mato Grosso'),
    ('88888555', 'Goiás'),
    ('96676765', 'Paraná');
    
    INSERT INTO MUNICIPIO (CODIGO, NOME) VALUES
    ('00088788', 'Rio de Janeiro'),
    ('12346665', 'São Paulo'),
    ('45534878', 'Vitória'),
    ('45454545', 'Serra'),
    ('08638656', 'Vila Velha'),
    ('33333355', 'Florianópolis'),
    ('75646756', 'Búzios'),
    ('88948478', 'Recife'),
    ('21121211', 'Paraty'),
    ('98788566', 'Salvador'),
    ('66688866', 'Cariacica'),
    ('23567356', 'Viana'),
    ('28356856', 'Campinas'),
    ('90656777', 'Natal'),
    ('12489000', 'Porto Alegre');
    
    INSERT INTO BAIRRO (CODIGO, NOME) VALUES
    ('23412890', 'Morada laranjeiras'),
    ('78945499', 'Laranjeiras'),
    ('78907894', 'Colina Laranjeiras'),
    ('92378673', 'Valparaíso'),
    ('27495723', 'Feu Rosa'),
    ('17234665', 'Nova Carapina'),
    ('04589278', 'Praia da Costa'),
    ('00112334', 'Itaparica'),
    ('47384783', 'Itapuã'),
    ('39292229', 'Novo México'),
    ('24523656', 'Jardim Colorado'),
    ('09280549', 'Jardim Camburi'),
    ('11211209', 'Jardim da Penha'),
    ('02394822', 'Praia do Canto'),
    ('27847584', 'Mata da Praia');
    
    INSERT INTO COMODO (codigo, nome_complemento,FK_RESIDENCIA_codigo) VALUES
     ('19284661','banheiro','12341324'),
     ('24357988','cozinha','51324312'),
     ('44433430','lavabo','17658214'),
     ('12395731','lavanderia','85676523'),
     ('11111006','quintal','63451342'),
     ('44445733','garagem','53453462'),
     ('12000912','banheiro','12454326'),
     ('65000765','cozinha','76745454'),
     ('98644432','lavabo','32456734'),
     ('55553332','lavanderia','73452345'),
     ('12367788','quintal','12415121'),
     ('88886664','garagem','12213421'),
     ('11111338','banheiro','53432243'),
     ('12456333','cozinha','51231424'),
     ('88889901','lavabo','15324123');


    INSERT INTO SENSOR (codigo,latitude,longitude,FK_RESIDENCIA_codigo,FK_COMODO_codigo) VALUES
    ('32325656',40.601203,-9.665677,'12341324','19284661'),
    ('76763526',40.601201,-15.668173,'51324312','24357988'),
    ('64444566',21.601203,-12.668173,'17658214','44433430'),
    ('09875665',45.601203,-11.668173,'85676523','12395731'),
    ('00056730',46.861203,-8.467173,'63451342','11111006'),
    ('11100421',33.601203,-10.668173,'53453462','44445733'),
    ('66644200',20.701203,-9.668173,'12454326','12000912'),
    ('00000004',21.601203,-1.668173,'76745454','65000765'),
    ('44446678',87.601203,-2.668173,'32456734','98644432'),
    ('11235543',40.601281,-3.668173,'73452345','55553332'),
    ('09876567',28.601203,-4.668173,'12415121','12367788'),
    ('44565686',29.601203,-5.668173,'12213421','88886664'),
    ('47865322',30.605203,-6.668173,'53432243','11111338'),
    ('10000444',12.601203,-7.668173,'51231424','12456333'),
    ('10033854',50.501203,-8.668173,'15324123','88889901');

    INSERT INTO DADO (codigo, data_hora, valor,FK_SENSOR_codigo) VALUES
    ('22233227','2019-04-02 00:00:00','10','32325656'),
    ('23478999','2019-04-01 13:20:00','1','76763526'),
    ('54674478','2018-12-19 15:20:00','1','64444566'),
    ('13453367','2017-06-01 01:00:00','2','09875665'),
    ('97465554','2019-09-01 07:45:00','2','00056730'),
    ('22333434','2019-04-14 08:13:00','4','11100421'),
    ('89886676','2016-05-30 12:32:00','6','66644200'),
    ('34456777','2015-10-27 16:07:00','25','00000004'),
    ('33466685','2019-11-17 06:00:0','4','44446678'),
    ('34456765','2018-09-14 20:10:00','4','11235543'),
    ('53435455','2019-09-14 20:10:00','2','09876567'),
    ('78967832','2019-09-14 14:10:00','1','44565686'),
    ('96786572','2018-06-01 01:10:00','1','47865322'),
    ('34576444','2018-06-01 01:10:00','9','10000444'),
    ('87655554','2010-08-04 00:14:00','9','10033854');
    
      INSERT INTO RESIDENCIA (CODIGO,TIPO_LOGRADOURO,NOME_LOGRADOURO,NUMERO,COMPLEMENTO,FK_BAIRRO_CODIGO,FK_ESTADO_CODIGO,FK_MUNICIPIO_CODIGO)
     VALUES
     ('12341324', 'R', 'Minas Gerais', 1290, 'Apartamento 1001', '23412890', '77777444', '45454545'),
     ('51324312', 'R', 'Marataízes', 301, 'Apartamento 504', '23412890', '77777444','45454545'),
     ('17658214', 'Av', 'Paulo Pereira Gomes', 500, Null, '23412890', '77777444', '45454545'),
     ('85676523', 'Av', 'Civit', 240, 'Apartamento 301', '23412890', '77777444', '45454545'),
     ('63451342', 'Av', 'Leandro Hassun', 120, Null, '27495723', '77777444', '45454545'),
     ('53453462', 'R', 'Nando Moura', 730, 'Apartamento 1502', '00112334', '77777444', '08638656'),
     ('12454326', 'R', 'Humberto Serrano', 280, 'Apartamento 903', '04589278', '77777444', '08638656'),
     ('76745454', 'Av', 'Champangnat', 1020, Null, '04589278', '77777444', '08638656'),
     ('32456734', 'R', 'Rômulo Mendonça', 410, 'Apartamento 201', '02394822', '77777444', '45534878'),
     ('73452345', 'R', 'Augusto Carrara', 820, 'Apartamento 1604', '09280549', '77777444', '45534878');
     ('12415121', 'R', 'Castelo Branco', 1540, 'Apartamento 1304', '24523656', '77777444', '08638656'),
     ('12213421', 'R', 'Marataízes', 301, 'Apartamento 504', '24523656', '77777444','08638656'),
     ('53432243', 'Av', 'Júlio Prestes', 3200, Null, '24523656', '77777444', '08638656'),
     ('51231424', 'R', 'Deodoro da Fonseca', 240, 'Apartamento 230', '27847584', '77777444', '45534878'),
     ('15324123', 'Av', 'Eurico Gaspar Dutra', 560, Null, '27847584', '77777444', '45534878');
     
     INSERT INTO USUARIO_RESIDENCIA(FK_USUARIO_CODIGO, FK_RESIDENCIA_CODIGO) VALUES
     ('2345222', '12341324'),
     ('2345222', '63451342'),
     ('2345222','17658214' ),
     ('8709431','63451342' ),
     ('8709431', '12341324'),
     ('8709431', '51231424'),
     ('1254673', '51231424'),
     ('1254673', '51324312'),
     ('1254673', '15324123'),
     ('9908466', '15324123'),
     ('9908466', '51324312'),
     ('9908466', '53432243'),
     ('1252705','17658214' ),
     ('1252705', '53432243'),
     ('1252705', '76745454'),
     ('4527869', '12213421'),
     ('4527869', '76745454'),
     ('4527869','85676523' ),
     ('1574806', '32456734'),
     ('1574806', '12415121'),
     ('1574806','85676523' ),
     ('3368907', '12213421'),
     ('3368907','12415121' ),
     ('3368907','12454326' ),
     ('6774322', '32456734'),
     ('6774322', '53453462'),
     ('6774322', '73452345'),
     ('2566788', '73452345'),
     ('2566788', '53453462'),
     ('2566788', '12454326');

#### 8.2 INCLUSÃO DO SCRIPT PARA CRIAÇÃO DE TABELA E INSERÇÃO DOS DADOS
        
        CREATE TABLE USUARIO (
    codigo varchar(8) PRIMARY KEY,
    nome varchar(50),
    cpf varchar(20)
    );

    CREATE TABLE SENSOR (
        codigo varchar(8) PRIMARY KEY,
        latitude float,
        longitude float,
        FK_RESIDENCIA_codigo varchar(8),
        FK_COMODO_codigo varchar(8)
    );

    CREATE TABLE RESIDENCIA (
        codigo varchar(8) PRIMARY KEY,
        tipo_logradouro varchar(20),
        nome_logradouro varchar(50),
        numero int,
        complemento varchar(50),
        FK_BAIRRO_codigo varchar(8),
        FK_ESTADO_codigo varchar(8),
        FK_MUNICIPIO_codigo varchar(8)
    );

    CREATE TABLE ESTADO (
        nome varchar(20),
        codigo varchar(8) PRIMARY KEY
    );

    CREATE TABLE BAIRRO (
        nome varchar(20),
        codigo varchar(8) PRIMARY KEY
    );

    CREATE TABLE MUNICIPIO (
        codigo varchar(8) PRIMARY KEY,
        nome varchar(20)
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

    ALTER TABLE SENSOR ADD CONSTRAINT FK_SENSOR_3
        FOREIGN KEY (FK_COMODO_codigo)
        REFERENCES COMODO (codigo)
        ON DELETE RESTRICT;

    ALTER TABLE RESIDENCIA ADD CONSTRAINT FK_RESIDENCIA_2
        FOREIGN KEY (FK_BAIRRO_codigo)
        REFERENCES BAIRRO (codigo)
        ON DELETE RESTRICT;

    ALTER TABLE RESIDENCIA ADD CONSTRAINT FK_RESIDENCIA_3
        FOREIGN KEY (FK_ESTADO_codigo)
        REFERENCES ESTADO (codigo)
        ON DELETE RESTRICT;

    ALTER TABLE RESIDENCIA ADD CONSTRAINT FK_RESIDENCIA_4
        FOREIGN KEY (FK_MUNICIPIO_codigo)
        REFERENCES MUNICIPIO (codigo)
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

    INSERT INTO USUARIO (codigo, nome, cpf) VALUES
     ('2345222', 'Mariana Tassan', 12345678910),	
     ('8709431', 'Gabriel Marinho', 23456789101),
     ('1254673', 'Augusto Silva', 34567891011),
     ('9908466', 'Juliana Nogueira', 45678910111),	
     ('1252705', 'Felipe Souza', 56789101112),
     ('4527869', 'Emanuel Andrade', 67891011121),	
     ('1574806', 'Suzana Pereira', 78910111213),	
     ('3368907', 'Marcos Ferraz', 89101112131),
     ('6774322', 'Priscila Pinto', 91011121314),	
     ('2566788', 'João Ferreira', 10111213141),	
     ('4444121', 'Mariana Pinto', 11739825458),
     ('5553331', 'Mariana Ferraz', 98754482288),
     ('9990003', 'Mariana Gabriel', 33387497825),
     ('2572557', 'Mariana Júlia', 75395128642),
     ('5462222', 'Gabriel Rego', 14725896495);
    
    INSERT INTO ESTADO (CODIGO, NOME) VALUES
    ('11333222', 'Rio de Janeiro'),
    ('00008887', 'São Paulo'),
    ('77777444', 'Espírito Santo'),
    ('22222655', 'Bahia'),
    ('78907889', 'Santa Catarina'),
    ('77776655', 'Minas Gerais'),
    ('12596643', 'Acre'),
    ('89766786', 'Amazonas'),
    ('87456655', 'Roraima'),
    ('09876452', 'Pernambuco'),
    ('11663335', 'Ceará'),
    ('76578853', 'Rondônia'),
    ('36666676', 'Mato Grosso'),
    ('88888555', 'Goiás'),
    ('96676765', 'Paraná');
    
    INSERT INTO MUNICIPIO (CODIGO, NOME) VALUES
    ('00088788', 'Rio de Janeiro'),
    ('12346665', 'São Paulo'),
    ('45534878', 'Vitória'),
    ('45454545', 'Serra'),
    ('08638656', 'Vila Velha'),
    ('33333355', 'Florianópolis'),
    ('75646756', 'Búzios'),
    ('88948478', 'Recife'),
    ('21121211', 'Paraty'),
    ('98788566', 'Salvador'),
    ('66688866', 'Cariacica'),
    ('23567356', 'Viana'),
    ('28356856', 'Campinas'),
    ('90656777', 'Natal'),
    ('12489000', 'Porto Alegre');
    
    INSERT INTO BAIRRO (CODIGO, NOME) VALUES
    ('23412890', 'Morada laranjeiras'),
    ('78945499', 'Laranjeiras'),
    ('78907894', 'Colina Laranjeiras'),
    ('92378673', 'Valparaíso'),
    ('27495723', 'Feu Rosa'),
    ('17234665', 'Nova Carapina'),
    ('04589278', 'Praia da Costa'),
    ('00112334', 'Itaparica'),
    ('47384783', 'Itapuã'),
    ('39292229', 'Novo México'),
    ('24523656', 'Jardim Colorado'),
    ('09280549', 'Jardim Camburi'),
    ('11211209', 'Jardim da Penha'),
    ('02394822', 'Praia do Canto'),
    ('27847584', 'Mata da Praia');
    
    INSERT INTO COMODO (codigo, nome_complemento,FK_RESIDENCIA_codigo) VALUES
        ('19284661','banheiro','12341324'),
        ('24357988','cozinha','51324312'),
        ('44433430','lavabo','17658214'),
        ('12395731','lavanderia','85676523'),
        ('11111006','quintal','63451342'),
        ('44445733','garagem','53453462'),
        ('12000912','banheiro','12454326'),
        ('65000765','cozinha','76745454'),
        ('98644432','lavabo','32456734'),
        ('55553332','lavanderia','73452345'),
        ('12367788','quintal','12415121'),
        ('88886664','garagem','12213421'),
        ('11111338','banheiro','53432243'),
        ('12456333','cozinha','51231424'),
        ('88889901','lavabo','15324123');


    INSERT INTO SENSOR (codigo,latitude,longitude,FK_RESIDENCIA_codigo,FK_COMODO_codigo) VALUES
    ('32325656',40.601203,-9.665677,'12341324','19284661'),
    ('76763526',40.601201,-15.668173,'51324312','24357988'),
    ('64444566',21.601203,-12.668173,'17658214','44433430'),
    ('09875665',45.601203,-11.668173,'85676523','12395731'),
    ('00056730',46.861203,-8.467173,'63451342','11111006'),
    ('11100421',33.601203,-10.668173,'53453462','44445733'),
    ('66644200',20.701203,-9.668173,'12454326','12000912'),
    ('00000004',21.601203,-1.668173,'76745454','65000765'),
    ('44446678',87.601203,-2.668173,'32456734','98644432'),
    ('11235543',40.601281,-3.668173,'73452345','55553332'),
    ('09876567',28.601203,-4.668173,'12415121','12367788'),
    ('44565686',29.601203,-5.668173,'12213421','88886664'),
    ('47865322',30.605203,-6.668173,'53432243','11111338'),
    ('10000444',12.601203,-7.668173,'51231424','12456333'),
    ('10033854',50.501203,-8.668173,'15324123','88889901');
    
    
    INSERT INTO DADO (codigo, data_hora, valor,FK_SENSOR_codigo) VALUES
    ('22233227','2019-04-02 00:00:00','10','32325656'),
    ('23478999','2019-04-01 13:20:00','1','76763526'),
    ('54674478','2018-12-19 15:20:00','1','64444566'),
    ('13453367','2017-06-01 01:00:00','2','09875665'),
    ('97465554','2019-09-01 07:45:00','2','00056730'),
    ('22333434','2019-04-14 08:13:00','4','11100421'),
    ('89886676','2016-05-30 12:32:00','6','66644200'),
    ('34456777','2015-10-27 16:07:00','25','00000004'),
    ('33466685','2019-11-17 06:00:0','4','44446678'),
    ('34456765','2018-09-14 20:10:00','4','11235543'),
    ('53435455','2019-09-14 20:10:00','2','09876567'),
    ('78967832','2019-09-14 14:10:00','1','44565686'),
    ('96786572','2018-06-01 01:10:00','1','47865322'),
    ('34576444','2018-06-01 01:10:00','9','10000444'),
    ('87655554','2010-08-04 00:14:00','9','10033854');
    
      INSERT INTO RESIDENCIA (CODIGO,TIPO_LOGRADOURO,NOME_LOGRADOURO,NUMERO,COMPLEMENTO,FK_BAIRRO_CODIGO,FK_ESTADO_CODIGO,FK_MUNICIPIO_CODIGO)
     VALUES
     ('12341324', 'R', 'Minas Gerais', 1290, 'Apartamento 1001', '23412890', '77777444', '45454545'),
     ('51324312', 'R', 'Marataízes', 301, 'Apartamento 504', '23412890', '77777444','45454545'),
     ('17658214', 'Av', 'Paulo Pereira Gomes', 500, Null, '23412890', '77777444', '45454545'),
     ('85676523', 'Av', 'Civit', 240, 'Apartamento 301', '23412890', '77777444', '45454545'),
     ('63451342', 'Av', 'Leandro Hassun', 120, Null, '27495723', '77777444', '45454545'),
     ('53453462', 'R', 'Nando Moura', 730, 'Apartamento 1502', '00112334', '77777444', '08638656'),
     ('12454326', 'R', 'Humberto Serrano', 280, 'Apartamento 903', '04589278', '77777444', '08638656'),
     ('76745454', 'Av', 'Champangnat', 1020, Null, '04589278', '77777444', '08638656'),
     ('32456734', 'R', 'Rômulo Mendonça', 410, 'Apartamento 201', '02394822', '77777444', '45534878'),
     ('73452345', 'R', 'Augusto Carrara', 820, 'Apartamento 1604', '09280549', '77777444', '45534878');
     ('12415121', 'R', 'Castelo Branco', 1540, 'Apartamento 1304', '24523656', '77777444', '08638656'),
     ('12213421', 'R', 'Marataízes', 301, 'Apartamento 504', '24523656', '77777444','08638656'),
     ('53432243', 'Av', 'Júlio Prestes', 3200, Null, '24523656', '77777444', '08638656'),
     ('51231424', 'R', 'Deodoro da Fonseca', 240, 'Apartamento 230', '27847584', '77777444', '45534878'),
     ('15324123', 'Av', 'Eurico Gaspar Dutra', 560, Null, '27847584', '77777444', '45534878');
     
     INSERT INTO USUARIO_RESIDENCIA(FK_USUARIO_CODIGO, FK_RESIDENCIA_CODIGO) VALUES
     ('2345222', '12341324'),
     ('2345222', '63451342'),
     ('2345222','17658214' ),
     ('8709431','63451342' ),
     ('8709431', '12341324'),
     ('8709431', '51231424'),
     ('1254673', '51231424'),
     ('1254673', '51324312'),
     ('1254673', '15324123'),
     ('9908466', '15324123'),
     ('9908466', '51324312'),
     ('9908466', '53432243'),
     ('1252705','17658214' ),
     ('1252705', '53432243'),
     ('1252705', '76745454'),
     ('4527869', '12213421'),
     ('4527869', '76745454'),
     ('4527869','85676523' ),
     ('1574806', '32456734'),
     ('1574806', '12415121'),
     ('1574806','85676523' ),
     ('3368907', '12213421'),
     ('3368907','12415121' ),
     ('3368907','12454326' ),
     ('6774322', '32456734'),
     ('6774322', '53453462'),
     ('6774322', '73452345'),
     ('2566788', '73452345'),
     ('2566788', '53453462'),
     ('2566788', '12454326');
        
#### 8.3 INCLUSÃO DO SCRIPT PARA EXCLUSÃO DE TABELAS EXISTENTES, CRIAÇÃO DE TABELA NOVAS E INSERÇÃO DOS DADOS
         
         DROP TABLE USUARIO;
         
         DROP TABLE RESIDENCIA;
         
         DROP TABLE SENSOR;
         
         DROP TABLE DADO;
         
         DROP TABLE Usuario_residencia;
         
         DROP TABLE COMODO;
         
         DROP TABLE ESTADO;
         
         DROP TABLE MUNICIPIO;
         
         DROP TABLE BAIRRO;
         
    CREATE TABLE USUARIO (
    codigo varchar(8) PRIMARY KEY,
    nome varchar(50),
    cpf varchar(20)
    );

    CREATE TABLE SENSOR (
        codigo varchar(8) PRIMARY KEY,
        latitude float,
        longitude float,
        FK_RESIDENCIA_codigo varchar(8),
        FK_COMODO_codigo varchar(8)
    );

    CREATE TABLE RESIDENCIA (
        codigo varchar(8) PRIMARY KEY,
        tipo_logradouro varchar(20),
        nome_logradouro varchar(50),
        numero int,
        complemento varchar(50),
        FK_BAIRRO_codigo varchar(8),
        FK_ESTADO_codigo varchar(8),
        FK_MUNICIPIO_codigo varchar(8)
    );

    CREATE TABLE ESTADO (
        nome varchar(20),
        codigo varchar(8) PRIMARY KEY
    );

    CREATE TABLE BAIRRO (
        nome varchar(20),
        codigo varchar(8) PRIMARY KEY
    );

    CREATE TABLE MUNICIPIO (
        codigo varchar(8) PRIMARY KEY,
        nome varchar(20)
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

    ALTER TABLE SENSOR ADD CONSTRAINT FK_SENSOR_3
        FOREIGN KEY (FK_COMODO_codigo)
        REFERENCES COMODO (codigo)
        ON DELETE RESTRICT;

    ALTER TABLE RESIDENCIA ADD CONSTRAINT FK_RESIDENCIA_2
        FOREIGN KEY (FK_BAIRRO_codigo)
        REFERENCES BAIRRO (codigo)
        ON DELETE RESTRICT;

    ALTER TABLE RESIDENCIA ADD CONSTRAINT FK_RESIDENCIA_3
        FOREIGN KEY (FK_ESTADO_codigo)
        REFERENCES ESTADO (codigo)
        ON DELETE RESTRICT;

    ALTER TABLE RESIDENCIA ADD CONSTRAINT FK_RESIDENCIA_4
        FOREIGN KEY (FK_MUNICIPIO_codigo)
        REFERENCES MUNICIPIO (codigo)
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

    INSERT INTO USUARIO (codigo, nome, cpf) VALUES
     ('2345222', 'Mariana Tassan', 12345678910),	
     ('8709431', 'Gabriel Marinho', 23456789101),
     ('1254673', 'Augusto Silva', 34567891011),
     ('9908466', 'Juliana Nogueira', 45678910111),	
     ('1252705', 'Felipe Souza', 56789101112),
     ('4527869', 'Emanuel Andrade', 67891011121),	
     ('1574806', 'Suzana Pereira', 78910111213),	
     ('3368907', 'Marcos Ferraz', 89101112131),
     ('6774322', 'Priscila Pinto', 91011121314),	
     ('2566788', 'João Ferreira', 10111213141),	
     ('4444121', 'Mariana Pinto', 11739825458),
     ('5553331', 'Mariana Ferraz', 98754482288),
     ('9990003', 'Mariana Gabriel', 33387497825),
     ('2572557', 'Mariana Júlia', 75395128642),
     ('5462222', 'Gabriel Rego', 14725896495);
    
    INSERT INTO ESTADO (CODIGO, NOME) VALUES
    ('11333222', 'Rio de Janeiro'),
    ('00008887', 'São Paulo'),
    ('77777444', 'Espírito Santo'),
    ('22222655', 'Bahia'),
    ('78907889', 'Santa Catarina'),
    ('77776655', 'Minas Gerais'),
    ('12596643', 'Acre'),
    ('89766786', 'Amazonas'),
    ('87456655', 'Roraima'),
    ('09876452', 'Pernambuco'),
    ('11663335', 'Ceará'),
    ('76578853', 'Rondônia'),
    ('36666676', 'Mato Grosso'),
    ('88888555', 'Goiás'),
    ('96676765', 'Paraná');
    
    INSERT INTO MUNICIPIO (CODIGO, NOME) VALUES
    ('00088788', 'Rio de Janeiro'),
    ('12346665', 'São Paulo'),
    ('45534878', 'Vitória'),
    ('45454545', 'Serra'),
    ('08638656', 'Vila Velha'),
    ('33333355', 'Florianópolis'),
    ('75646756', 'Búzios'),
    ('88948478', 'Recife'),
    ('21121211', 'Paraty'),
    ('98788566', 'Salvador'),
    ('66688866', 'Cariacica'),
    ('23567356', 'Viana'),
    ('28356856', 'Campinas'),
    ('90656777', 'Natal'),
    ('12489000', 'Porto Alegre');
    
    INSERT INTO BAIRRO (CODIGO, NOME) VALUES
    ('23412890', 'Morada laranjeiras'),
    ('78945499', 'Laranjeiras'),
    ('78907894', 'Colina Laranjeiras'),
    ('92378673', 'Valparaíso'),
    ('27495723', 'Feu Rosa'),
    ('17234665', 'Nova Carapina'),
    ('04589278', 'Praia da Costa'),
    ('00112334', 'Itaparica'),
    ('47384783', 'Itapuã'),
    ('39292229', 'Novo México'),
    ('24523656', 'Jardim Colorado'),
    ('09280549', 'Jardim Camburi'),
    ('11211209', 'Jardim da Penha'),
    ('02394822', 'Praia do Canto'),
    ('27847584', 'Mata da Praia');
    
    INSERT INTO COMODO (codigo, nome_complemento,FK_RESIDENCIA_codigo) VALUES
    ('19284661','banheiro','00665324'),
    ('24357988','cozinha','15555332'),
    ('44433430','lavabo','22255683'),
    ('12395731','lavanderia','15297655'),
    ('11111006','quintal','55333532'),
    ('44445733','garagem','09757444'),
    ('12000912','banheiro','11111112'),
    ('65000765','cozinha','22222215'),
    ('98644432','lavabo','63333544'),
    ('55553332','lavanderia','44544336'),
    ('12367788','quintal','10100202'),
    ('88886664','garagem','22765566'),
    ('11111338','banheiro','12312223'),
    ('12456333','cozinha','33345899'),
    ('88889901','lavabo','66767676');

    INSERT INTO SENSOR (codigo,latitude,longitude,FK_RESIDENCIA_codigo,FK_COMODO_codigo) VALUES
     ('32325656',40.601203,-9.665677,'12341324','19284661'),
     ('76763526',40.601201,-15.668173,'51324312','24357988'),
     ('64444566',21.601203,-12.668173,'17658214','44433430'),
     ('09875665',45.601203,-11.668173,'85676523','12395731'),
     ('00056730',46.861203,-8.467173,'63451342','11111006'),
     ('11100421',33.601203,-10.668173,'53453462','44445733'),
     ('66644200',20.701203,-9.668173,'12454326','12000912'),
     ('00000004',21.601203,-1.668173,'76745454','65000765'),
     ('44446678',87.601203,-2.668173,'32456734','98644432'),
     ('11235543',40.601281,-3.668173,'73452345','55553332'),
     ('09876567',28.601203,-4.668173,'12415121','12367788'),
     ('44565686',29.601203,-5.668173,'12213421','88886664'),
     ('47865322',30.605203,-6.668173,'53432243','11111338'),
     ('10000444',12.601203,-7.668173,'51231424','12456333'),
     ('10033854',50.501203,-8.668173,'15324123','88889901');

    INSERT INTO DADO (codigo, data_hora, valor,FK_SENSOR_codigo) VALUES
    ('22233227','2019-04-02 00:00:00','10','32325656'),
    ('23478999','2019-04-01 13:20:00','1','76763526'),
    ('54674478','2018-12-19 15:20:00','1','64444566'),
    ('13453367','2017-06-01 01:00:00','2','09875665'),
    ('97465554','2019-09-01 07:45:00','2','00056730'),
    ('22333434','2019-04-14 08:13:00','4','11100421'),
    ('89886676','2016-05-30 12:32:00','6','66644200'),
    ('34456777','2015-10-27 16:07:00','25','00000004'),
    ('33466685','2019-11-17 06:00:0','4','44446678'),
    ('34456765','2018-09-14 20:10:00','4','11235543'),
    ('53435455','2019-09-14 20:10:00','2','09876567'),
    ('78967832','2019-09-14 14:10:00','1','44565686'),
    ('96786572','2018-06-01 01:10:00','1','47865322'),
    ('34576444','2018-06-01 01:10:00','9','10000444'),
    ('87655554','2010-08-04 00:14:00','9','10033854');
    
      INSERT INTO RESIDENCIA (CODIGO,TIPO_LOGRADOURO,NOME_LOGRADOURO,NUMERO,COMPLEMENTO,FK_BAIRRO_CODIGO,FK_ESTADO_CODIGO,FK_MUNICIPIO_CODIGO)
     VALUES
     ('12341324', 'R', 'Minas Gerais', 1290, 'Apartamento 1001', '23412890', '77777444', '45454545'),
     ('51324312', 'R', 'Marataízes', 301, 'Apartamento 504', '23412890', '77777444','45454545'),
     ('17658214', 'Av', 'Paulo Pereira Gomes', 500, Null, '23412890', '77777444', '45454545'),
     ('85676523', 'Av', 'Civit', 240, 'Apartamento 301', '23412890', '77777444', '45454545'),
     ('63451342', 'Av', 'Leandro Hassun', 120, Null, '27495723', '77777444', '45454545'),
     ('53453462', 'R', 'Nando Moura', 730, 'Apartamento 1502', '00112334', '77777444', '08638656'),
     ('12454326', 'R', 'Humberto Serrano', 280, 'Apartamento 903', '04589278', '77777444', '08638656'),
     ('76745454', 'Av', 'Champangnat', 1020, Null, '04589278', '77777444', '08638656'),
     ('32456734', 'R', 'Rômulo Mendonça', 410, 'Apartamento 201', '02394822', '77777444', '45534878'),
     ('73452345', 'R', 'Augusto Carrara', 820, 'Apartamento 1604', '09280549', '77777444', '45534878');
     ('12415121', 'R', 'Castelo Branco', 1540, 'Apartamento 1304', '24523656', '77777444', '08638656'),
     ('12213421', 'R', 'Marataízes', 301, 'Apartamento 504', '24523656', '77777444','08638656'),
     ('53432243', 'Av', 'Júlio Prestes', 3200, Null, '24523656', '77777444', '08638656'),
     ('51231424', 'R', 'Deodoro da Fonseca', 240, 'Apartamento 230', '27847584', '77777444', '45534878'),
     ('15324123', 'Av', 'Eurico Gaspar Dutra', 560, Null, '27847584', '77777444', '45534878');
     
     INSERT INTO USUARIO_RESIDENCIA(FK_USUARIO_CODIGO, FK_RESIDENCIA_CODIGO) VALUES
     ('2345222', '12341324'),
     ('2345222', '63451342'),
     ('2345222','17658214' ),
     ('8709431','63451342' ),
     ('8709431', '12341324'),
     ('8709431', '51231424'),
     ('1254673', '51231424'),
     ('1254673', '51324312'),
     ('1254673', '15324123'),
     ('9908466', '15324123'),
     ('9908466', '51324312'),
     ('9908466', '53432243'),
     ('1252705','17658214' ),
     ('1252705', '53432243'),
     ('1252705', '76745454'),
     ('4527869', '12213421'),
     ('4527869', '76745454'),
     ('4527869','85676523' ),
     ('1574806', '32456734'),
     ('1574806', '12415121'),
     ('1574806','85676523' ),
     ('3368907', '12213421'),
     ('3368907','12415121' ),
     ('3368907','12454326' ),
     ('6774322', '32456734'),
     ('6774322', '53453462'),
     ('6774322', '73452345'),
     ('2566788', '73452345'),
     ('2566788', '53453462'),
     ('2566788', '12454326');

## Marco de Entrega 08 em: (29/05/2019)<br>

### 9	TABELAS E PRINCIPAIS CONSULTAS<br>
    OBS: Incluir para cada tópico as instruções SQL + imagens (print da tela) mostrando os resultados.<br>
#### 9.1	CONSULTAS DAS TABELAS COM TODOS OS DADOS INSERIDOS (Todas) <br>
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/residencia.PNG?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/usuario1000.png?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/usuario_residencia.PNG?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/Sensor.PNG?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/bairro.png?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/Estado.PNG?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/municipio.png?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/Comodo.PNG?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/Dado.PNG?raw="true" "little")


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


        
        


    





