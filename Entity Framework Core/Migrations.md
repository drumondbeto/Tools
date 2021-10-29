# Migrations

## Requisitos:

- Microsoft.EntityFrameworkCore.Design
- Microsoft.EntityFrameworkCore.Tools

## Criando a sua primeira Migração:

- Vá até a Terminar de sua preferencia e digite:

        dotnet ef migrations add PrimeiraMigracao -p (caminho do projeto)

## Comando para Aplicar Migrações:

dotnet ef database update -p (caminho do projeto) ("-v" é chamado para utilizar o Verbose, para detalhar as ações no Console). 

Exemplo:

    dotnet ef database update -p .\Cursos\CursoEFCore.csproj -v
