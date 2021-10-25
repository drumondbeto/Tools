## Preparando o Ambiente:

### Usando o VS Code:

- Faça a instalação do SDK do .NET Core (acesse dot.net).
- Faça a instalação do Banco de Dados desejado (Instruções abaixo).

## Criando uma aplicação console pelo CLI do .NET Core:

- Crie uma solition:
    - No prompt de comando, insira:
            
            dotnet new sln -n Curso

    No exemplo acima o "-n Curso" está informando que o nome da solução será "Curso".

- Crie um projeto: 
    - Insira:
            
            dotnet new console -n CursoEFCore -o Curso -f netcoreapp3.1

    No exemplo acima o "-o Curso" indica o diretorio em que será criado o projeto, caso a pasta "Curso" não exista, ela será criada.

    O "-f netcoreapp3.1" informa a versão do framework.

- Adicione o seu projeto à sua solução:
    - Insira:

            dotnet sln Curso.sln add Curso\CursoEFCore.csproj
    
- Abrindo seu projeto:
    - Para abrir seu projeto no VS Code, digite:

            code .

    - Para abrir seu projeto no Visual Studio, digite:
    
            Curso.sln

## Instalando os pacotes do SqlServer

### VS Code:

- No prompt, insira:

        dotnet add Curso\CursoEFCore.csproj package Microsoft.EntityFrameworkCore.SqlServer --verson 3.1.5

### Visual Studio:

- Abra sua solução no Visual Studio.
- Clique com o botão direito em "Dependencies".
- Selecione "Manage NuGet Packages...".
- Pesquise por "Microsoft.EntityFrameworkCore.SqlServer".
- Instale o pacote.

ou pelo Console do NuGet:

- Vá até a aba Tools
- NuGet Package Manager
- Package Manager Console
- Digite no console:

        install-Package Microsoft.EntityFrameworkCore.SqlServer -version 3.1.5


### Extensões Recomendadas:

- C#
- "SQL Server (mssql)" (Caso seja este o BD escolhido)


## Banco de Dados

### Banco Local:

- Usando o SQL Server:
    - Instala-se a extensao SQL Server (mssql).
    - Acesse a aba lateral SQL Server (ou Ctrl + Alt + D).
    - Add Connection.
    - (localdb)\mssqllocaldb.
    - Insira um nome para o banco de dados (opcional).
    - Escolha o tipo de autentificacao (Sugestão: Integrated).
    - Escolha o nome que será exibido no menu lateral.
    - Acesse https://www.microsoft.com/pt-br/sql-server/sql-server-downloads.
    - Faça o download e execute.
    - Selecione Download Media.
    - Escolha LocalDB.
    - Conclua a instalação e depois intale o arquivo baixado com a extensão .msi

## Estrutura do projeto

### Criando Entidades:

- Cria-se na raíz uma pasta Domain, para criar as Classes, nossas entidades. Exemplo de entidade:

        using System.ComponentModel.DataAnnotations;
        using System.ComponentModel.DataAnnotations.Schema;

        namespace CursoEFCore.Domain
        {
                // Data Annotation para atribuir esta classe a uma tabela no nosso banco de dados:
                //[Table("Cliente")]      
                public class Cliente
                {
                        //[Key]         // Opcional
                        public int Id { get; set; }

                        //[Column]
                        public string Nome { get; set; }

                        //[Column("Phone")]
                        public string Telefone { get; set; }

                        //[Column]
                        public string CEP { get; set; }

                        //[Column]
                        public string Estado { get; set; }

                        //[Column]
                        public string Cidade { get; set; }
                        
                        public string Email { get; set; }
                }
        }

- Cria-se na raíz uma pasta ValueObjects, onde ficará alguns Enums. Exemplo de Enum:

        namespace CursoEFCore.ValueObjects
        {
                public enum StatusPedido
                {
                        Analise,
                        Finalizado,
                        Entregue,
                }
        }

## Migrations

### Comando para Aplicar Migrações:

dotnet ef database update -p (caminho do projeto) ("-v" é chamado para utilizar o Verbose, para detalhar as ações no Console). 

Exemplo:

    dotnet ef database update -p .\Cursos\CursoEFCore.csproj -v