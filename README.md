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

## Como utilizar o código - Passo a Passo 💡

### Passo 1 - PostgreSQL - Criando a tabela que irá receber os dados

```sql
-- Criando a tabela
create table tabela_crud (
    id    serial primary key,
    name  varchar(100),
    email varchar(100)
);

-- Consultando a tabela
select *
  from tabela_crud
 where 1 = 1;
```

### Passo 2 - Python - Instalando as bibliotecas necessárias

Certifique-se de ter o Python instalado em seu sistema. Sugiro a plataforma do [Anaconda](https://www.anaconda.com/).

Instale a biblioteca `Flask` para criar a aplicação web:
```
pip install flask
```

Instale a biblioteca `psycopg2` para se conectar ao banco de dados PostgreSQL:
```
pip install psycopg2
```

### Passo 3 - Python - Criando a aplicação ("backend")

Crie um arquivo Python chamado `app.py` e adicione o seguinte código:

```python
from flask import Flask, render_template, request, redirect
import psycopg2

app = Flask(__name__, template_folder='templates')

# Configurações do banco de dados
db_config = {
    'host': 'localhost',
    'port': '5432',
    'database': 'seu_banco_de_dados',
    'user': 'seu_usuario',
    'password': 'sua_senha'
}

# Conexão com o banco de dados
conn = psycopg2.connect(**db_config)
cursor = conn.cursor()

# Rota para exibir os registros da tabela
@app.route('/')
def index():
    # Consulta os registros da tabela
    cursor.execute("SELECT * FROM tabela_crud")
    records = cursor.fetchall()
    return render_template('index.html', records=records)

# Rota para adicionar um novo registro
@app.route('/add', methods=['POST'])
def add():
    if request.method == 'POST':
        name = request.form['name']
        email = request.form['email']
        
        # Insere um novo registro na tabela
        cursor.execute("INSERT INTO tabela_crud (name, email) VALUES (%s, %s)", (name, email))
        conn.commit()
        
    return redirect('/')

# Rota para editar um registro existente
@app.route('/edit/<int:id>', methods=['GET', 'POST'])
def edit(id):
    cursor.execute("SELECT * FROM tabela_crud WHERE id = %s", (id,))
    record = cursor.fetchone()

    if request.method == 'POST':
        name = request.form['name']
        email = request.form['email']
        
        # Atualiza o registro na tabela
        cursor.execute("UPDATE tabela_crud SET name = %s, email = %s WHERE id = %s", (name, email, id))
        conn.commit()
        
        return redirect('/')
    
    return render_template('edit.html', record=record)

# Rota para excluir um registro
@app.route('/delete/<int:id>')
def delete(id):
    # Exclui o registro da tabela
    cursor.execute("DELETE FROM tabela_crud WHERE id = %s", (id,))
    conn.commit()
    
    return redirect('/')

if __name__ == '__main__':
    app.run(debug=True)
```

### Passo 4 - Crie uma pasta Templates e os arquivos HTML ("frontend")
Crie no mesmo local do arquivo `app.py`, uma pasta chamada: `templates` e adicione nela dois arquivos HTML. 

Nomeie o primeiro arquivo como `index.html` e adicione o seguinte código:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Integrando o Python ao PostgreSQL</title>
</head>
<body>
    <h1>Integrando o Python ao PostgreSQL</h1>

    <form action="/add" method="POST">
        <label for="name">Nome:</label>
        <input type="text" id="name" name="name" required>
        <br><br>
        <label for="email">Email:</label>
        <input type="email" id="email" name="email" required>
        <br><br>
        <button type="submit">Adicionar</button>
    </form>

    <br>

    <table border="1">
        <tr>
            <th>ID</th>
            <th>Nome</th>
            <th>Email</th>
            <th>Ações</th>
        </tr>
        {% for record in records %}
        <tr>
            <td>{{ record[0] }}</td>
            <td>{{ record[1] }}</td>
            <td>{{ record[2] }}</td>
            <td>
                <a href="/edit/{{ record[0] }}">Editar</a>
                <a href="/delete/{{ record[0] }}">Excluir</a>
            </td>
        </tr>
        {% endfor %}
    </table>
</body>
</html>
```

Crie outro arquivo HTML (na pasta `\templates`) chamado `edit.html` e adicione o seguinte código:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Editar Registro</title>
</head>
<body>
    <h1>Editar Registro</h1>

    <form action="/edit/{{ record[0] }}" method="POST">
        <label for="name">Nome:</label>
        <input type="text" id="name" name="name" value="{{ record[1] }}" required>
        <br><br>
        <label for="email">Email:</label>
        <input type="email" id="email" name="email" value="{{ record[2] }}" required>
        <br><br>
        <button type="submit">Salvar</button>
    </form>
</body>
</html>
```

### Passo 5 - Verificaçoes
Certifique-se de substituir `'seu_banco_de_dados'`, `'seu_usuario'` e `'sua_senha'` pelas informações corretas do seu banco de dados PostgreSQL.

##$ Passo 6 - Executando

Execute o aplicativo Flask digitando o seguinte comando no terminal:

```
python app.py
```

Abra um navegador e acesse `http://localhost:5000` para ver a tabela de CRUD em ação. Você poderá adicionar registros, editar registros existentes e excluí-los.

## Conclusão
Com esses passos, você poderá aprender como uma aplicação Python lança dados dentro de um banco de dados PostgreSQL e como visualizar e manipular esses dados por meio de uma interface web interativa. É um excelente ponto de partida para explorar o desenvolvimento de aplicativos web com Python e bancos de dados PostgreSQL! 🚀📊

Lembre-se de adaptar o código conforme necessário para atender às suas necessidades específicas e explorar outras funcionalidades disponíveis nessas tecnologias. Aproveite e divirta-se criando suas próprias aplicações! 😊💻

Bons estudos
