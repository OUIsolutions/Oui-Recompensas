
# Implementação de Sistema de Tipagem Lua para Bibliotecas OUI Technologies

## Visão Geral

Este documento define as especificações técnicas e o escopo para a implementação de sistemas de tipagem Lua em 10 projetos selecionados da organização OUI Technologies. O objetivo principal é criar documentação de tipos padronizada e compatível com Language Server Protocol (LSP), estabelecendo uma infraestrutura de desenvolvimento mais robusta e produtiva para as bibliotecas Lua da organização.

## Contexto Técnico

O sistema de tipagem Lua utiliza as convenções do LuaLS (Language Server for Lua), que implementa anotações de tipo baseadas em comentários especiais. Esta abordagem permite fornecer informações de tipo sem afetar a execução do código, mantendo a natureza dinâmica do Lua enquanto oferece suporte aprimorado para IDEs e ferramentas de desenvolvimento.

### Estrutura do Sistema de Tipagem

#### 1. Arquivo de Definições (`types.lua`)

O arquivo `types.lua` serve como biblioteca central de definições de tipo para cada projeto, contendo todas as classes, interfaces e assinaturas de função públicas da biblioteca. Este arquivo deve ser posicionado na raiz do projeto para máxima compatibilidade com ferramentas de desenvolvimento.

**Características principais:**
- Compatibilidade total com LuaLS (sumneko.lua Language Server)
- Definições de classes usando anotação `@class`
- Especificação de campos com anotação `@field`
- Documentação de funções com parâmetros e retornos tipados
- Suporte para tipos opcionais e unions

#### 2. Convenções de Anotação

##### Definição de Classes
```lua
---@class NomeClasse: ClassePai
---@field campo_publico string
---@field campo_opcional string | nil
---@field metodo_publico fun(param: string): boolean
```

##### Definição de Funções
```lua
---@param parametro string Descrição do parâmetro
---@param opcional? number Parâmetro opcional
---@return boolean success Indica se a operação foi bem-sucedida
---@return string | nil error_message Mensagem de erro em caso de falha
```

##### Tipos Complexos
```lua
---@alias TipoPersonalizado string | number | boolean
---@type table<string, any> Tabela com chaves string e valores any
```

### Exemplo de Implementação Completa

