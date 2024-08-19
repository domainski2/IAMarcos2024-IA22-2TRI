# IAMarcos2024-IA22-2TRI
# Vamos iniciar um projeto Node.js com TypeScript


Olá caro amigo ou amiga tudo certo espero que sim vamos criar um projeto com TyperScript.

-- Agora vamos direto ao ponto para começar na sua area de trabalho crie uma pasta e de um nome qualquer 

--  acesse-a a (pasta que vc criou) pelo vscode, abra o terminal e siga os passos abaixo.

-- Um de cada vez para abrir o terminal só vc clicar Ctrl + "

```bash
npm init -y
npm install express cors sqlite3 sqlite
npm install --save-dev typescript nodemon ts-node @types/express @types/cors
npx tsc --init
mkdir src
touch src/app.ts
```

## vamos configurar o `tsconfig.json`
Vamos lá 
-- Va até seus arquivos e clique "tsconfig.json" depois disso procure por ("outDir": "./") e altere para
( "outDir": "./dist",) e assim que vc fizer isso abaixo dessa linha que vc acabou de mexer adicione essa linha ( "rootDir ": "./src";) 

-- Seu código vai estar assim igual a baixo

```json
{
{
  "compilerOptions": {
    "target": "ES2017",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  }
}
```
  

## Configurando o `package.json`

--  Agora no mesmo esquema igual foi feito anteriormente nos seus arquivos clique em "package.json"

--- Em seguida na linha ("test": "echo \"Error: no test specified\" && exit 1",) vc vai adicionar uma simples , e na linha de baixo vc vai por esse comando "dev": "npx nodemon src/app.ts";

-- Vai ficar assim

```json

"scripts": {
  "dev": "npx nodemon src/app.ts"
}

```


## Agora vamos Criar o arquivo inicial do servidor

Crie um arquivo src e dentro dele crie outro chamado app.ts e adicione o seguinte código:

```typescript
import express from 'express';
import cors from 'cors';
import { connect } from './database';

const port = 3333;
const app = express();

app.use(cors());
app.use(express.json());


app.get('/', (req, res) => {
  res.send('Olá Mundo');
});

app.listen(port, () => {
    console.log(`Server running on port ${port}`);
  });
  


app.post('/users', async (req, res) => {
  const db = await connect();
  const { name, email } = req.body;

  const result = await db.run('INSERT INTO users (name, email) VALUES (?, ?)', [name, email]);
  const user = await db.get('SELECT * FROM users WHERE id = ?', [result.lastID]);

  res.json(user);
});


app.get('/users', async (req, res) => {
  const db = await connect();
  const users = await db.all('SELECT * FROM users');

  res.json(users);
});


app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});

app.put('/users/:id', async (req, res) => {
    const db = await connect();
    const { name, email } = req.body;
    const { id } = req.params;
  
    await db.run('UPDATE users SET name = ?, email = ? WHERE id = ?', [name, email, id]);
    const user = await db.get('SELECT * FROM users WHERE id = ?', [id]);
  
    res.json(user);
  });
  
  app.delete('/users/:id', async (req, res) => {
    const db = await connect();
    const { id } = req.params;
  
    await db.run('DELETE FROM users WHERE id = ?', [id]);
  
    res.json({ message: 'Usuário excluído' });
  });
  
```

## Inicializando o servidor

-- Agora antes de iniciarmos instale a Biblioteca "Rest Client" para baixar va até o canto esquerdo da tela e vc vai ver 4 quadrados basta clicar em cima e pesquisar "Rest Client" e dps clicar em dowload.

