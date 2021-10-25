# DbContext

## Atribuições:
- Configurar seu modelo de dados;
- Gerenciar a conexão com o banco de dados;
- Consultar e persistir dados em seu banco;
- Fazer toda a rastreabilidade de objetos;
- Matearilizar resultados das consultas;
- Cache de primeiro nível.

## Principais Métodos:

### OnConfiguring

- É usado para informar qual o provider será utilizado e informar  a string de conexão, logger, serviços customizados e outros.

### OnModelCreating

- É usado para confgurar todo o modelo de dados que serão posteriormente transformados em tabelas e comandos SQL.

### SaveChanges

- Método responsável por coletar os dados que sofreram alterações e persistir no banco de dados.