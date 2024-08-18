# IAMarcos2024-IA22-2TRI
# Iniciando um projeto Node.js com TypeScript


Olá caro amigo ou amiga tudo certo espero que sim vamos criar um projeto com TyperScript caso vc não saiba oq é TypeScript leia abaixo 

TypeScript : TypeScript é uma linguagem desenvolvida pela Microsoft que estende o JavaScript adicionando tipagem estática e outras funcionalidades avançadas. Ele permite que você defina tipos para variáveis, funções e objetos, o que ajuda a identificar erros durante o desenvolvimento antes da execução do código.

---Agora vamos direto ao ponto para começar na sua area de trabalho crie uma pasta e de um nome qualquer 

--- acesse-a a (pasta que vc criou) pelo vscode, abra o terminal e siga os passos abaixo.
---Um de cada vez para abrir o terminal só vc clicar Ctrl + "

```bash
npm init -y
npm install express cors sqlite3 sqlite
npm install --save-dev typescript nodemon ts-node @types/express @types/cors
npx tsc --init
mkdir src
touch src/app.ts
```

##  Agora vamos configurar o `tsconfig.json`
Vamos lá 
--- Va até seus arquivos e clique "tsconfig.json" depois disso procure por ("outDir": "./") e altere para
( "outDir": "./dist",) e assim que vc fizer isso abaixo dessa linha que vc acabou de mexer adicione essa linha ( "rootDir ": "./src";) 

--- Seu código vai estar assim igual a baixo

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

--- Agora no mesmo esquema igual foi feito anteriormente nos seus arquivos clique em "package.json"

--- Em seguida na linha ("test": "echo \"Error: no test specified\" && exit 1",

```json

{
  "compilerOptions": {
  
    "target": "es2016",                                 
    "module": "commonjs",                                
  
    "esModuleInterop": true,                           
                       
    "forceConsistentCasingInFileNames": true,           

    "strict": true,                                      
    "skipLibCheck": true                               
  }
}


## Agora vamos Criar o arquivo inicial do servidor

Va até a pasta src e dps até o arquivo app.ts e adicione o seguinte código:

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
  


## Inicializando o servidor

--- Agora antes de iniciarmos instale a Biblioteca "Rest Client" para baixar va até o canto esquerdo da tela e vc vai ver 4 quadrados basta clicar em cima e pesquisar oq falei para baixar acima 

Agora volte até sua pagina e abra o terminal lembre-se para abrir clique (Ctrl + ") em seguida escreva esse comando 

```node
npm run dev
```
--- Se deu tudo certinho vai aparecer essa mensagem `Server running on port 3333` no terminal
```
##Agora vamos testar 

---Espero que esteja tudo ocorrendo certo agora va até o seu navegador e pesquise`http://localhost:3333`, você verá a mensagem `Hello World`se estiver tudo certo.



tchau


 







