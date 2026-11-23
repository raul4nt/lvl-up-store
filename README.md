# Lvl Up Store

Loja virtual de jogos com vitrine, carrinho e painel administrativo, feita em ASP.NET Core MVC (.NET 6) com Identity e Entity Framework Core sobre SQL Server.

## Contexto

Projeto acadêmico do curso de Análise e Desenvolvimento de Sistemas da Faculdade Senac Porto Alegre (FSPOA), desenvolvido em dupla entre novembro e dezembro de 2024 por Raul Silveira ([@raul4nt](https://github.com/raul4nt)) e Alexandre Costa ([@alexandre-costa22](https://github.com/alexandre-costa22)).

## Funcionalidades

**Loja (área pública)**
- Vitrine com carrossel dos jogos cadastrados e busca por título.
- Página de detalhes do jogo, com desenvolvedora, publicadora, franquia, plataforma, classificação indicativa e modo multijogador.
- Carrinho por usuário logado: adicionar, remover e alterar quantidade.
- Páginas Sobre nós, Contato e Política de privacidade, e página 404 personalizada.

**Contas (ASP.NET Core Identity)**
- Cadastro, login e gerenciamento de conta com as páginas do Identity traduzidas para o português.
- Foto de perfil enviada pelo usuário e exibida no cabeçalho.
- Papel `Admin` criado automaticamente na subida, com um usuário administrador de desenvolvimento (ver `Program.cs`).

**Painel administrativo**
- CRUD de jogos (com upload da capa), desenvolvedoras, publicadoras, franquias, plataformas (softwares), clientes, vendedores e avaliações, além do cadastro de notas fiscais (CPF, cliente, vendedor e valor).
- Página de acesso negado própria para quem não tem permissão.

## Stack

- C# e ASP.NET Core MVC 6 com Razor Views
- ASP.NET Core Identity 6 (contas, papéis e Razor Pages de autenticação)
- Entity Framework Core 6 com SQL Server (LocalDB no desenvolvimento) e migrations
- Bootstrap e jQuery Validation nas views

## Como rodar

Pré-requisitos: .NET 6 SDK e SQL Server LocalDB (vem com o Visual Studio) ou outra instância do SQL Server.

```bash
dotnet tool install --global dotnet-ef   # se ainda não tiver
dotnet restore
dotnet ef database update                # aplica as migrations
dotnet run
```

A string de conexão fica em `appsettings.json` (`ConnectionStrings:LvlUpContext`). Também dá para abrir `LvlUp.sln` no Visual Studio e rodar por lá.

## Estrutura

```
Controllers/            Store e Cart (loja), CRUDs do painel, Account e Error
Models/                 Game, Developer, Publisher, Franchise, Software, Client, Seller,
                        Review, Receipt, Cart, CartItem e ApplicationUser
Enums/                  DeveloperType, Gender e Rating
Data/LvlUpContext.cs    contexto do EF Core (Identity + entidades da loja)
Migrations/             histórico do banco
Views/                  views Razor, com layouts separados para a loja e para o painel
Areas/Identity/         páginas do Identity personalizadas
wwwroot/                CSS, imagens e capas enviadas
```

## Destaques técnicos

- Autorização por papel com `[Authorize(Roles = "Admin")]` nos controllers de cadastro do painel, separando a loja pública da área administrativa.
- `ApplicationUser` estende o `IdentityUser` com nome completo, telefone e foto de perfil, e o mesmo `DbContext` guarda contas e dados da loja.
- Upload de imagens com `IFormFile`: as capas dos jogos vão para `wwwroot/images/games` com nome de arquivo aleatório (`Path.GetRandomFileName`), evitando colisão entre envios.
- Modelagem relacional com EF Core: jogo ligado a desenvolvedora, publicadora, franquia e plataforma, carrinho com itens, e validação por Data Annotations com mensagens em português.
