# 📅 DominoPontaDeQuina - Sistema de Gerenciamento de Jogos de Dominó

![C#](https://img.shields.io/badge/C%23-239120?style=for-the-badge&logo=c-sharp&logoColor=white)
![.NET](https://img.shields.io/badge/.NET-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![Entity Framework](https://img.shields.io/badge/Entity%20Framework-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white)
![xUnit](https://img.shields.io/badge/xUnit-5E2B97?style=for-the-badge&logo=xunit&logoColor=white)
![DI](https://img.shields.io/badge/DI-2C8EBB?style=for-the-badge&logo=spring&logoColor=white)

---

## 👤 INTEGRANTE

| Nome | RM |
|------|-----|
| **Isadora Meneghetti** | RM556326 |

---

## 📚 DISCIPLINA

**Entity Framework Core - Acesso a Dados com ORM**

**Professor:** Vinícius Costa Santos

**Instituição:** FACULDADE FIAP

**Ano:** 2026

---

## 📋 SOBRE O PROJETO

Este projeto implementa o modelo de dados do jogo de dominó **"Ponta de Quina"** utilizando **Entity Framework Core** como ORM (Object-Relational Mapper), com uma arquitetura completa em camadas e **Injeção de Dependência (DI)** para promover desacoplamento e testabilidade.

### 🎯 Objetivos do Laboratório

**Laboratório anterior (EF Core):**
- Configurar todas as entidades utilizando **Fluent API**
- Criar **migrations** para evolução do banco de dados
- Implementar **repositories** com consultas LINQ
- Utilizar **Unit of Work** para gerenciar transações

**Laboratório atual (DI):**
- ✅ Criar interfaces para todos os repositories já implementados
- ✅ Criar uma camada de **services** para orquestrar as regras de uso
- ✅ Registrar **DbContext**, **repositories** e **services** no `Program.cs`
- ✅ Alterar a classe de entrada para receber **dependências por construtor**
- ✅ Remover instanciações diretas com `new` das classes de aplicação
- ✅ Testar os fluxos principais mantendo as consultas LINQ nos repositories

---

## 🗄️ ESTRUTURA DO BANCO DE DADOS

### Diagrama de Entidades

```
┌─────────────┐          ┌─────────────┐          ┌─────────────┐
│   Usuario   │ 1      N │   Jogador   │ 1      N │   Partida   │
├─────────────┤──────────├─────────────┤──────────├─────────────┤
│ Id (PK)     │          │ Id (PK)     │          │ Id (PK)     │
│ Nome        │          │ NomeExibicao│          │ IniciadoEm  │
│ Email       │          │ UsuarioId   │          │ FinalizadoEm│
│ HashSenha   │          │ Usuario (FK)│          │ Status      │
│ CriadoEm    │          └─────────────┘          │ PontuacaoAlvo│
└─────────────┘                    ↑              └─────────────┘
                                   │ N                        ↑
                                   │                          │ 1
                                   │                          │
                                   └──────────────────────────┘
                                   │ N             1          │
                                   ▼                          │
                          ┌─────────────────────────────────────┐
                          │    ParticipacaoPartida              │
                          ├─────────────────────────────────────┤
                          │ Id (PK)                             │
                          │ PartidaId (FK)                      │
                          │ JogadorId (FK)                      │
                          │ Posicao                             │
                          │ Pontuacao                           │
                          │ Vencedor                            │
                          └─────────────────────────────────────┘
```

---

## ⚙️ TECNOLOGIAS UTILIZADAS

| Tecnologia | Versão | Descrição |
|------------|--------|-----------|
| **.NET** | 8.0 | Plataforma de desenvolvimento |
| **C#** | 12.0 | Linguagem de programação |
| **Entity Framework Core** | 8.0.15 | ORM para acesso a dados |
| **SQLite** | 8.0.15 | Banco de dados leve embarcado |
| **xUnit** | 2.9.2 | Framework de testes unitários |
| **Moq** | 4.20.72 | Mocking para testes unitários |
| **coverlet.collector** | 6.0.2 | Cobertura de testes |
| **Microsoft.Extensions.Hosting** | 8.0.0 | Host para DI e configuração |
| **Microsoft.EntityFrameworkCore.Design** | 8.0.15 | Ferramentas de design para migrações |
| **Microsoft.EntityFrameworkCore.Tools** | 8.0.15 | Ferramentas CLI para migrações |

---

## 📁 ESTRUTURA DO PROJETO

```
DominoPontaDeQuina/
│
├── DominoPontaDeQuina.Core/                # 🧠 Núcleo do Domínio
│   ├── Enums/
│   │   ├── LadoTabuleiro.cs
│   │   ├── StatusJogada.cs
│   │   ├── StatusPartida.cs
│   │   ├── StatusRodada.cs
│   │   └── TipoFinalizacaoRodada.cs
│   ├── Exceptions/
│   │   ├── DominoException.cs
│   │   ├── JogadaInvalidaException.cs
│   │   ├── PartidaException.cs
│   │   └── RodadaException.cs
│   ├── Interfaces/
│   │   ├── IJogada.cs
│   │   ├── IMaoJogador.cs
│   │   ├── IPartida.cs
│   │   └── IRodada.cs
│   ├── Models/
│   │   ├── Jogada.cs
│   │   ├── Jogador.cs
│   │   ├── MaoJogador.cs
│   │   ├── Partida.cs
│   │   ├── Peca.cs
│   │   ├── Rodada.cs
│   │   ├── Tabuleiro.cs
│   │   └── Time.cs
│   └── Services/
│       ├── DistribuicaoService.cs
│       ├── ITabuleiroService.cs
│       └── TabuleiroService.cs
│
├── DominoPontaDeQuina.Domain/              # 📦 Entidades para Persistência
│   └── Entities/
│       ├── Jogador.cs
│       ├── Partida.cs
│       ├── ParticipacaoPartida.cs
│       ├── StatusPartida.cs
│       └── Usuario.cs
│
├── DominoPontaDeQuina.Repository/          # 🗄️ Camada de Dados
│   ├── Context/
│   │   └── DominoDbContext.cs              # ✅ Fluent API
│   ├── Extensions/
│   │   └── ServiceCollectionExtensions.cs  # ✅ Registro de Repositories
│   ├── Interfaces/                         # ✅ Novas Interfaces
│   │   ├── IJogadorRepository.cs
│   │   ├── IPartidaRepository.cs
│   │   ├── IParticipacaoPartidaRepository.cs
│   │   ├── IRepository.cs
│   │   ├── IUnitOfWork.cs
│   │   └── IUsuarioRepository.cs
│   ├── Repositories/
│   │   ├── BaseRepository.cs
│   │   ├── JogadorRepository.cs
│   │   ├── PartidaRepository.cs
│   │   ├── ParticipacaoPartidaRepository.cs
│   │   └── UsuarioRepository.cs
│   └── UnitOfWork/
│       └── UnitOfWork.cs
│
├── DominoPontaDeQuina.Services/            # ✅ Nova Camada de Services
│   ├── Interfaces/
│   │   ├── IJogadorService.cs
│   │   ├── IPartidaService.cs
│   │   ├── IParticipacaoService.cs
│   │   └── IUsuarioService.cs
│   ├── Implementations/
│   │   ├── JogadorService.cs
│   │   ├── PartidaService.cs
│   │   ├── ParticipacaoService.cs
│   │   └── UsuarioService.cs
│   └── Extensions/
│       └── ServiceCollectionExtensions.cs  # ✅ Registro de Services
│
├── DominoPontaDeQuina.Migrations/          # 🔄 Migrações EF Core
│   ├── DominoDbContextFactory.cs
│   ├── Program.cs                          # ✅ Com DI via IHost
│   ├── domino.db                           # Banco de dados SQLite
│   └── Migrations/
│       └── 20260825121106_InitialCreate.cs
│
└── DominoPontaDeQuina.Tests/               # 🧪 Testes Unitários
    ├── Services/                           # ✅ Testes com DI e Moq
    │   ├── JogadorServiceTests.cs
    │   ├── PartidaServiceTests.cs
    │   ├── ParticipacaoServiceTests.cs
    │   └── UsuarioServiceTests.cs
    ├── Models/
    │   ├── MaoJogadorTests.cs
    │   ├── PartidaTests.cs
    │   ├── PecaTests.cs
    │   └── TabuleiroTests.cs
    ├── JogoTests.cs
    ├── MaoJogadorGapTests.cs
    ├── PartidaFluxoTests.cs
    ├── PartidaGapTests.cs
    ├── RodadaExcecaoTests.cs
    ├── RodadaFinalizacaoGapTests.cs
    ├── RodadaGapTests.cs
    └── TabuleiroGapTests.cs
```

---

## 🏗️ CONFIGURAÇÃO DAS ENTIDADES (Fluent API)

### 1. Usuario

```csharp
private static void ConfigureUsuario(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Usuario>(entity =>
    {
        entity.HasKey(u => u.Id);
        entity.Property(u => u.Nome).IsRequired().HasMaxLength(100);
        entity.Property(u => u.Email).IsRequired().HasMaxLength(150);
        entity.HasIndex(u => u.Email).IsUnique();
        entity.Property(u => u.HashSenha).IsRequired().HasMaxLength(255);
        entity.Property(u => u.CriadoEm).IsRequired().HasDefaultValueSql("CURRENT_TIMESTAMP");
        entity.HasMany(u => u.Jogadores)
            .WithOne(j => j.Usuario)
            .HasForeignKey(j => j.UsuarioId)
            .OnDelete(DeleteBehavior.Restrict);
    });
}
```

### 2. Jogador

```csharp
private static void ConfigureJogador(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Jogador>(entity =>
    {
        entity.HasKey(j => j.Id);
        entity.Property(j => j.NomeExibicao).IsRequired().HasMaxLength(100);
        entity.Property(j => j.UsuarioId).IsRequired();
        entity.HasMany(j => j.Participacoes)
            .WithOne(p => p.Jogador)
            .HasForeignKey(p => p.JogadorId)
            .OnDelete(DeleteBehavior.Cascade);
    });
}
```

### 3. Partida

```csharp
private static void ConfigurePartida(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Partida>(entity =>
    {
        entity.HasKey(p => p.Id);
        entity.Property(p => p.IniciadoEm).IsRequired().HasDefaultValueSql("CURRENT_TIMESTAMP");
        entity.Property(p => p.Status).IsRequired().HasConversion<int>();
        entity.Property(p => p.PontuacaoAlvo).IsRequired().HasDefaultValue(50);
        entity.HasMany(p => p.Participacoes)
            .WithOne(pp => pp.Partida)
            .HasForeignKey(pp => pp.PartidaId)
            .OnDelete(DeleteBehavior.Cascade);
    });
}
```

### 4. ParticipacaoPartida

```csharp
private static void ConfigureParticipacaoPartida(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<ParticipacaoPartida>(entity =>
    {
        entity.HasKey(pp => pp.Id);
        entity.Property(pp => pp.PartidaId).IsRequired();
        entity.Property(pp => pp.JogadorId).IsRequired();
        entity.Property(pp => pp.Posicao).IsRequired();
        entity.Property(pp => pp.Pontuacao).IsRequired().HasDefaultValue(0);
        entity.Property(pp => pp.Vencedor).IsRequired().HasDefaultValue(false);
        entity.HasIndex(pp => new { pp.PartidaId, pp.JogadorId }).IsUnique();
    });
}
```

---

## 🎯 CAMADA DE SERVICES (DI)

### Exemplo de Service com DI

```csharp
public class PartidaService : IPartidaService
{
    private readonly IUnitOfWork _unitOfWork;

    public PartidaService(IUnitOfWork unitOfWork)
    {
        _unitOfWork = unitOfWork;
    }

    public async Task<Partida> CriarPartidaAsync(int pontuacaoAlvo = 50, CancellationToken cancellationToken = default)
    {
        if (pontuacaoAlvo <= 0)
            throw new ArgumentException("Pontuação alvo deve ser maior que 0", nameof(pontuacaoAlvo));

        var partida = new Partida
        {
            PontuacaoAlvo = pontuacaoAlvo,
            Status = StatusPartida.AguardandoJogadores,
            IniciadoEm = DateTime.Now
        };

        await _unitOfWork.Partidas.AddAsync(partida, cancellationToken);
        await _unitOfWork.CompleteAsync(cancellationToken);

        return partida;
    }

    public async Task<bool> IniciarPartidaAsync(Guid partidaId, CancellationToken cancellationToken = default)
    {
        var partida = await _unitOfWork.Partidas.GetByIdAsync(partidaId, cancellationToken);
        if (partida == null)
            return false;

        if (partida.Status != StatusPartida.AguardandoJogadores)
            throw new InvalidOperationException("Partida não pode ser iniciada.");

        var totalParticipantes = await _unitOfWork.ParticipacoesPartidas
            .GetTotalParticipantesAsync(partidaId, cancellationToken);
        
        if (totalParticipantes < 2)
            throw new InvalidOperationException("Partida precisa de pelo menos 2 participantes.");

        partida.Status = StatusPartida.EmAndamento;
        _unitOfWork.Partidas.Update(partida);
        await _unitOfWork.CompleteAsync(cancellationToken);

        return true;
    }
}
```

---

## 📦 REPOSITORIES COM LINQ

### Exemplo de Consultas LINQ

```csharp
// PartidaRepository.cs
public async Task<IEnumerable<Partida>> GetByJogadorAsync(Guid jogadorId, CancellationToken cancellationToken = default)
{
    return await _dbSet
        .Where(p => p.Participacoes.Any(pp => pp.JogadorId == jogadorId))
        .Include(p => p.Participacoes)
            .ThenInclude(pp => pp.Jogador)
        .OrderByDescending(p => p.IniciadoEm)
        .ToListAsync(cancellationToken);
}

public async Task<IEnumerable<Partida>> GetPartidasComPontuacaoAcimaAsync(int pontuacaoMinima, CancellationToken cancellationToken = default)
{
    return await _dbSet
        .Where(p => p.Participacoes.Any(pp => pp.Pontuacao > pontuacaoMinima))
        .Include(p => p.Participacoes)
        .OrderByDescending(p => p.IniciadoEm)
        .ToListAsync(cancellationToken);
}
```

```csharp
// JogadorRepository.cs
public async Task<IEnumerable<Jogador>> GetJogadoresRankingAsync(CancellationToken cancellationToken = default)
{
    return await _dbSet
        .Include(j => j.Participacoes)
        .Select(j => new
        {
            Jogador = j,
            PontuacaoTotal = j.Participacoes.Sum(pp => pp.Pontuacao)
        })
        .OrderByDescending(x => x.PontuacaoTotal)
        .Select(x => x.Jogador)
        .ToListAsync(cancellationToken);
}
```

---

## 🔧 REGISTRO DE SERVIÇOS (DI)

### Repository Extensions

```csharp
public static class ServiceCollectionExtensions
{
    public static IServiceCollection AddRepositoryServices(this IServiceCollection services, string connectionString)
    {
        services.AddDbContext<DominoDbContext>(options =>
            options.UseSqlite(connectionString));

        services.AddScoped<IUnitOfWork, UnitOfWork>();
        services.AddScoped<IUsuarioRepository, UsuarioRepository>();
        services.AddScoped<IJogadorRepository, JogadorRepository>();
        services.AddScoped<IPartidaRepository, PartidaRepository>();
        services.AddScoped<IParticipacaoPartidaRepository, ParticipacaoPartidaRepository>();

        return services;
    }
}
```

### Service Extensions

```csharp
public static class ServiceCollectionExtensions
{
    public static IServiceCollection AddServices(this IServiceCollection services)
    {
        services.AddScoped<IUsuarioService, UsuarioService>();
        services.AddScoped<IJogadorService, JogadorService>();
        services.AddScoped<IPartidaService, PartidaService>();
        services.AddScoped<IParticipacaoService, ParticipacaoService>();

        return services;
    }
}
```

### Program.cs com DI

```csharp
static async Task Main(string[] args)
{
    var host = CreateHostBuilder(args).Build();

    using var scope = host.Services.CreateScope();
    var program = ActivatorUtilities.CreateInstance<Program>(scope.ServiceProvider);
    await program.RunAsync();
}

private static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureServices((context, services) =>
        {
            var connectionString = "Data Source=domino.db";
            services.AddRepositoryServices(connectionString);
            services.AddServices();
            services.AddTransient<Program>();
        });
```

---

## 🧪 TESTES UNITÁRIOS COM MOCKS

```csharp
public class PartidaServiceTests
{
    private readonly Mock<IUnitOfWork> _unitOfWorkMock;
    private readonly IPartidaService _partidaService;

    public PartidaServiceTests()
    {
        _unitOfWorkMock = new Mock<IUnitOfWork>();
        _partidaService = new PartidaService(_unitOfWorkMock.Object);
    }

    [Fact]
    public async Task CriarPartidaAsync_DeveCriarPartidaComSucesso()
    {
        // Arrange
        var pontuacaoAlvo = 50;
        _unitOfWorkMock.Setup(u => u.Partidas.AddAsync(It.IsAny<Partida>(), It.IsAny<CancellationToken>()))
            .Returns(Task.CompletedTask);
        _unitOfWorkMock.Setup(u => u.CompleteAsync(It.IsAny<CancellationToken>()))
            .ReturnsAsync(1);

        // Act
        var partida = await _partidaService.CriarPartidaAsync(pontuacaoAlvo);

        // Assert
        Assert.NotNull(partida);
        Assert.Equal(pontuacaoAlvo, partida.PontuacaoAlvo);
        Assert.Equal(StatusPartida.AguardandoJogadores, partida.Status);
        _unitOfWorkMock.Verify(u => u.Partidas.AddAsync(It.IsAny<Partida>(), It.IsAny<CancellationToken>()), Times.Once);
        _unitOfWorkMock.Verify(u => u.CompleteAsync(It.IsAny<CancellationToken>()), Times.Once);
    }

    [Fact]
    public async Task IniciarPartidaAsync_ComMenosDe2Participantes_DeveLancarExcecao()
    {
        // Arrange
        var partidaId = Guid.NewGuid();
        var partida = new Partida { Id = partidaId, Status = StatusPartida.AguardandoJogadores };

        _unitOfWorkMock.Setup(u => u.Partidas.GetByIdAsync(partidaId, It.IsAny<CancellationToken>()))
            .ReturnsAsync(partida);
        _unitOfWorkMock.Setup(u => u.ParticipacoesPartidas.GetTotalParticipantesAsync(partidaId, It.IsAny<CancellationToken>()))
            .ReturnsAsync(1);

        // Act & Assert
        await Assert.ThrowsAsync<InvalidOperationException>(
            () => _partidaService.IniciarPartidaAsync(partidaId));
    }
}
```

---

## 🔧 COMANDOS DE MIGRAÇÃO

```bash
# Instalar a ferramenta globalmente
dotnet tool install --global dotnet-ef

# Criar a migração inicial
cd DominoPontaDeQuina.Migrations
dotnet ef migrations add InitialCreate --context DominoDbContext --startup-project .

# Aplicar a migração ao banco
dotnet ef database update --context DominoDbContext --startup-project .

# Remover a última migração (não aplicada)
dotnet ef migrations remove --context DominoDbContext --startup-project .

# Gerar script SQL da migração
dotnet ef migrations script --context DominoDbContext --startup-project .

# Listar migrações aplicadas
dotnet ef migrations list --context DominoDbContext --startup-project .
```

---

## 🚀 COMO EXECUTAR

### Pré-requisitos

- .NET SDK 8.0 ou superior
- Git (para clonar o repositório)

### Passos

```bash
# 1. Clonar o repositório
git clone https://github.com/seu-usuario/DominoPontaDeQuina.git
cd DominoPontaDeQuina

# 2. Restaurar pacotes
dotnet restore

# 3. Construir a solução
dotnet build

# 4. Entrar na pasta de migrações
cd DominoPontaDeQuina.Migrations

# 5. Criar a migration
dotnet ef migrations add InitialCreate --context DominoDbContext --startup-project .

# 6. Aplicar a migration
dotnet ef database update --context DominoDbContext --startup-project .

# 7. Executar o programa com DI
dotnet run

# 8. Voltar para a raiz e executar os testes
cd ..
dotnet test

# 9. Executar testes com cobertura
dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=cobertura
```

---

## 📊 SAÍDA ESPERADA

```
=== DOMINO PONTA DE QUINA - LABORATÓRIO DI ===

--- Verificando Banco de Dados ---
✓ Banco existe: True
✓ Migrações pendentes: 0

--- Executando Seed ---
Inserindo dados iniciais...
✓ 3 usuários criados
✓ 4 jogadores criados
✓ Partida criada com ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
✓ 4 participantes adicionados à partida
✓ Partida iniciada!
✅ Seed concluído com sucesso!

--- Dados no Banco ---

📋 Usuários (3):
  - João Silva (joao@email.com)
    * Jogador: Joãozinho (ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx) - Pontuação Total: 25
  - Maria Oliveira (maria@email.com)
    * Jogador: Mariazinha (ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx) - Pontuação Total: 18
  - Pedro Santos (pedro@email.com)
    * Jogador: Pedrinho (ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx) - Pontuação Total: 30

📋 Partidas (1):
  - Partida ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    * Status: EmAndamento
    * Iniciado: 01/09/2026 14:30
    * Pontuação Alvo: 50

📋 Participações (4):
  - Joãozinho (Posição: 1 | Pontos: 25 | Vencedor: False)
  - Mariazinha (Posição: 2 | Pontos: 18 | Vencedor: False)
  - Pedrinho (Posição: 3 | Pontos: 30 | Vencedor: False)
  - Ana (Posição: 4 | Pontos: 22 | Vencedor: False)

✅ Verificação concluída com sucesso!

📋 RESUMO DO LABORATÓRIO DI:
   ✅ Interfaces criadas para todos os repositories
   ✅ Camada de Services implementada
   ✅ Services registrados no ServiceCollectionExtensions
   ✅ Program.cs alterado para receber dependências via construtor
   ✅ Removidas instanciações com new das classes de aplicação
   ✅ Testes atualizados com mocks (Moq)
   ✅ Consultas LINQ mantidas nos repositories
   ✅ Arquitetura em camadas preservada e aprimorada
```

---

## 🧪 RESULTADO DOS TESTES

| Categoria | Testes | Status |
|-----------|--------|--------|
| Básicos | 44 | ✅ Passando |
| Exceção | 8 | ✅ Passando |
| Gap | 26 | ✅ Passando |
| Services (novos) | 12 | ✅ Passando |
| **Total** | **90** | **✅ 100% Passando** |

---

## 📈 APRENDIZADOS

### Primeiro Semestre
1. **Organização de código** - Separação em camadas (Core, Domain, Repository, Migrations)
2. **Entity Framework Core** - ORM para mapeamento objeto-relacional
3. **Fluent API** - Configuração programática de entidades
4. **Migrations** - Versionamento e evolução do esquema do banco
5. **SQLite** - Banco de dados leve embarcado
6. **Relacionamentos** - 1:N e N:1 com EF Core
7. **Repository Pattern** - Encapsulamento da lógica de acesso a dados
8. **Unit of Work** - Gerenciamento de transações e repositórios
9. **LINQ** - Consultas avançadas com Includes, Filters e Aggregations

### Segundo Semestre (Laboratório DI)
10. **Injeção de Dependência (DI)** - Desacoplamento e testabilidade
11. **Inversão de Controle (IoC)** - Container gerencia o ciclo de vida
12. **Interfaces** - Definição de contratos para repositories e services
13. **Services Layer** - Orquestração de regras de negócio
14. **Testes com Mocks** - Uso do Moq para testes unitários isolados
15. **Host Builder** - Configuração centralizada da aplicação
16. **Arquitetura em Camadas** - Core, Domain, Repository, Services, Migrations, Tests

---

## 🔗 LINKS ÚTEIS

- [Documentação EF Core](https://learn.microsoft.com/pt-br/ef/core/)
- [Documentação C#](https://learn.microsoft.com/pt-br/dotnet/csharp/)
- [SQLite](https://www.sqlite.org/index.html)
- [xUnit Documentation](https://xunit.net/)
- [Moq Documentation](https://github.com/devlooped/moq)
- [.NET Download](https://dotnet.microsoft.com/download)
- [Dependency Injection in .NET](https://learn.microsoft.com/pt-br/dotnet/core/extensions/dependency-injection)

---

## 📊 CHANGELOG

| Versão | Data | Alterações |
|--------|------|------------|
| 1.0.0 | 25/08/2026 | Versão inicial com EF Core e Fluent API |
| 2.0.0 | 01/09/2026 | Adicionado DI, Services Layer e testes com Moq |

---

## 📝 LICENÇA

Este projeto foi desenvolvido para fins educacionais na **FIAP - Faculdade de Informática e Administração Paulista**.

---

<p align="center">
  <b>FIAP - Faculdade de Informática e Administração Paulista</b><br>
  Desenvolvido com ❤️ por <b>Isadora Meneghetti</b><br>
  © 2026 - Todos os direitos reservados
</p>