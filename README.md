# Bibliocom – Sistema de Gerenciamento de Biblioteca

API REST desenvolvida com **NestJS, TypeScript, TypeORM e PostgreSQL**, para gerenciamento de obras e exemplares de uma biblioteca.

## Tecnologias utilizadas

- Node.js
- NestJS
- TypeScript
- TypeORM
- PostgreSQL
- Swagger
- class-validator

## Configuração do projeto

### 1. Instalar dependências

```bash
npm install
```

### 2. Configurar o banco de dados

Configure a variável `DATABASE_URL` no arquivo `.env` com os dados de conexão do PostgreSQL.

### 3. Executar a aplicação

```bash
# Desenvolvimento
npm run start:dev

# Produção
npm run start:prod
```

## Documentação da API

Após iniciar a aplicação, acesse:

http://localhost:3000/api/docs

A documentação OpenAPI também está disponível no arquivo `openapi.json`.

## Recursos da API

A API possui operações CRUD para:

- **Obras:** cadastro, consulta, atualização e remoção de obras.
- **Exemplares:** cadastro, consulta, atualização e remoção de exemplares.

Cada recurso possui cinco rotas REST.

## Exemplos de requisições

Os exemplos consideram a aplicação em execução em `http://localhost:3000`.

### 1. Criar uma obra – HTTP 201

```bash
curl -i -X POST http://localhost:3000/obras \
-H "Content-Type: application/json" \
-d '{"titulo":"Livro de Teste","isbn":"9788522031445","anoPublicacao":1943,"autorId":1}'
```

### 2. Excluir um exemplar – HTTP 204

Substitua `ID_EXISTENTE` pelo identificador de um exemplar cadastrado.

```bash
curl -i -X DELETE http://localhost:3000/exemplares/ID_EXISTENTE
```

### 3. Enviar dados inválidos – HTTP 400

```bash
curl -i -X POST http://localhost:3000/exemplares \
-H "Content-Type: application/json" \
-d '{"tombo":"ex-01","estado":"DISPONIVEL","dataAquisicao":"2025-03-15","obraId":1}'
```

### 4. Consultar recurso inexistente – HTTP 404

```bash
curl -i http://localhost:3000/obras/99999
```

### 5. Cadastrar exemplar com tombo duplicado – HTTP 409

O tombo `EX-TESTE-001` precisa já estar cadastrado no banco.

```bash
curl -i -X POST http://localhost:3000/exemplares \
-H "Content-Type: application/json" \
-d '{"tombo":"EX-TESTE-001","estado":"DISPONIVEL","dataAquisicao":"2025-03-15","obraId":1}'
```

## Validação e tratamento de erros

A API utiliza DTOs e `class-validator` para validar os dados recebidos.

O `ValidationPipe` global está configurado com:

- `whitelist: true`
- `forbidNonWhitelisted: true`
- `transform: true`

Principais códigos HTTP utilizados:

| Código | Significado |
|---|---|
| 200 | Operação realizada com sucesso |
| 201 | Recurso criado |
| 204 | Recurso removido |
| 400 | Dados inválidos |
| 404 | Recurso não encontrado |
| 409 | Conflito de dados |

## Exercícios da atividade

### E07.2 – Códigos de status HTTP

Foram utilizados códigos HTTP adequados para representar os resultados das operações, como 201 para criação, 204 para exclusão, 400 para dados inválidos, 404 para recursos inexistentes e 409 para conflitos.

### E07.3 – DTO de Exemplar

O DTO de criação de exemplar possui validações para os campos tombo, estado, data de aquisição e obra associada, garantindo que os dados recebidos estejam de acordo com as regras definidas.

### E07.7 – Limite de paginação

O limite máximo de registros por página foi removido temporariamente para testar uma requisição com `tamanho=1000`. Sem a validação, a API aceitou a solicitação e retornou os registros disponíveis.

Após restaurar o `@Max(100)`, a mesma requisição passou a retornar HTTP 400, informando que o tamanho máximo permitido é 100.

**Conclusão:** o limite de paginação ajuda a evitar consultas excessivamente grandes, reduzindo o consumo de memória e processamento e protegendo a aplicação contra sobrecarga.

### E07.8 – Documentação OpenAPI e integração contínua

O arquivo `openapi.json` foi gerado a partir da documentação Swagger e incluído no repositório.

Também foi criado um workflow com GitHub Actions (`.github/workflows/ci.yml`) para automatizar a validação da aplicação. O processo instala as dependências, executa os testes, compila o projeto, inicia a API e gera novamente a documentação OpenAPI.

Por fim, o arquivo gerado é comparado com a versão existente no repositório. Caso exista alguma divergência, a verificação falha, indicando que a documentação precisa ser atualizada.

**Objetivo:** manter a documentação da API sincronizada com sua implementação.

## Testes

```bash
npm run test
npm run test:e2e
```

## Licença

Projeto acadêmico desenvolvido para fins educacionais.