Agora volte até sua pagina e abra o terminal lembre-se para abrir clique (Ctrl + ") em seguida escreva esse comando 

```node
npm run dev
```
-- Se deu tudo certinho vai aparecer essa mensagem `Server running on port 3333` no terminal

## Agora vamos testar

---Espero que esteja tudo ocorrendo certo agora va até o seu navegador e pesquise`http://localhost:3333`, você verá a mensagem `Hello World`se estiver tudo certo.

```typescript
http://localhost:3333
```

## Configurando o banco de dados

Crie mais um arquivo no src, chamado database.ts e dentro dele adicione o seguinte código

```typescript
import { Database, open } from 'sqlite';
import sqlite3 from 'sqlite3';

let instance: any | null = null;

export async function connect() {
  if (instance) return instance;

  const db = await open({
     filename: './src/database.sqlite',
     driver: sqlite3.Database
   });
  
  await db.exec(`
    CREATE TABLE IF NOT EXISTS users (
      id INTEGER PRIMARY KEY AUTOINCREMENT,
      name TEXT,
      email TEXT
    )
  `);

  instance = db;
  return db;
}
```

## Agora vamos adicionar o banco de dados ao servidor

Acesse o arquivo app.ts e cole o seguinte código:

```typescript
import express from 'express'
import cors from 'cors'
import { connect } from './database'

const port = 3333
const app = express()

app.use(cors())
app.use(express.json())
app.use(express.static(__dirname + '/../public'))

app.get('/users', async (req, res) => {
  const db = await connect()
  const users = await db.all('SELECT * FROM users')
  res.json(users)
})

app.post('/users', async (req, res) => {
  const db = await connect()
  const { name, email } = req.body
  const result = await db.run('INSERT INTO users (name, email) VALUES (?, ?)', [name, email])
  const user = await db.get('SELECT * FROM users WHERE id = ?', [result.lastID])
  res.json(user)
})

```
## Listando os Usuários

--Adicione mais esse código ao seu arquivo "app.ts" 
cole o código abaixo dentro do "app.ts"

```typescript
app.listen(port, () => {
  console.log(`Server running on port ${port}`)
})
```

## Editando um Usuário

--Novamente no seu arquivo "app.ts"
cole o seguinte o código 
```typescript
app.put('/users/:id', async (req, res) => {
  const db = await connect()
  const { name, email } = req.body
  const { id } = req.params
  await db.run('UPDATE users SET name = ?, email = ? WHERE id = ?', [name, email, id])
  const user = await db.get('SELECT * FROM users WHERE id = ?', [id])
  res.json(user)
}
``` 

## Deletando um Usuário

Novamente no arquivo "app.ts"
cole o seguinte código

```typescript
app.delete('/users/:id', async (req, res) => {
  const db = await connect()
  const { id } = req.params
  await db.run('DELETE FROM users WHERE id = ?', [id])
  res.json({ message: 'User deleted' })
})

```

## Agora finalizando ja

--Crie uma pasta chamada public e nela crie o arquivo index.html nesse arquivo cole o seguinte código:

```typescript

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <form>
    <input type="text" name="name" placeholder="Nome">
    <input type="email" name="email" placeholder="Email">
    <button type="submit">Cadastrar</button>
  </form>

  <table>
    <thead>
      <tr>
        <th>Id</th>
        <th>Name</th>
        <th>Email</th>
        <th>Ações</th>
      </tr>
    </thead>
    <tbody>
      <!--  -->
    </tbody>
  </table>

  <script>
    // 
    const form = document.querySelector('form')

    form.addEventListener('submit', async (event) => {
      event.preventDefault()

      const name = form.name.value
      const email = form.email.value

      await fetch('/users', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ name, email })
      })

      form.reset()
      fetchData()
    })

    // 
    
    const tbody = document.querySelector('tbody')

    async function fetchData() {
      const resp = await fetch('/users')
      const data = await resp.json()

      tbody.innerHTML = ''

      data.forEach(user => {
        const tr = document.createElement('tr')
        tr.innerHTML = `
          <td>${user.id}</td>
          <td>${user.name}</td>
          <td>${user.email}</td>
          <td>
            <button class="excluir">excluir</button>
            <button class="editar">editar</button>
          </td>
        `

        const btExcluir = tr.querySelector('button.excluir')
        const btEditar = tr.querySelector('button.editar')

        btExcluir.addEventListener('click', async () => {
          await fetch(`/users/${user.id}`, { method: 'DELETE' })
          tr.remove()
        })

        btEditar.addEventListener('click', async () => {
          const name = prompt('Novo nome:', user.name)
          const email = prompt('Novo email:', user.email)

          await fetch(`/users/${user.id}`, {
            method: 'PUT',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ name, email })
          })

          fetchData()
        })

        tbody.appendChild(tr)
      })
    }

    fetchData()u
  </script>
</body>

</html>



```
## Agora vamos testar a inserção de dados

--Agora por ultimo dentro da pasta "src" crie o arquivo "ts.http" e depois cole o código abaixo

```typescript
POST http://localhost:3333/users
Content-Type: application/json

{
  "name": "John Doe",
  "email" : "BomDia"
}
```
--## Ultima coisa agora 

Abra seu navegador, vá na barra de pesquisa e digite http://localhost:3333/users ;

```typescript
http://localhost:3333/users
```

## Meus parabéns vc conseguiu concluir o tutorial





