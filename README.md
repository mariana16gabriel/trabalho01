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
![Balsamiq - Hydro Economizer](https://github.com/mariana16gabriel/trabalho01/blob/master/balsamiq.pdf?raw="true" "little")

#### 4.1 QUAIS PERGUNTAS PODEM SER RESPONDIDAS COM O SISTEMA PROPOSTO?

O projeto Hydro Economizer precisa inicialmente dos seguintes relatórios:

1º) Em que horas na semana o consumo é mais elevado em cada residência?

2º) Qual o consumo médio de um bairro?

3º) Qual a diferença entre o maior e o menor consumo por Estado?

4º) Em quais municípios se encontram mais usuários?

5º) O quanto de água foi consumido numa rua ou avenida em uma semana? 



#### 4.2 TABELA DE DADOS DO SISTEMA:
[Exemplo de Tabela de dados do Hydro Economizer]
https://github.com/mariana16gabriel/trabalho01/blob/master/tabelas_Hydro_Economizer.xlsx

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
     

#### 8.1 DETALHAMENTO DAS INFORMAÇÕES- DADOS FAKES
    fake_person_names = Person('pt')
    for i in range(20):
      cpf = ""
      for j in range(11):
        cpf += str(randint(0, 9))
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      name = fake_person_names.full_name()
      data = (code, name, cpf)

      cur.execute("start transaction")
      insert = """insert into usuario (codigo, nome, cpf) values (%s,%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    fakes = Factory.create('pt_BR')
    code = ""
    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      city = fakes.city()
      data = (code, city)
      cur.execute("start transaction")
      insert = """insert into municipio (codigo, nome) values (%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    fakes = Factory.create('pt_BR')
    ba = ['Morada laranjeiras','Laranjeiras','Colina Laranjeiras','Valparaíso','Feu Rosa', 'Nova Carapina', 'Praia da Costa', 'Itaparica', 'Itapuã', 'Novo México', 'Jardim Colorado',
          'Jardim Camburi', 'Jardim da Penha', 'Praia do Canto', 'Mata da Praia', 'Padre Gabriel','Bairro República','Taquara II','Flechal II','Flechal I']
    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      data = (code, ba[i])
      cur.execute("start transaction")
      insert = """insert into bairro (codigo, nome) values (%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    fakes = Factory.create('pt_BR')
    es = ['Rio de Janeiro', 'São Paulo', 'Espírito Santo', 'Bahia', 'Santa Catarina', 'Minas Gerais', 'Acre', 'Amazonas', 'Roraima', 'Pernambuco', 'Ceará',
          'Rondônia','Pará', 'Mato Grosso', 'Goiás', 'Paraná','Alagoas','Paraíba','Piauí','Maranhão']
    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      data = (code, es[i])
      cur.execute("start transaction")
      insert = """insert into estado(codigo, nome) values (%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    cur.execute("start transaction")
    insert = """select codigo from municipio"""
    cur.execute(insert, data)
    result = cur.fetchall()
    cur.execute("commit")
    lstcodemun = []
    for i in range(len(result)):
      lstcodemun.append(result[i][0])

    cur.execute("start transaction")
    insert = """select codigo from bairro"""
    cur.execute(insert, data)
    result2 = cur.fetchall()
    cur.execute("commit")
    lstcodebai = []
    for i in range(len(result2)):
      lstcodebai.append(result2[i][0])

    cur.execute("start transaction")
    insert = """select codigo from estado"""
    cur.execute(insert, data)
    result3 = cur.fetchall()
    cur.execute("commit")
    lstcodeest = []
    for i in range(len(result3)):
      lstcodeest.append(result3[i][0])

    for i in range(20):
      print(lstcodebai[randint(0, len(lstcodebai)- 1)], lstcodemun[randint(0, len(lstcodemun)- 1)], lstcodeest[randint(0, len(lstcodeest)- 1)])

    fake = Factory.create('pt_BR')
    for k in range(20):
      r = fake.street_name()
      lstr = r.split(" ")
      prefix = ""
      for i in range(len(lstr)):
        if i == 0:
          if lstr[i][0:3] != "Rua":
            prefix = lstr[i][0:2]
          else: prefix = lstr[i][0:1]
      nom = ""
      for i in range(1,len(lstr)):
        nom += lstr[i] + " "
      num = fake.building_number()
      rnd = randint(0, 2)
      comp = ""
      if rnd == 0:
        comp = None
      elif rnd ==1:
        comp = "Apartamento " + str(randint(0, 1)) + str(randint(0, 9)) + "0" + str(randint(1, 4))
      elif rnd == 2:
        comp = "Lote " + str(randint(1, 10))

      cur.execute("start transaction")
      insert = """select codigo from municipio"""
      cur.execute(insert)
      result = cur.fetchall()
      cur.execute("commit")
      lstcodemun = []
      for i in range(len(result)):
        lstcodemun.append(result[i][0])

      cur.execute("start transaction")
      insert = """select codigo from bairro"""
      cur.execute(insert)
      result2 = cur.fetchall()
      cur.execute("commit")
      lstcodebai = []
      for i in range(len(result2)):
        lstcodebai.append(result2[i][0])

      cur.execute("start transaction")
      insert = """select codigo from estado"""
      cur.execute(insert)
      result3 = cur.fetchall()
      cur.execute("commit")
      lstcodeest = []
      for i in range(len(result3)):
        lstcodeest.append(result3[i][0])

      code = ""
      for j in range(7):
        code += str(randint(0, 9))

      bai = lstcodebai[randint(0, len(lstcodebai)- 1)]
      mun = lstcodemun[randint(0, len(lstcodemun)- 1)]
      est = lstcodeest[randint(0, len(lstcodeest)- 1)]

      data  = (code, prefix, nom, num, comp, bai, est, mun)
      cur.execute("start transaction")
      insert = """insert into residencia(codigo, tipo_logradouro, nome_logradouro, numero, complemento, fk_bairro_codigo, fk_estado_codigo, fk_municipio_codigo) values (%s,%s,%s,%s,%s,%s,%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit") 
      #print(data)

    fake = Factory.create('pt_BR')
    fake.building_number()

    cur.execute("start transaction")
    insert = """select codigo from usuario"""
    cur.execute(insert)
    result = cur.fetchall()
    cur.execute("commit")
    lstu = []
    for j in range(len(result)):
        lstu.append(result[j][0])


    cur.execute("start transaction")
    insert = """select codigo from residencia"""
    cur.execute(insert)
    result2 = cur.fetchall()
    cur.execute("commit")
    lstr = []
    for j in range(len(result2)):
        lstr.append(result2[j][0])

    for i in range(100):
      data = (lstu[randint(0, len(lstu)-1)], lstr[randint(0, len(lstr)-1)])
      cur.execute("start transaction")
      insert = """insert into usuario_residencia(fk_usuario_codigo, fk_residencia_codigo) values (%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    cur.execute("start transaction")
    insert = """select codigo from residencia"""
    cur.execute(insert)
    result2 = cur.fetchall()
    cur.execute("commit")
    lstr = []
    for j in range(len(result2)):
        lstr.append(result2[j][0])
    com = ['lavabo','banheiro','quintal','cozinha','lavanderia','garagem']

    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      data = (code, com[randint(0, len(com)-1)], lstr[randint(0, len(lstr) - 1)])
      cur.execute("start transaction")
      insert = """insert into comodo(codigo, nome_complemento, fk_residencia_codigo) values (%s,%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    fake = Factory.create()


    cur.execute("start transaction")
    insert = """select codigo from residencia"""
    cur.execute(insert)
    result = cur.fetchall()
    cur.execute("commit")
    lstr = []
    for j in range(len(result)):
        lstr.append(result[j][0])

    cur.execute("start transaction")
    insert = """select codigo from comodo"""
    cur.execute(insert)
    result2 = cur.fetchall()
    cur.execute("commit")
    lstc = []
    for j in range(len(result2)):
        lstc.append(result2[j][0])
    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      data = (code, float(fake.latitude()), float(fake.longitude()), lstr[randint(0, len(lstr)-1)], lstc[randint(0, len(lstc)-1)])
      cur.execute("start transaction")
      insert = """insert into sensor(codigo, latitude, longitude, fk_residencia_codigo, fk_comodo_codigo) values (%s,%s,%s,%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    from random import uniform

    cur.execute("start transaction")
    insert = """select codigo from sensor"""
    cur.execute(insert)
    result = cur.fetchall()
    cur.execute("commit")
    lsts = []
    for j in range(len(result)):
        lsts.append(result[j][0])
    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      date = str(fake.date_time(tzinfo=None, end_datetime=None))
      val = round(uniform(0.0, 100.0), 2)

      data = (code, date, val, lsts[randint(0, len(lsts) -1)])
      cur.execute("start transaction")
      insert = """insert into dado(codigo, data_hora, valor, fk_sensor_codigo) values (%s,%s,%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    cur.execute("commit")


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

       fake_person_names = Person('pt')
    for i in range(20):
      cpf = ""
      for j in range(11):
        cpf += str(randint(0, 9))
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      name = fake_person_names.full_name()
      data = (code, name, cpf)

      cur.execute("start transaction")
      insert = """insert into usuario (codigo, nome, cpf) values (%s,%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    fakes = Factory.create('pt_BR')
    code = ""
    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      city = fakes.city()
      data = (code, city)
      cur.execute("start transaction")
      insert = """insert into municipio (codigo, nome) values (%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    fakes = Factory.create('pt_BR')
    ba = ['Morada laranjeiras','Laranjeiras','Colina Laranjeiras','Valparaíso','Feu Rosa', 'Nova Carapina', 'Praia da Costa', 'Itaparica', 'Itapuã', 'Novo México', 'Jardim Colorado',
          'Jardim Camburi', 'Jardim da Penha', 'Praia do Canto', 'Mata da Praia', 'Padre Gabriel','Bairro República','Taquara II','Flechal II','Flechal I']
    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      data = (code, ba[i])
      cur.execute("start transaction")
      insert = """insert into bairro (codigo, nome) values (%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    fakes = Factory.create('pt_BR')
    es = ['Rio de Janeiro', 'São Paulo', 'Espírito Santo', 'Bahia', 'Santa Catarina', 'Minas Gerais', 'Acre', 'Amazonas', 'Roraima', 'Pernambuco', 'Ceará',
          'Rondônia','Pará', 'Mato Grosso', 'Goiás', 'Paraná','Alagoas','Paraíba','Piauí','Maranhão']
    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      data = (code, es[i])
      cur.execute("start transaction")
      insert = """insert into estado(codigo, nome) values (%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    cur.execute("start transaction")
    insert = """select codigo from municipio"""
    cur.execute(insert, data)
    result = cur.fetchall()
    cur.execute("commit")
    lstcodemun = []
    for i in range(len(result)):
      lstcodemun.append(result[i][0])

    cur.execute("start transaction")
    insert = """select codigo from bairro"""
    cur.execute(insert, data)
    result2 = cur.fetchall()
    cur.execute("commit")
    lstcodebai = []
    for i in range(len(result2)):
      lstcodebai.append(result2[i][0])

    cur.execute("start transaction")
    insert = """select codigo from estado"""
    cur.execute(insert, data)
    result3 = cur.fetchall()
    cur.execute("commit")
    lstcodeest = []
    for i in range(len(result3)):
      lstcodeest.append(result3[i][0])

    for i in range(20):
      print(lstcodebai[randint(0, len(lstcodebai)- 1)], lstcodemun[randint(0, len(lstcodemun)- 1)], lstcodeest[randint(0, len(lstcodeest)- 1)])

    fake = Factory.create('pt_BR')
    for k in range(20):
      r = fake.street_name()
      lstr = r.split(" ")
      prefix = ""
      for i in range(len(lstr)):
        if i == 0:
          if lstr[i][0:3] != "Rua":
            prefix = lstr[i][0:2]
          else: prefix = lstr[i][0:1]
      nom = ""
      for i in range(1,len(lstr)):
        nom += lstr[i] + " "
      num = fake.building_number()
      rnd = randint(0, 2)
      comp = ""
      if rnd == 0:
        comp = None
      elif rnd ==1:
        comp = "Apartamento " + str(randint(0, 1)) + str(randint(0, 9)) + "0" + str(randint(1, 4))
      elif rnd == 2:
        comp = "Lote " + str(randint(1, 10))

      cur.execute("start transaction")
      insert = """select codigo from municipio"""
      cur.execute(insert)
      result = cur.fetchall()
      cur.execute("commit")
      lstcodemun = []
      for i in range(len(result)):
        lstcodemun.append(result[i][0])

      cur.execute("start transaction")
      insert = """select codigo from bairro"""
      cur.execute(insert)
      result2 = cur.fetchall()
      cur.execute("commit")
      lstcodebai = []
      for i in range(len(result2)):
        lstcodebai.append(result2[i][0])

      cur.execute("start transaction")
      insert = """select codigo from estado"""
      cur.execute(insert)
      result3 = cur.fetchall()
      cur.execute("commit")
      lstcodeest = []
      for i in range(len(result3)):
        lstcodeest.append(result3[i][0])

      code = ""
      for j in range(7):
        code += str(randint(0, 9))

      bai = lstcodebai[randint(0, len(lstcodebai)- 1)]
      mun = lstcodemun[randint(0, len(lstcodemun)- 1)]
      est = lstcodeest[randint(0, len(lstcodeest)- 1)]

      data  = (code, prefix, nom, num, comp, bai, est, mun)
      cur.execute("start transaction")
      insert = """insert into residencia(codigo, tipo_logradouro, nome_logradouro, numero, complemento, fk_bairro_codigo, fk_estado_codigo, fk_municipio_codigo) values (%s,%s,%s,%s,%s,%s,%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit") 
      #print(data)

    fake = Factory.create('pt_BR')
    fake.building_number()

    cur.execute("start transaction")
    insert = """select codigo from usuario"""
    cur.execute(insert)
    result = cur.fetchall()
    cur.execute("commit")
    lstu = []
    for j in range(len(result)):
        lstu.append(result[j][0])


    cur.execute("start transaction")
    insert = """select codigo from residencia"""
    cur.execute(insert)
    result2 = cur.fetchall()
    cur.execute("commit")
    lstr = []
    for j in range(len(result2)):
        lstr.append(result2[j][0])

    for i in range(100):
      data = (lstu[randint(0, len(lstu)-1)], lstr[randint(0, len(lstr)-1)])
      cur.execute("start transaction")
      insert = """insert into usuario_residencia(fk_usuario_codigo, fk_residencia_codigo) values (%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    cur.execute("start transaction")
    insert = """select codigo from residencia"""
    cur.execute(insert)
    result2 = cur.fetchall()
    cur.execute("commit")
    lstr = []
    for j in range(len(result2)):
        lstr.append(result2[j][0])
    com = ['lavabo','banheiro','quintal','cozinha','lavanderia','garagem']

    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      data = (code, com[randint(0, len(com)-1)], lstr[randint(0, len(lstr) - 1)])
      cur.execute("start transaction")
      insert = """insert into comodo(codigo, nome_complemento, fk_residencia_codigo) values (%s,%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    fake = Factory.create()


    cur.execute("start transaction")
    insert = """select codigo from residencia"""
    cur.execute(insert)
    result = cur.fetchall()
    cur.execute("commit")
    lstr = []
    for j in range(len(result)):
        lstr.append(result[j][0])

    cur.execute("start transaction")
    insert = """select codigo from comodo"""
    cur.execute(insert)
    result2 = cur.fetchall()
    cur.execute("commit")
    lstc = []
    for j in range(len(result2)):
        lstc.append(result2[j][0])
    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      data = (code, float(fake.latitude()), float(fake.longitude()), lstr[randint(0, len(lstr)-1)], lstc[randint(0, len(lstc)-1)])
      cur.execute("start transaction")
      insert = """insert into sensor(codigo, latitude, longitude, fk_residencia_codigo, fk_comodo_codigo) values (%s,%s,%s,%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    from random import uniform

    cur.execute("start transaction")
    insert = """select codigo from sensor"""
    cur.execute(insert)
    result = cur.fetchall()
    cur.execute("commit")
    lsts = []
    for j in range(len(result)):
        lsts.append(result[j][0])
    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      date = str(fake.date_time(tzinfo=None, end_datetime=None))
      val = round(uniform(0.0, 100.0), 2)

      data = (code, date, val, lsts[randint(0, len(lsts) -1)])
      cur.execute("start transaction")
      insert = """insert into dado(codigo, data_hora, valor, fk_sensor_codigo) values (%s,%s,%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    cur.execute("commit")

        
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

    fake_person_names = Person('pt')
    for i in range(20):
      cpf = ""
      for j in range(11):
        cpf += str(randint(0, 9))
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      name = fake_person_names.full_name()
      data = (code, name, cpf)

      cur.execute("start transaction")
      insert = """insert into usuario (codigo, nome, cpf) values (%s,%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    fakes = Factory.create('pt_BR')
    code = ""
    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      city = fakes.city()
      data = (code, city)
      cur.execute("start transaction")
      insert = """insert into municipio (codigo, nome) values (%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    fakes = Factory.create('pt_BR')
    ba = ['Morada laranjeiras','Laranjeiras','Colina Laranjeiras','Valparaíso','Feu Rosa', 'Nova Carapina', 'Praia da Costa', 'Itaparica', 'Itapuã', 'Novo México', 'Jardim Colorado',
          'Jardim Camburi', 'Jardim da Penha', 'Praia do Canto', 'Mata da Praia', 'Padre Gabriel','Bairro República','Taquara II','Flechal II','Flechal I']
    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      data = (code, ba[i])
      cur.execute("start transaction")
      insert = """insert into bairro (codigo, nome) values (%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    fakes = Factory.create('pt_BR')
    es = ['Rio de Janeiro', 'São Paulo', 'Espírito Santo', 'Bahia', 'Santa Catarina', 'Minas Gerais', 'Acre', 'Amazonas', 'Roraima', 'Pernambuco', 'Ceará',
          'Rondônia','Pará', 'Mato Grosso', 'Goiás', 'Paraná','Alagoas','Paraíba','Piauí','Maranhão']
    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      data = (code, es[i])
      cur.execute("start transaction")
      insert = """insert into estado(codigo, nome) values (%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    cur.execute("start transaction")
    insert = """select codigo from municipio"""
    cur.execute(insert, data)
    result = cur.fetchall()
    cur.execute("commit")
    lstcodemun = []
    for i in range(len(result)):
      lstcodemun.append(result[i][0])

    cur.execute("start transaction")
    insert = """select codigo from bairro"""
    cur.execute(insert, data)
    result2 = cur.fetchall()
    cur.execute("commit")
    lstcodebai = []
    for i in range(len(result2)):
      lstcodebai.append(result2[i][0])

    cur.execute("start transaction")
    insert = """select codigo from estado"""
    cur.execute(insert, data)
    result3 = cur.fetchall()
    cur.execute("commit")
    lstcodeest = []
    for i in range(len(result3)):
      lstcodeest.append(result3[i][0])

    for i in range(20):
      print(lstcodebai[randint(0, len(lstcodebai)- 1)], lstcodemun[randint(0, len(lstcodemun)- 1)], lstcodeest[randint(0, len(lstcodeest)- 1)])

    fake = Factory.create('pt_BR')
    for k in range(20):
      r = fake.street_name()
      lstr = r.split(" ")
      prefix = ""
      for i in range(len(lstr)):
        if i == 0:
          if lstr[i][0:3] != "Rua":
            prefix = lstr[i][0:2]
          else: prefix = lstr[i][0:1]
      nom = ""
      for i in range(1,len(lstr)):
        nom += lstr[i] + " "
      num = fake.building_number()
      rnd = randint(0, 2)
      comp = ""
      if rnd == 0:
        comp = None
      elif rnd ==1:
        comp = "Apartamento " + str(randint(0, 1)) + str(randint(0, 9)) + "0" + str(randint(1, 4))
      elif rnd == 2:
        comp = "Lote " + str(randint(1, 10))

      cur.execute("start transaction")
      insert = """select codigo from municipio"""
      cur.execute(insert)
      result = cur.fetchall()
      cur.execute("commit")
      lstcodemun = []
      for i in range(len(result)):
        lstcodemun.append(result[i][0])

      cur.execute("start transaction")
      insert = """select codigo from bairro"""
      cur.execute(insert)
      result2 = cur.fetchall()
      cur.execute("commit")
      lstcodebai = []
      for i in range(len(result2)):
        lstcodebai.append(result2[i][0])

      cur.execute("start transaction")
      insert = """select codigo from estado"""
      cur.execute(insert)
      result3 = cur.fetchall()
      cur.execute("commit")
      lstcodeest = []
      for i in range(len(result3)):
        lstcodeest.append(result3[i][0])

      code = ""
      for j in range(7):
        code += str(randint(0, 9))

      bai = lstcodebai[randint(0, len(lstcodebai)- 1)]
      mun = lstcodemun[randint(0, len(lstcodemun)- 1)]
      est = lstcodeest[randint(0, len(lstcodeest)- 1)]

      data  = (code, prefix, nom, num, comp, bai, est, mun)
      cur.execute("start transaction")
      insert = """insert into residencia(codigo, tipo_logradouro, nome_logradouro, numero, complemento, fk_bairro_codigo, fk_estado_codigo, fk_municipio_codigo) values (%s,%s,%s,%s,%s,%s,%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit") 
      #print(data)

    fake = Factory.create('pt_BR')
    fake.building_number()

    cur.execute("start transaction")
    insert = """select codigo from usuario"""
    cur.execute(insert)
    result = cur.fetchall()
    cur.execute("commit")
    lstu = []
    for j in range(len(result)):
        lstu.append(result[j][0])


    cur.execute("start transaction")
    insert = """select codigo from residencia"""
    cur.execute(insert)
    result2 = cur.fetchall()
    cur.execute("commit")
    lstr = []
    for j in range(len(result2)):
        lstr.append(result2[j][0])

    for i in range(100):
      data = (lstu[randint(0, len(lstu)-1)], lstr[randint(0, len(lstr)-1)])
      cur.execute("start transaction")
      insert = """insert into usuario_residencia(fk_usuario_codigo, fk_residencia_codigo) values (%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    cur.execute("start transaction")
    insert = """select codigo from residencia"""
    cur.execute(insert)
    result2 = cur.fetchall()
    cur.execute("commit")
    lstr = []
    for j in range(len(result2)):
        lstr.append(result2[j][0])
    com = ['lavabo','banheiro','quintal','cozinha','lavanderia','garagem']

    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      data = (code, com[randint(0, len(com)-1)], lstr[randint(0, len(lstr) - 1)])
      cur.execute("start transaction")
      insert = """insert into comodo(codigo, nome_complemento, fk_residencia_codigo) values (%s,%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    fake = Factory.create()


    cur.execute("start transaction")
    insert = """select codigo from residencia"""
    cur.execute(insert)
    result = cur.fetchall()
    cur.execute("commit")
    lstr = []
    for j in range(len(result)):
        lstr.append(result[j][0])

    cur.execute("start transaction")
    insert = """select codigo from comodo"""
    cur.execute(insert)
    result2 = cur.fetchall()
    cur.execute("commit")
    lstc = []
    for j in range(len(result2)):
        lstc.append(result2[j][0])
    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      data = (code, float(fake.latitude()), float(fake.longitude()), lstr[randint(0, len(lstr)-1)], lstc[randint(0, len(lstc)-1)])
      cur.execute("start transaction")
      insert = """insert into sensor(codigo, latitude, longitude, fk_residencia_codigo, fk_comodo_codigo) values (%s,%s,%s,%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    from random import uniform

    cur.execute("start transaction")
    insert = """select codigo from sensor"""
    cur.execute(insert)
    result = cur.fetchall()
    cur.execute("commit")
    lsts = []
    for j in range(len(result)):
        lsts.append(result[j][0])
    for i in range(20):
      code = ""
      for j in range(7):
        code += str(randint(0, 9))
      date = str(fake.date_time(tzinfo=None, end_datetime=None))
      val = round(uniform(0.0, 100.0), 2)

      data = (code, date, val, lsts[randint(0, len(lsts) -1)])
      cur.execute("start transaction")
      insert = """insert into dado(codigo, data_hora, valor, fk_sensor_codigo) values (%s,%s,%s,%s)"""
      cur.execute(insert, data)
      cur.execute("commit")

    cur.execute("commit")

## Marco de Entrega 08 em: (29/05/2019)<br>

### 9	TABELAS E PRINCIPAIS CONSULTAS<br>
    OBS: Incluir para cada tópico as instruções SQL + imagens (print da tela) mostrando os resultados.<br>
#### 9.1	CONSULTAS DAS TABELAS COM TODOS OS DADOS INSERIDOS (Todas) <br>
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/residencia.png?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/usuario1000.png?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/usuario_residencia.png?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/sensor.png?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/bairro.png?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/estado.png?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/municipio.png?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/comodo.png?raw="true" "little")
![Alt text](https://github.com/mariana16gabriel/trabalho01/blob/master/dado.png?raw="true" "little")


#### 9.2	CONSULTAS DAS TABELAS COM FILTROS WHERE (Mínimo 4)<br>
    1- select * from dado where valor > 50;
    2- select * from residencia where fk_estado_codigo = '1622796';
    3- select fk_residencia_codigo from usuario_residencia where fk_usuario_codigo = '3471000';
    4- select codigo as codigo_sensor from sensor where fk_comodo_codigo = '0160177';
 
#### 9.3	CONSULTAS QUE USAM OPERADORES LÓGICOS, ARITMÉTICOS E TABELAS OU CAMPOS RENOMEADOS (Mínimo 11)
    a) Criar 5 consultas que envolvam os operadores lógicos AND, OR e Not
    1- select * from sensor where latitude >-10 and longitude <40;
    2- select * from residencia where complemento is not null and fk_bairro_codigo = '0319033'
    3- select * from dado where valor > 60 or valor > 40;
    4- select * from comodo where nome_complemento = 'cozinha' and fk_residencia_codigo = '6208284'
    5- select * from residencia where tipo_logradouro = 'Fa' and complemento is null;
    
    b) Criar no mínimo 3 consultas com operadores aritméticos 
    1- select * from sensor where latitude/2 > 10;  
    2- select * from sensor where longitude+100 < 1; 
    3- select * from residencia where numero+10 > 5;
    
    c) Criar no mínimo 3 consultas com operação de renomear nomes de campos ou tabelas
    1- select nome_complemento as nome_comodo from comodo;
    2- select valor as litros from dado;
    3- select (tipo_logradouro,nome_logradouro,numero,complemento) as endereco from residencia;
    
#### 9.4	CONSULTAS QUE USAM OPERADORES LIKE E DATAS (Mínimo 12) <br>
    a) Criar outras 5 consultas que envolvam like ou ilike
    1- select * from comodo where nome_complemento like '%banheiro%';
    2- select nome as Usuarios from usuario where nome ilike 'c%';
    3- select * from usuario where nome like '%s';
    4- select * from residencia where nome_logradouro ilike 'de%' and numero>5;
    5- select * from estado where nome ilike 'r%';
    b) Criar uma consulta para cada tipo de função data apresentada.
    1- select codigo as codigo_dado, current_date - data_hora as dias_desde_coleta from dado;
    2- select codigo as codigo_dado, age(current_date, data_hora) as tempo_desde_coleta from dado;
    3- select codigo as codigo_dado, date_part('year', age(current_date, data_hora)) as anos_desde_coleta from dado;
    4- select codigo as codigo_dado, extract('month' from age(current_date,data_hora)) as meses_desde_coleta from dado;
    5- select codigo as codigo_dado, now() - data_hora as tempo_exato_desde_coleta from dado;
    6- select codigo as codigo_dado, extract('hour' from current_time) - extract('hour' from data_hora) as horas_desde_coleta from dado;
    7- select codigo as codigo_dado, extract('day' from now()) - extract('day' from data_hora) as dias_desde_coleta from dado;

  
#### 9.5	ATUALIZAÇÃO E EXCLUSÃO DE DADOS (Mínimo 6)<br>
    1- delete from residencia where complemento is null;
    2- delete from sensor where latitude>100 and longitude<100;
    3- update usuario set nome='Mariana Tassan' where cpf='20375978179';
    4- update residencia set tipo_logradouro='R' where numero=70;
    5- delete from usuario where nome ilike 'c%';
    6- update dado set data_hora='2019-11-07 19:45:55' where valor=64.8;


#### 9.6	CONSULTAS COM JUNÇÃO E ORDENAÇÃO (Mínimo 6)<br>
        a) Uma junção que envolva todas as tabelas possuindo no mínimo 3 registros no resultado
        1- 
         select * from usuario u inner join usuario_residencia ur
         on(u.codigo = ur.fk_usuario_codigo)
         inner join residencia r on(ur.fk_residencia_codigo = r.codigo)
         inner join bairro b on (b.codigo = r.fk_bairro_codigo)
         inner join estado e on (e.codigo = r.fk_estado_codigo)
         inner join municipio m on (m.codigo = r.fk_municipio_codigo)
         inner join comodo c on(c.fk_residencia_codigo = r.codigo)
         inner join sensor s on(s.fk_comodo_codigo = c.codigo)
         inner join residencia altr on(s.fk_residencia_codigo = altr.codigo)
         inner join dado d on(d.fk_sensor_codigo = s.codigo);
         
        b) Outras junções que o grupo considere como sendo as de principal importância para o trabalho
         2- 
         select * from usuario u inner join usuario_residencia ur
         on(u.codigo = ur.fk_usuario_codigo)
         inner join residencia r on(ur.fk_residencia_codigo = r.codigo)
         inner join sensor s on(s.fk_residencia_codigo = r.codigo)
         inner join dado d on(d.fk_sensor_codigo = s.codigo)

         3-
         select * from usuario u inner join usuario_residencia ur
         on(u.codigo = ur.fk_usuario_codigo)
         inner join residencia r on(ur.fk_residencia_codigo = r.codigo)
         inner join bairro b on (b.codigo = r.fk_bairro_codigo)
         inner join estado e on (e.codigo = r.fk_estado_codigo)
         inner join municipio m on (m.codigo = r.fk_municipio_codigo)
         
         4-
         select * from residencia r inner join comodo c 
         on(c.fk_residencia_codigo = r.codigo)
         inner join sensor s on(s.fk_comodo_codigo = c.codigo)
         
         5-
         select * from sensor s inner join dado d 
         on(s.codigo = d.fk_sensor_codigo);
         
         6-
         select * from residencia r inner join comodo c
         on(r.codigo = c.fk_residencia_codigo)
         inner join sensor s on(c.codigo = s.fk_comodo_codigo);

### ATUALIZAÇÃO DA DOCUMENTAÇÃO DOS SLIDES PARA APRESENTAÇAO SEMESTRAL (Mínimo 6 e Máximo 10)<br>


#### 9.7	CONSULTAS COM GROUP BY E FUNÇÕES DE AGRUPAMENTO (Mínimo 6)<br>
    1-
    select u.nome as nome_usuario, avg(d.valor) from usuario u inner join usuario_residencia ur
    on(u.codigo = ur.fk_usuario_codigo)
    inner join residencia r on(ur.fk_residencia_codigo = r.codigo)
    inner join sensor s on(s.fk_residencia_codigo = r.codigo)
    inner join dado d on(d.fk_sensor_codigo = s.codigo)
    group by u.nome;

    2-
    select m.nome as nome_municipio, sum(d.valor) as total_consumo from municipio m
    inner join residencia r on (r.fk_municipio_codigo = m.codigo)
    inner join sensor s on (s.fk_residencia_codigo = r.codigo)
    inner join dado d on (d.fk_sensor_codigo = s.codigo)
    group by m.nome;

    3-
    select r.numero as num_residencia, d.data_hora as hora_maior_consumo from residencia r
    inner join sensor s on(s.fk_residencia_codigo= r.codigo)
    inner join dado d on(d.fk_sensor_codigo = s.codigo)
    group by r.numero, d.data_hora
    order by sum(d.valor) desc;

    4-
    select e.nome, max(d.valor) - min(d.valor) as variacao_consumo from estado e
    inner join residencia r on (e.codigo = r.fk_estado_codigo)
    inner join sensor s on(s.fk_residencia_codigo = r.codigo)
    inner join dado d on(d.fk_sensor_codigo = s.codigo)
    group by e.nome;

    5-
    select b.nome as nome_bairro, count(u.codigo) as total_usuarios from usuario u
    inner join usuario_residencia ur on(u.codigo = ur.fk_usuario_codigo)
    inner join residencia r on(ur.fk_residencia_codigo = r.codigo)
    inner join bairro b on (b.codigo = r.fk_bairro_codigo)
    group by b.nome;

    6-
    select r.numero as numero_residencia, count(s.codigo) as total_sensores from residencia r
    inner join comodo c on(c.fk_residencia_codigo = r.codigo)
    inner join sensor s on(s.fk_comodo_codigo = c.codigo)
    group by r.numero; 

#### 9.8	CONSULTAS COM LEFT E RIGHT JOIN (Mínimo 4)<br>
    1-
    select u.nome as nome_usuario, r.numero as numero_residencia from usuario u
    left outer join usuario_residencia ur on (u.codigo = ur.fk_usuario_codigo)
    left outer join residencia r on (ur.fk_residencia_codigo = r.codigo);

    2-
    select s.codigo as codigo_sensor, d.codigo as codigo_dado from sensor s
    left outer join dado d on (s.codigo = d.fk_sensor_codigo);

    3-
    select s.codigo as codigo_sensor, c.codigo as codigo_comodo from sensor s
    right outer join comodo c on (s.fk_comodo_codigo = c.codigo);

    4-
    select r.codigo as codigo_residencia, b.nome as nome_bairro from residencia r
    right outer join bairro b on (r.fk_bairro_codigo = b.codigo);
#### 9.9	CONSULTAS COM SELF JOIN E VIEW (Mínimo 6)<br>
        a) Uma junção que envolva Self Join 
        b) Outras junções com views que o grupo considere como sendo de relevante importância para o trabalho
        create view tempo_coleta_dado as select codigo as codigo_dado, age(current_date, data_hora) as tempo_desde_coleta from dado;

#### 9.10	SUBCONSULTAS (Mínimo 3)<br>
        1- select * from dado where data_hora in (select data_hora from dado where date_part('year',age(current_date,data_hora))<15);


#### 9.11	LISTA DE CODIGOS DAS FUNÇÕES E TRIGGERS<br>
        Detalhamento sobre funcionalidade de cada código.
        a) Objetivo
        b) Código do objeto (função/trigger)
        cur.execute("""
        CREATE OR REPLACE FUNCTION insere_muda_residencia(cod_user VARCHAR(8), cod_nov_res VARCHAR(8), nov_tip_log VARCHAR(20), nov_nom_log VARCHAR(50),
         nov_num int, nov_comp VARCHAR(50), nov_cod_bai VARCHAR(8) , nov_cod_est VARCHAR(8), nov_cod_mun VARCHAR(8), nov_cod VARCHAR(8), cod_ant_res VARCHAR(8), 
         bol BOOLEAN)
          RETURNS trigger  AS 
        '
        BEGIN
          INSERT INTO residencia (codigo, tipo_logradouro, nome_logradouro, numero, complemento, fk_bairro_codigo, fk_estado_codigo, fk_municipio_codigo)
          VALUES (cod_nov_res, nov_tip_log, nov_nom_log, nov_num, nov_comp, nov_cod_bai, nov_cod_est, nov_cod_mun);
          INSERT INTO usuario_residencia (fk_usuario_codigo, fk_residencia_codigo) VALUES (cod_user, cod_nov_res);
          IF bol == True THEN
            DELETE FROM residencia WHERE residencia = cod_ant_res;
            DELETE FROM usuario_residencia WHERE fk_usuario_codigo = cod_user AND fk_residencia_codigo = cod_ant_res;
          END IF;

        END;
        '
        LANGUAGE plpgsql;

        CREATE TRIGGER verifica_mudanca
          BEFORE INSERT
          ON residencia
          EXECUTE PROCEDURE muda_residencia()

        """)
        c) exemplo de dados para aplicação
        d) resultados em forma de tabela/imagem
        1º gráfico- 
        teste = pd.read_sql_query("""
                            select m.nome as municipios, count(u.nome) as numero_usuarios from municipio m inner join residencia r on (m.codigo = r.fk_municipio_codigo) inner join usuario_residencia ur on (r.codigo = ur.fk_residencia_codigo) inner join usuario u on (ur.fk_usuario_codigo = u.codigo) group by m.nome order by count(u.nome) desc;
                            """,conn)
                            
        sns.barplot(y='municipios',x='numero_usuarios',data=teste).set_title("Quantidade de usuários por município")
        
        2º gráfico-
        teste2 = pd.read_sql_query("""
                            select bairro.nome as bairro, avg(dado.valor) as media from bairro inner join residencia on (bairro.codigo = residencia.fk_bairro_codigo) inner join sensor on (residencia.codigo = sensor.fk_residencia_codigo) inner join dado on (sensor.codigo = dado.fk_sensor_codigo) group by bairro.nome limit 5;
                            """,conn)
       
       sns.lineplot(teste2.bairro, teste2.media).set_title("Média de consumo por bairro")

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
    cur.execute("create table if not exists dado_com_indice as select * from dado");
    cur.execute("CREATE INDEX datas_posteriores ON dado_com_indice USING BTREE (data_hora);")
    cur.execute("""
    explain analyse select * 
    from dado
    inner join sensor
    on (dado.fk_sensor_codigo=sensor.codigo)
    inner join residencia
    on (sensor.fk_residencia_codigo=residencia.codigo)
    where data_hora>('1989-10-22')
    """);
    rows = cur.fetchall()
    for i in rows:
      print(i)
    cur.execute("commit")
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


        
        


    





