## Preparando o Ambiente:

- Faça a instalação do SKD do .NET Core (acesse dot.net)

### Extensões Recomendadas:

- C#
- "SQL Server (mssql)" (Caso seja este o BD escolhido)


## Banco de Dados

### Banco Local:

- Usando o SQL Server:
    - Instala-se a extensao SQL Server (mssql)
    - Acesse a aba lateral SQL Server (ou Ctrl + Alt + D) 
    - Add Connection
    - (localdb)\mssqllocaldb
    - Insira um nome para o banco de dados (opcional)
    - Escolha o tipo de autentificacao (Sugestão: Integrated)
    - Escolha o nome que será exibido no menu lateral

## Migrations

### Comando para Aplicar Migrações:

dotnet ef database update -p (caminho do projeto) ("-v" é chamado para utilizar o Verbose, para detalhar as ações no Console). 

Exemplo:

    dotnet ef database update -p .\Cursos\CursoEFCore.csproj -v