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
    - (localdb)\mssqllocaldb
    - Insira um nome para o banco de dados (opcional).
    - Escolha o tipo de autentificacao (Sugestão: Integrated).
    - Escolha o nome que será exibido no menu lateral.
    - Acesse https://www.microsoft.com/pt-br/sql-server/sql-server-downloads.
    - Faça o download e execute.
    - Selecione Download Media.
    - Escolha LocalDB.
    - Conclua a instalação e depois intale o arquivo baixado com a extensão .msi

## Estruturando o projeto

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

### Criando a classe de contexto:

- Cria-se na raíz uma pasta Data;
- Dentro desta pasta é criado um arquivo ApplicationContext.cs, onde será configurado o DbContext.
- Cria-se no arquivo a classe ApplicationContext herdada da classe DbContext.

#### Configurando a seção entre a aplicação e seu Banco de Dados

- Exemplo usando um banco local com SqlServer:

        using Microsoft.EntityFrameworkCore;

        namespace CursoEFCore.Data
        {
                // CLASSE RESPONSAVEL POR ESTABELECER UMA SESSÃO ENTRE A APLICAÇÃO E SEU BANCO DE DADOS
                protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
                {   
                        optionsBuilder.UseSqlServer("Data source=(localdb)\\mssqllocaldb;Initial Catalog=CursoEFCore;Integrated Security=true");
                }

- Outras strings de conexão:

        https://www.connectionstrings.com/

#### Mapeamento do Modelo de Dados

- Criamos as classes que representarão as nossas entidades, na pasta Domain;
- Criamos a classe ApplicationContext;

- MODO 1, Empondo explicitamente sua entidade através de uma propriedade genérica DbSet no contexto da classe ApplicationContext:

        using CursoEFCore.Domain;
        using Microsoft.EntityFrameworkCore;

        namespace CursoEFCore.Data
        {
                public class ApplicationContext : DbContext
                {
                        public DbSet<Pedido> Pedidos { get; set; }
                }
        }

Inclui o mapeamento das classes declaradas nas propriedades das entidades. (Classes dependentes ou propriedades de navegação)


- MODO 2, Expecificar no método OnModelCreating utilizando o Fluent API:

        using CursoEFCore.Domain;
        using Microsoft.EntityFrameworkCore;

        namespace CursoEFCore.Data
        {
                public class ApplicationContext : DbContext
                {
                        protected override void OnModelCreating(ModelBuilder modelBuilder)
                        {
                                modelBuilder.Entity<Cliente>(p =>
                                {
                                        p.ToTable("Clientes");
                                        // Adicionando colunas às tabelas:
                                        p.HasKey(p => p.Id);
                                        p.Property(p => p.Nome).HasColumnType("VARCHAR(80)").IsRequired();
                                        p.Property(p => p.Telefone).HasColumnType("CHAR(11)");
                                        p.Property(p => p.CEP).HasColumnType("CHAR(8)").IsRequired();
                                        p.Property(p => p.Estado).HasColumnType("CHAR(2)").IsRequired();
                                        p.Property(p => p.Cidade).HasMaxLength(60).IsRequired();
                                        // Gera uma variavel automatica para uma string com no max 60 caracteres.
                                        p.HasIndex(i => i.Telefone).HasDatabaseName("idx_cliente_telefone");
                                });

                                modelBuilder.Entity<Produto>(p =>
                                {
                                        p.ToTable("Produtos");
                                        p.HasKey(p => p.Id);
                                        p.Property(p => p.CodigoBarras).HasColumnType("VARCHAR(14)").IsRequired();
                                        p.Property(p => p.Descricao).HasColumnType("VARCHAR(60)");
                                        p.Property(p => p.Valor).IsRequired();
                                        p.Property(p => p.TipoProduto).HasConversion<string>();
                                        // Define como o Enum TipoProduto sera armazenado no banco de dados.
                                });

                                modelBuilder.Entity<Pedido>(p=>
                                {
                                        p.ToTable("Pedidos");
                                        p.HasKey(p => p.Id);
                                        p.Property(p => p.IniciadoEm).HasDefaultValueSql("GETDATE()").ValueGeneratedOnAdd();
                                        p.Property(p => p.Status).HasConversion<string>();
                                        p.Property(p => p.TipoFrete).HasConversion<int>();
                                        p.Property(p => p.Observacao).HasColumnType("VARCHAR(512)");

                                        // Configura o "Muitos para Um":
                                        p.HasMany(p => p.Itens)
                                                .WithOne(p => p.Pedido)
                                                .OnDelete(DeleteBehavior.Cascade);
                                                // Faz com que os itens do pedido sejam deletados em cascata ao apagar um pedido.

                                                // .OnDelete(DeleteBehavior.Restrict); 
                                                // Poderia ser utilizado para obrigar o usuario a apagar os itens primeiro para poder apagar o pedido.
                                });

                                modelBuilder.Entity<PedidoItem>(p =>
                                {
                                        p.ToTable("PedidoItens");
                                        p.HasKey(p => p.Id);
                                        p.Property(p => p.Quantidade).HasDefaultValue(1).IsRequired();
                                        // Quando eu instanciar um Item de Pedido e não informar a quantidade, o valor será 1 por padrão.
                                        p.Property(p => p.Valor).IsRequired();
                                        p.Property(p => p.Desconto).IsRequired();
                                });
                        }
                }
        }


- Modo 3 (Mais escalável):
    - Criamos dentro da pasta Data uma pasta chamada Configurations;
    - Dentro da pasta Configurations criamos um arquivo para cada entidade que desejamos criar em nossa base de dados, exemplo:

                using CursoEFCore.Domain;
                using Microsoft.EntityFrameworkCore;

                namespace CursoEFCore.Data.Configurations
                {
                        public class ClienteConfigurations : IEntityTypeConfiguration<Cliente>
                        {
                                public void Configure(Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder<Cliente> builder)
                                {
                                // CRIANDO ENTIDADE APARTIR DA CLASSE EXISTENTE
                                builder.ToTable("Clientes");
                                // Adicionando colunas às tabelas:
                                builder.HasKey(p => p.Id);
                                builder.Property(p => p.Nome).HasColumnType("VARCHAR(80)").IsRequired();
                                builder.Property(p => p.Telefone).HasColumnType("CHAR(11)");
                                builder.Property(p => p.CEP).HasColumnType("CHAR(8)").IsRequired();
                                builder.Property(p => p.Estado).HasColumnType("CHAR(2)").IsRequired();
                                builder.Property(p => p.Cidade).HasMaxLength(60).IsRequired();

                                builder.HasIndex(i => i.Telefone).HasDatabaseName("idx_cliente_telefone");
                                }
                        }
                }

    - Em seguida, dentro do método OnModelCreating em nosso ApplicationContext, na pasta Data, fazemos com que sejam encontradas todas as entidades mapeadas pelo Entity:

                using Microsoft.EntityFrameworkCore;

                namespace ASPNET.Data
                {
                        public class ApplicationContext : DbContext
                        {
                                protected override void OnModelCreating(ModelBuilder modelBuilder)
                                {
                                        modelBuilder.ApplyConfigurationsFromAssembly(typeof(ApplicationContext).Assembly);
                                }
                        }
                }

