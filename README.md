# Criando uma pequena aplicação Python integrada com o PostgreSQL 🐍🐘

## Motivação 😊

A motivação por trás deste código é fornecer um exemplo prático de como criar uma aplicação web simples que integra Python com um banco de dados PostgreSQL. Muitas vezes, as pessoas têm dúvidas sobre como criar uma aplicação que possa interagir com um banco de dados, realizar operações de criação, leitura, atualização e exclusão (CRUD), e exibir os dados em um formato amigável. 🌐💻

Este código foi desenvolvido com o objetivo de atender a essa demanda e fornecer uma base sólida para aprender e explorar o desenvolvimento de aplicativos Python integrados ao PostgreSQL. Com ele, é possível compreender como estabelecer a conexão com o banco de dados, executar operações CRUD e apresentar os resultados em uma interface web agradável. 🚀🔍

## Componentes principais 🧩

O código é composto pelos seguintes elementos:

1. **Flask e psycopg2:** Foram utilizadas as bibliotecas Flask e psycopg2 para criar a aplicação web em Python e estabelecer a conexão com o banco de dados PostgreSQL, respectivamente. O Flask é um framework web leve e flexível, enquanto o psycopg2 é um adaptador de banco de dados PostgreSQL para Python. 💻🐘

2. **Criação da tabela:** O código inclui um script SQL que cria uma tabela chamada "tabela_crud" no banco de dados PostgreSQL. Essa tabela possui três colunas: "id", "name" e "email". A estrutura da tabela é fundamental para armazenar e organizar os registros inseridos pela aplicação. 🔧📋

3. **Rotas da aplicação:** A aplicação define três rotas principais:
   - Rota raiz `'/'`: Essa rota exibe os registros existentes na tabela em um formato de tabela HTML. Também apresenta um formulário para adicionar novos registros.
   - Rota `'/add'`: Essa rota recebe os dados do formulário de adição de registro e insere um novo registro na tabela do banco de dados.
   - Rota `'/edit/<int:id>'`: Essa rota permite editar um registro existente. Ela exibe um formulário preenchido com os detalhes do registro selecionado e atualiza os dados do registro no banco de dados após a edição. 📄✏️

4. **Templates HTML:** A aplicação utiliza dois arquivos HTML localizados na pasta "templates" para renderizar as páginas da aplicação. O arquivo "index.html" exibe a tabela de registros e o formulário de adição, enquanto o arquivo "edit.html" apresenta o formulário preenchido para a edição de um registro específico. Esses templates garantem uma interface amigável para o usuário. 🖥️

## Como utilizar o código 💡

Para utilizar o código:

1. Certifique-se de ter o Python instalado em seu sistema.
2. Instale as bibliotecas Flask e psycopg2 utilizando o gerenciador de pacotes `pip`.
3. Configure corretamente as informações de conexão com o seu banco de dados PostgreSQL no código.
4. Execute o arquivo `app.py` para iniciar a aplicação Flask.
5. Abra um navegador e acesse `http://localhost:5000` para ver a aplicação em funcionamento

.

Com esses passos, você poderá aprender como uma aplicação Python lança dados dentro de um banco de dados PostgreSQL e como visualizar e manipular esses dados por meio de uma interface web interativa. É um excelente ponto de partida para explorar o desenvolvimento de aplicativos web com Python e bancos de dados PostgreSQL! 🚀📊

Lembre-se de adaptar o código conforme necessário para atender às suas necessidades específicas e explorar outras funcionalidades disponíveis nessas tecnologias. Aproveite e divirta-se criando suas próprias aplicações! 😊💻
