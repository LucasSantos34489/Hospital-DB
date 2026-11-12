# Hospital-DB
🏥 Sistema de Gerenciamento Hospitalar

Projeto acadêmico de Banco de Dados Relacional em MySQL, criado para organizar as informações de um hospital: pacientes, médicos, quartos, internações, consultas e prescrições.

📌 Contextualização

Um hospital lida o tempo todo com cadastros, atendimentos e internações. Quando esses dados ficam separados, aparecem problemas como:

Dificuldade para localizar informações de pacientes
Registros de consultas desorganizados
Falta de controle sobre internações e quartos
Dificuldade para consultar prescrições médicas
Informações repetidas ou inconsistentes
🎯 Proposta

Centralizar tudo em um banco de dados relacional (hospital_db), ligando as áreas do hospital por chaves primárias, chaves estrangeiras e relacionamentos entre tabelas. Assim, consultas, atualizações e relatórios usam a mesma base.

⚙️ Tecnologias
MySQL 8.0: sistema de gerenciamento de banco de dados
MySQL Workbench: modelagem, administração e execução das consultas
🗄️ Modelagem do Banco de Dados

O banco hospital_db tem 6 tabelas relacionadas.

Diagrama Entidade-Relacionamento (DER)

DER do hospital_db

Tabelas
1. pacientes

Dados cadastrais dos pacientes.

Campo	Tipo	Descrição
id_paciente	INT	Chave primária
nome_paciente	VARCHAR(100)	Nome do paciente
cpf	CHAR(14)	CPF único
data_nascimento	DATE	Data de nascimento
telefone_paciente	CHAR(15)	Telefone
convenio	CHAR(50)	Convênio médico

Na prática:

Tabela pacientes

2. medicos

Profissionais responsáveis pelos atendimentos.

Campo	Tipo	Descrição
id_medico	INT	Chave primária
nome_medico	CHAR(100)	Nome do médico
crm	CHAR(12)	CRM único
especialidade	CHAR(50)	Especialidade médica

Na prática:

Tabela medicos

3. quartos

Quartos do hospital.

Campo	Tipo	Descrição
id_quarto	INT	Chave primária
numero_quarto	CHAR(3)	Número único do quarto
andar	CHAR(3)	Andar
tipo_quarto	CHAR(30)	Categoria do quarto
status_quarto	CHAR(20)	Situação do quarto

Na prática:

Tabela quartos

4. internacoes

Internações dos pacientes e quartos utilizados.

Campo	Tipo	Descrição
id_internacao	INT	Chave primária
id_paciente	INT	Chave estrangeira
id_quarto	INT	Chave estrangeira
data_entrada	DATE	Data de entrada
data_saida	DATE	Data de saída
valor_diaria	DECIMAL(10,2)	Valor da diária

Na prática:

Tabela internacoes

5. consultas

Atendimentos realizados pelos médicos.

Campo	Tipo	Descrição
id_consulta	INT	Chave primária
data_consulta	DATETIME	Data e horário
status_consulta	CHAR(20)	Situação da consulta
valor_consulta	DECIMAL(10,2)	Valor do atendimento
id_paciente	INT	Chave estrangeira
id_medico	INT	Chave estrangeira

Na prática:

Tabela consultas

6. prescricoes

Medicamentos e orientações prescritos nas consultas.

Campo	Tipo	Descrição
id_prescricao	INT	Chave primária
id_consulta	INT	Chave estrangeira
medicamento	CHAR(100)	Nome do medicamento
dosagem	CHAR(50)	Orientação de uso
quantidade	CHAR(30)	Quantidade prescrita

Na prática:

Tabela prescricoes

🔗 Relacionamentos e Cardinalidade

Todos os relacionamentos são 1:N, e a chave estrangeira fica na tabela do lado "muitos":

pacientes → internacoes: um paciente pode ter várias internações; cada internação é de um paciente
quartos → internacoes: um quarto aparece em várias internações ao longo do tempo
pacientes → consultas: um paciente pode ter várias consultas
medicos → consultas: um médico atende várias consultas
consultas → prescricoes: uma consulta pode ter várias prescrições

As chaves estrangeiras (FK) garantem que os registros relacionados existam, e as chaves primárias (PK) identificam cada registro de forma única.

🚀 Como executar o projeto

Pré-requisitos: MySQL 8.0 ou superior e MySQL Workbench (ou outro cliente MySQL).

Abra a conexão no MySQL Workbench.
Execute o script de criação do banco e das tabelas (ele cria o hospital_db).
Execute o script de inserção de dados.
Confira se deu certo:
sql
USE hospital_db;
SHOW TABLES;   -- deve listar as 6 tabelas