Com base no projeto [LuaBerrante](https://github.com/SamuelHenriqueDeMoraisVitrio/LuaBerrante), demonstramos a estrutura completa de um arquivo `types.lua`:

```lua
-- Definições de resposta HTTP
---@class RequestResponse
---@field status_code integer Código de status HTTP da resposta
---@field headers table<string, string> Cabeçalhos HTTP da resposta
---@field read_body fun(): string Função para ler o corpo da resposta como string
---@field read_body_json fun(): table Função para ler o corpo da resposta como JSON
---@field read_body_chunk fun(size:integer): string Função para ler o corpo em chunks

-- Configuração de requisições HTTP
---@class RequestProps
---@field url string URL de destino da requisição
---@field method string | nil Método HTTP (GET, POST, etc.). Default: GET
---@field headers table<string, string> | nil Cabeçalhos HTTP personalizados
---@field body string | table | nil Corpo da requisição (string ou tabela para JSON)

-- Informações do bot Telegram
---@class TelegramBotInfo
---@field id string ID único do bot
---@field is_bot boolean Indica se é um bot
---@field first_name string Primeiro nome do bot
---@field username string Username do bot
---@field token string Token de autenticação
---@field id_chat string ID do chat padrão
---@field url string URL base da API do Telegram

-- Resposta padrão da API Telegram
---@class BerranteTelegramResponse
---@field status_code integer Código de status da requisição
---@field body string | nil Corpo da resposta em formato string
---@field json table Corpo da resposta parseado como JSON
---@field in_error boolean Indica se houve erro na requisição
---@field chat_id string ID do chat relacionado à resposta

-- Interface principal do bot Telegram
---@class BerranteTelegramBot
---@field request fun(props: RequestProps): RequestResponse Cliente HTTP interno
---@field infos TelegramBotInfo Informações do bot
---@field getMe fun(): BerranteTelegramResponse Obtém informações do bot
---@field sendMessage fun(body: table, destination: string[]): BerranteTelegramResponse[] Envia mensagem de texto
---@field sendPhoto fun(body: table, destination: string[]): BerranteTelegramResponse[] Envia foto
---@field sendAudio fun(body: table, destination: string[]): BerranteTelegramResponse[] Envia áudio
---@field sendDocument fun(body: table, destination: string[]): BerranteTelegramResponse[] Envia documento
---@field sendVideo fun(body: table, destination: string[]): BerranteTelegramResponse[] Envia vídeo
---@field sendVoice fun(body: table, destination: string[]): BerranteTelegramResponse[] Envia mensagem de voz
---@field sendLocation fun(body: table, destination: string[]): BerranteTelegramResponse[] Envia localização
---@field sendContact fun(body: table, destination: string[]): BerranteTelegramResponse[] Envia contato
---@field forwardMessage fun(body: table, destination: string[]): BerranteTelegramResponse[] Encaminha mensagem
---@field forwardMessages fun(body: table, destination: string[]): BerranteTelegramResponse[] Encaminha múltiplas mensagens
---@field copyMessage fun(body: table, destination: string[]): BerranteTelegramResponse[] Copia mensagem
---@field copyMessages fun(body: table, destination: string[]): BerranteTelegramResponse[] Copia múltiplas mensagens
---@field sendAnimation fun(body: table, destination: string[]): BerranteTelegramResponse[] Envia animação/GIF
---@field sendVideoNote fun(body: table, destination: string[]): BerranteTelegramResponse[] Envia vídeo circular
---@field sendPaidMedia fun(body: table, destination: string[]): BerranteTelegramResponse[] Envia mídia paga
---@field sendMediaGroup fun(body: table, destination: string[]): BerranteTelegramResponse[] Envia grupo de mídias
---@field sendVenue fun(body: table, destination: string[]): BerranteTelegramResponse[] Envia venue/local
---@field sendPoll fun(body: table, destination: string[]): BerranteTelegramResponse[] Envia enquete
---@field sendDice fun(body: table, destination: string[]): BerranteTelegramResponse[] Envia dado animado
---@field sendSticker fun(body: table, destination: string[]): BerranteTelegramResponse[] Envia sticker
---@field editMessageText fun(body: table, destination: string[]): BerranteTelegramResponse[] Edita texto da mensagem
---@field editMessageCaption fun(body: table, destination: string[]): BerranteTelegramResponse[] Edita legenda da mensagem
---@field editMessageMedia fun(body: table, destination: string[]): BerranteTelegramResponse[] Edita mídia da mensagem
---@field editMessageReplyMarkup fun(body: table, destination: string[]): BerranteTelegramResponse[] Edita teclado da mensagem
---@field deleteMessage fun(body: table, destination: string[]): BerranteTelegramResponse[] Deleta mensagem
---@field answerCallbackQuery fun(body: table, destination: string[]): BerranteTelegramResponse[] Responde callback query
---@field answerInlineQuery fun(body: table, destination: string[]): BerranteTelegramResponse[] Responde inline query
---@field getFile fun(body: table, destination: string[]): BerranteTelegramResponse[] Obtém informações de arquivo
---@field getUserProfilePhotos fun(body: table, destination: string[]): BerranteTelegramResponse[] Obtém fotos de perfil
---@field getChat fun(body: table, destination: string[]): BerranteTelegramResponse[] Obtém informações do chat
---@field getChatAdministrators fun(body: table, destination: string[]): BerranteTelegramResponse[] Obtém administradores do chat
---@field getChatMember fun(body: table, destination: string[]): BerranteTelegramResponse[] Obtém informações de membro
---@field getChatMemberCount fun(body: table, destination: string[]): BerranteTelegramResponse[] Obtém contagem de membros
---@field leaveChat fun(body: table, destination: string[]): BerranteTelegramResponse[] Sai do chat
---@field getUpdates fun(body: table, destination: string[]): BerranteTelegramResponse[] Obtém atualizações

-- Configuração de inicialização do bot
---@class LuaBerranteTelegramFlags
---@field token string Token de autenticação do bot
---@field id_chat string ID do chat padrão
---@field url string | nil URL personalizada da API (opcional)

-- Módulo principal da biblioteca
---@class LuaBerranteModule
---@field newTelegramSession fun(flags: LuaBerranteTelegramFlags, request_maker: fun(props: RequestProps): RequestResponse): BerranteTelegramBot | nil Cria nova sessão do Telegram Bot
```

### Funcionalidades do Sistema

#### Integração com IDEs Modernas
As definições de tipo são automaticamente reconhecidas pelas seguintes ferramentas de desenvolvimento:

- **Visual Studio Code**: Através da extensão Lua Language Server
- **IntelliJ IDEA**: Via plugin Lua
- **Vim/Neovim**: Com configuração LSP apropriada
- **Ferramentas CLI**: lua-language-server para análise estática

#### Suporte à Análise Estática
O sistema permite:
- Detecção automática de erros de tipo
- Autocomplete inteligente baseado em contexto
- Navegação de código aprimorada
- Refatoração assistida por IDE
- Documentação inline em tempo real

#### Validação de Código em Tempo Real
```lua
-- Exemplo de validação automática
---@param user_data table<string, any>
---@return boolean success
---@return string | nil error_message
local function validate_user(user_data)
    -- IDE automaticamente detecta tipos incorretos
    if user_data.name ~= "string" then -- Warning: comparação inválida
        return false, "Nome deve ser string"
    end
    return true
end
```

## Especificação Técnica da Implementação

### Objetivos Principais

1. **Padronização**: Implementar sistema de tipagem uniforme across todos os projetos Lua da organização
2. **Compatibilidade**: Garantir compatibilidade total com LuaLS e ferramentas modernas de desenvolvimento
3. **Documentação**: Criar documentação de API rica e navegável diretamente no IDE
4. **Produtividade**: Aumentar significativamente a produtividade dos desenvolvedores através de melhor suporte ferramental

### Arquitetura de Tipagem

#### Estrutura de Arquivos Requerida
```
projeto_lua/
├── types.lua               # Obrigatório: definições centrais de tipo
├── README.md              # Documentação do projeto
├── examples/              # Exemplos de uso com tipos
│   ├── basic_usage.lua
│   └── advanced_features.lua
└── src/                   # Código fonte da biblioteca
    ├── main.lua
    └── modules/
```

#### Hierarquia de Tipagem
```lua
-- Estrutura obrigatória no types.lua
---@meta

-- 1. Definição do objeto global principal
---@class GlobalObjectName
local global_object = {}

-- 2. Definição de todas as classes públicas
---@class PublicClass
---@field public_field type

-- 3. Definição de todas as funções públicas
---@param param type
---@return return_type
function global_object.public_function(param) end

-- 4. Export do objeto global
return global_object
```

### Convenções de Nomenclatura

#### Padrões Obrigatórios
- **Classes**: PascalCase (`HttpClient`, `DatabaseConnection`)
- **Campos**: snake_case (`user_id`, `connection_string`)
- **Funções**: snake_case (`get_user`, `create_connection`)
- **Constantes**: SCREAMING_SNAKE_CASE (`MAX_CONNECTIONS`, `DEFAULT_TIMEOUT`)

#### Convenções de Documentação
```lua
---@class HttpResponse Representa uma resposta HTTP completa
---@field status_code integer Código de status HTTP (200, 404, etc.)
---@field headers table<string, string> Cabeçalhos da resposta
---@field body string Corpo da resposta em formato string
---@field json fun(): table | nil Converte o corpo para JSON se possível
```

## Informações Contratuais

### Estrutura de Pagamento
- **Valor base por projeto**: R$ 15,00 - R$ 60,00 (baseado em complexidade)
- **Total de projetos**: 10 bibliotecas selecionadas
- **Valor total estimado**: R$ 310,00

### Metodologia de Entrega
**IMPORTANTE**: Os projetos podem ser submetidos individualmente para processamento gradual dos pagamentos e validação contínua de qualidade.

### Prazo de Execução
- **Deadline final**: 31 de outubro de 2025
- **Entrega recomendada**: Submissão gradual para feedback iterativo

## Escopo de Projetos

## Escopo de Projetos

### Lista de Bibliotecas para Implementação de Tipagem

As seguintes 10 bibliotecas Lua da organização OUI Technologies foram selecionadas para implementação do sistema de tipagem:

| # | Projeto | Valor | Objeto Global | Complexidade | Descrição Técnica |
|---|---------|-------|---------------|--------------|-------------------|
| 1 | **[LuaWebDriver](https://github.com/OUIsolutions/LuaWebDriver)** | R$ 60,00 | `webdriver` | Alta | Biblioteca de automação web compatível com WebDriver Protocol. Controle programático de browsers Chrome/Chromium com suporte completo para navegação, interação com elementos DOM, gerenciamento de janelas e execução de JavaScript. |
| 2 | **[LuaDoTheWorld](https://github.com/OUIsolutions/LuaDoTheWorld)** | R$ 60,00 | `dtw` | Alta | Framework utilitário multiplataforma para operações de sistema de arquivos. Inclui leitura/escrita de arquivos, manipulação de diretórios, operações de metadata, e funcionalidades de sistema operacional cross-platform. |
| 3 | **[Serjao Berranteiro](https://github.com/SamuelHenriqueDeMoraisVitrio/SerjaoBerranteiroServer)** | R$ 30,00 | `serjao_berranteiro` | Média | Servidor HTTP/WebSocket simplificado com roteamento automático e middleware system. Arquivo `serjao_berranteiro.lua` da release contém a maioria das definições públicas necessárias. |
| 4 | **[LuaFluidJson](https://github.com/OUIsolutions/LuaFluidJson)** | R$ 20,00 | `json` | Baixa | Parser/Serializer JSON otimizado com API fluente. Suporte para encoding/decoding, validação de schema, manipulação de estruturas JSON complexas e integração com tipos Lua nativos. |
| 5 | **[LuaArgv](https://github.com/OUIsolutions/LuaArgv)** | R$ 15,00 | `argv` | Baixa | Parser de argumentos de linha de comando com suporte para flags, parâmetros posicionais, validação automática e geração de help. Interface declarativa para definição de CLI. |
| 6 | **[LuaShip](https://github.com/OUIsolutions/LuaShip)** | R$ 25,00 | `ship` | Média | Wrapper Lua para Podman/Docker com gerenciamento de containers, volumes, redes e images. Abstração high-level para operações de containerização e orquestração. |
| 7 | **[Lua-Bear](https://github.com/OUIsolutions/Lua-Bear)** | R$ 25,00 | `luabear` | Média | Cliente HTTPS completo com suporte para SSL/TLS, autenticação, cookies, redirects automáticos, upload de arquivos multipart e streaming de grandes payloads. |
| 8 | **[LuaBerrante](https://github.com/SamuelHenriqueDeMoraisVitrio/LuaBerrante)** | R$ 25,00 | `luaberrante` | Média | SDK completo para Telegram Bot API com suporte a todos os tipos de mensagem, inline keyboards, callbacks, webhook processing e gerenciamento de sessões. |
| 9 | **[LuaHeritage](https://github.com/mateusmoutinho/LuaHeritage)** | R$ 20,00 | `heritage` | Média | Sistema avançado de Programação Orientada a Objetos para Lua. Implementa herança múltipla, polimorfismo, encapsulamento, métodos abstratos e sistema de mixins. |
| 10 | **[clpr](https://github.com/OUIsolutions/clpr)** | R$ 20,00 | `clpr` | Média | Framework para execução multiprocesso em linha de comando com pool de workers, queue management, IPC (Inter-Process Communication) e monitoramento de recursos. |

### Especificações Técnicas por Categoria

#### Bibliotecas de Alta Complexidade (R$ 60,00)
**Características:**
- 50+ funções/métodos públicos
- Múltiplas classes e interfaces
- Estados complexos e callbacks
- Integração com APIs externas
- Documentação extensiva de parâmetros

#### Bibliotecas de Média Complexidade (R$ 20-30,00)
**Características:**
- 15-50 funções/métodos públicos
- Classes principais com hierarquia simples
- Configuração através de tabelas de opções
- API relativamente linear

#### Bibliotecas de Baixa Complexidade (R$ 15-20,00)
**Características:**
- Menos de 15 funções públicas
- Interface funcional simples
- Poucos ou nenhum estado interno
- Documentação direta e concisa

### Dependências e Integrações

#### Projetos com Dependências Externas
- **LuaWebDriver**: Requer ChromeDriver/GeckoDriver
- **LuaDoTheWorld**: Interface com APIs do sistema operacional
- **Lua-Bear**: Dependência de bibliotecas SSL/TLS
- **LuaShip**: Interface com Docker/Podman CLI

#### Projetos Standalone
- **LuaFluidJson**: Implementação pura em Lua
- **LuaArgv**: Sem dependências externas
- **LuaHeritage**: Metaclasses e metatables Lua padrão
- **clpr**: Utiliza apenas APIs POSIX padrão

## Requisitos de Implementação

### Padrões de Qualidade Obrigatórios

#### 1. Cobertura Completa da API Pública
- ✅ Todas as funções exportadas devem ter assinatura tipada
- ✅ Todos os campos de classe devem ser documentados
- ✅ Parâmetros opcionais devem usar sintaxe `param?`
- ✅ Valores de retorno múltiplos devem ser especificados

#### 2. Documentação Inline Rica
```lua
---@param connection_string string URL de conexão no formato protocol://host:port/database
---@param options? table<string, any> Opções de configuração opcional
---@param options.timeout? number Timeout em segundos (default: 30)
---@param options.ssl? boolean Usar conexão SSL (default: false)
---@return DatabaseConnection connection Instância de conexão ativa
---@return string | nil error Mensagem de erro em caso de falha
function database.connect(connection_string, options) end
```

#### 3. Validação de Tipos Rigorosa
- ✅ Uso correto de unions (`string | number`)
- ✅ Especificação de arrays (`string[]`, `table<number, User>`)
- ✅ Callbacks tipados (`fun(param: string): boolean`)
- ✅ Tipos opcionais explícitos (`field?: type`)

### Ferramentas de Validação

#### Checklist de Qualidade LuaLS
1. **Arquivo válido**: `types.lua` não deve ter erros de sintaxe
2. **Resolução de tipos**: Todas as referências devem resolver corretamente
3. **Documentação completa**: Parâmetros e retornos documentados
4. **Convenções**: Nomenclatura consistente com padrões estabelecidos

#### Exemplo de Validação IDE
```lua
-- Teste de validação: este código deve funcionar sem warnings
local webdriver = require("webdriver_types")

---@type webdriver.Driver
local driver = webdriver.new()

-- IDE deve reconhecer estes métodos automaticamente
driver:get("https://example.com")
driver:find_element("css", "#button"):click()
driver:close()
```

## Critérios de Aceitação

### Requisitos Técnicos Obrigatórios
1. ✅ Arquivo `types.lua` localizado na raiz do projeto
2. ✅ Compatibilidade completa com LuaLS (lua-language-server)
3. ✅ Cobertura de 100% da API pública documentada
4. ✅ Validação sem erros em IDE compatível
5. ✅ Estrutura consistente com convenções estabelecidas

### Entregáveis por Projeto
- **Arquivo `types.lua`**: Implementação completa das definições
- **Documentação técnica**: README atualizado com instruções de uso
- **Exemplos de uso**: Código demonstrativo com tipos aplicados
- **Relatório de validação**: Confirmação de compatibilidade LuaLS

### Processo de Validação
1. **Análise estática**: Verificação automática via lua-language-server
2. **Teste de integração**: Validação em VS Code com extensão Lua
3. **Review de código**: Verificação de conformidade com padrões
4. **Teste de usabilidade**: Validação de experiência do desenvolvedor

### Metodologia de Aceitação
- **Aprovação individual**: Cada projeto é validado independentemente
- **Feedback iterativo**: Possibilidade de revisões e melhorias
- **Pagamento progressivo**: Liberação de pagamento após validação
- **Documentação de qualidade**: Manutenção de padrões consistentes

---

**Documento de Especificação Técnica - v1.0**  
**OUI Technologies Lua Type System Implementation Project**  
**Data:** 18 de setembro de 2025