Atenção: o script de criação usa DROP TABLE IF EXISTS, então executá-lo de novo recria as tabelas e apaga os dados existentes.

📊 Índices
Índice	Tabela	Campo
idx_pacientes_nome	pacientes	nome_paciente
idx_medicos_especialidade	medicos	especialidade
idx_consultas_data	consultas	data_consulta

Além desses, o MySQL cria índices para as chaves primárias, chaves estrangeiras e campos únicos.

📝 Consultas SQL

As 10 consultas ficam cada uma em sua pasta, no arquivo consulta.sql. Todas usam LIKE para filtrar texto e funções de agregação com GROUP BY.

1. Faturamento Total de Consultas por Convênio

Situação-problema: o setor financeiro precisa do valor total arrecadado em consultas para os convênios que têm "Saúde" no nome, para acompanhar o faturamento por parceiro.

Recursos: JOIN, LIKE, SUM, GROUP BY
Pasta: 01_faturamento_por_convenio

Resultado da consulta 1

2. Média do Valor de Consulta por Especialidade Médica

Situação-problema: a coordenação clínica quer a média do valor das consultas nas especialidades ligadas à Cardiologia.

Recursos: JOIN, LIKE, AVG, ROUND, GROUP BY
Pasta: 02_media_por_especialidade

Resultado da consulta 2

3. Valor Médio das Diárias por Tipo de Quarto

Situação-problema: a gerência de infraestrutura precisa do valor médio da diária nas internações em acomodações do tipo "Apartamento" ou "UTI".

Recursos: JOIN, LIKE, OR, AVG, ROUND, GROUP BY
Pasta: 03_media_diaria_por_tipo_quarto

Resultado da consulta 3

4. Total de Consultas Realizadas por Médico

Situação-problema: a diretoria de atendimento quer o nome dos médicos e a soma dos valores de suas consultas, só com status "Realizada" ou "Finalizada".

Recursos: JOIN, LIKE, OR, SUM, GROUP BY
Pasta: 04_total_consultas_por_medico

Resultado da consulta 4

5. Soma de Diárias por Paciente com Sobrenome "Silva"

Situação-problema: o setor de cobrança precisa da soma das diárias de internação de todos os pacientes com sobrenome "Silva".

Recursos: JOIN, LIKE, OR, SUM, GROUP BY
Pasta: 05_diarias_pacientes_silva

Resultado da consulta 5

6. Contagem de Prescrições por Status da Consulta

Situação-problema: a farmácia hospitalar quer a quantidade de medicamentos prescritos por status da consulta, só para "Dipirona" ou "Paracetamol".

Recursos: JOIN, LIKE, OR, COUNT, GROUP BY
Pasta: 06_prescricoes_por_status

Resultado da consulta 6

7. Média de Valor de Consultas por Convênio e Especialidade

Situação-problema: em uma negociação contratual, a auditoria quer o valor médio pago por consulta nos convênios que contêm "Unimed", cruzando com a especialidade do médico.

Recursos: JOIN em 3 tabelas, LIKE, AVG, ROUND, GROUP BY
Pasta: 07_media_por_convenio_especialidade

Resultado da consulta 7

8. Média de Diária de Internação por Convênio em Quartos de UTI

Situação-problema: a regulação hospitalar precisa do valor médio das diárias por convênio, só em quartos de "UTI".

Recursos: JOIN em 3 tabelas, LIKE, AVG, ROUND, GROUP BY
Pasta: 08_media_diaria_uti_por_convenio

Resultado da consulta 8

9. Medicamentos Prescritos por Médico em Consultas Realizadas

Situação-problema: a comissão de farmácia e terapêutica quer o total de itens prescritos por médico nas consultas marcadas como "Realizada".

Recursos: JOIN em 3 tabelas, LIKE, COUNT, GROUP BY
Pasta: 09_itens_prescritos_por_medico

Resultado da consulta 9

10. Valor Total e Média de Consultas por Paciente

Situação-problema: o atendimento ao cliente precisa de um extrato com a soma e a média dos valores gastos em consultas por pacientes cujo nome começa com a letra "M".

Recursos: JOIN, LIKE, SUM, AVG, ROUND, GROUP BY
Pasta: 10_total_e_media_pacientes_M

Resultado da consulta 10

👨‍💻 Autor

Projeto acadêmico de Banco de Dados.

Autor: Lucas Vieira Santos

📚 Objetivo Educacional

Praticar os conceitos de bancos relacionais: criação de tabelas, chaves primárias e estrangeiras, relacionamentos, inserção de dados e consultas SQL com JOIN, LIKE e funções de agregação.

Os dados usados são fictícios e servem apenas para testes e demonstrações.
