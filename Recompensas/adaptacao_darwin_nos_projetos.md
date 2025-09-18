# Migração de Projetos OUI para o Darwin Build System

## Visão Geral

Este documento define as especificações técnicas e o escopo para a migração de 23 projetos da organização OUISolutions para o novo formato de build do Darwin. O objetivo principal é padronizar e modernizar o sistema de construção de todos os projetos, estabelecendo uma estrutura unificada baseada em arquivos de configuração específicos.

## Contexto Técnico

O Darwin Build System é uma ferramenta de automação de build que utiliza uma arquitetura modular baseada em dois arquivos principais de configuração. Para esta migração, utilizaremos o próprio repositório [Darwin](https://github.com/OUIsolutions/Darwin) como referência de implementação.

### Estrutura do Sistema

#### 1. Arquivo de Dependências (`darwindeps.json`)

Este arquivo JSON é responsável pelo gerenciamento automatizado de dependências externas do projeto. Ele permite especificar releases do GitHub, URLs de repositórios e arquivos específicos que devem ser baixados e organizados automaticamente na estrutura do projeto.

**Características principais:**
- Suporte para releases específicas de repositórios GitHub
- Download automatizado de arquivos de dependências
- Organização hierárquica de destinos personalizáveis
- Controle de versioning integrado

##### Types:
*gitrelease*
Serve para pegar um arquivo direto das releases;
```json
[
  {  
    "type": "gitrelease",
    "repo": "owner/Name_repo",
    "file": "release archive",
    "tag":  "release version",
    "dest": "dependencies/Name_out_dependencie.extension"
  }
]
```

*url*
Serve para pegar um arquivo direto pela URL;
```json
[
  {  
    "type": "url",
    "url":"link",
    "dest": "dependencies/Name_out_dependencie.extension"
  }
]
```

*gitrepo*
Serve para clonar uma dependencia;
```json
[
  {  
    "type": "gitrepo",
    "repo": "owner/Name_repo",
    "dest": "dependencies/",
    "branch": "name_branch"
  }
]
```

**Exemplo de configuração `darwindeps.json`:**
 ```json
[
    {
        "type": "gitrelease",
        "repo": "OUIsolutions/MDeclare",
        "file": "MDeclareApiNoDependenciesIncluded.h",
        "tag":"0.2.0",
        "dest": "dependencies/MDeclareApiNoDependenciesIncluded.h"
    },
    {
        "type": "gitrelease",
        "repo": "OUIsolutions/LuaFluidJson",
        "file": "luaFluidJson_no_dep.c",
        "tag":"0.6.1",
        "dest": "dependencies/luaFluidJson_no_dep.c"
    },
    {
        "type": "gitrelease",
        "repo": "OUIsolutions/LuaDoTheWorld",
        "file": "luaDoTheWorld_no_dep.c",
        "tag":"0.14.0",
        "dest": "dependencies/luaDoTheWorld_no_dep.c"
    },
    {
        "type": "gitrelease",
        "repo": "SamuelHenriqueDeMoraisVitrio/candangoEngine",
        "file":"CandangoEngine.c",
        "tag":"0.2.0",
        "dest": "dependencies/candangoEngine.c"
    },
    {
        "type": "gitrelease",
        "tag":"3.0.000",
        "repo": "OUIsolutions/CTextEngine",
        "file": "CTextEngineOne.c",
        "dest": "dependencies/CTextEngineOne.c"
    },
    {
        "type": "gitrelease",
        "tag":"0.1.0",
        "repo": "OUIsolutions/LuaMDeclare",
        "file": "luamdeclare.c",
        "dest": "dependencies/luamdeclare.c"
    },
    {
        "type": "gitrelease",
        "repo": "OUIsolutions/LuaCEmbed",
        "tag":"0.12.0",
        "file": "LuaCEmbedOne.c",
        "dest": "assets/api/LuaCEmbedOne.c"
    },
    {
        "type": "gitrelease",
        "tag":"11.1.0",
        "repo": "OUIsolutions/DoTheWorld",
        "file": "doTheWorldOne.c",
        "dest": "dependencies/doTheWorldOne.c"
    },
    {
        "type": "gitrelease",
        "tag":"0.1.0",
        "repo": "OUIsolutions/LuaArgv",
        "file": "luargv.lua",
        "dest": "dependencies/luargv.lua"
    },
    {
        "type": "gitrelease",
        "tag":"0.2.0",
        "repo": "OUIsolutions/LuaShip",
        "file": "LuaShip.lua",
        "dest": "dependencies/LuaShip.lua"
    },
    {
        "type": "gitrelease",
        "tag":"0.2.1",
        "repo": "OUIsolutions/LuaSilverChain",
        "file": "silverchain_no_dependecie_included.c",
        "dest": "dependencies/silverchain_no_dependecie_included.c"
    },
    {
        "type": "gitrelease",
        "tag":"0.1.0",
        "repo": "OUIsolutions/LuaCAmalgamator",
        "file": "lua_c_amalgamator_dependencie_not_included.c",
        "dest": "dependencies/lua_c_amalgamator_dependencie_not_included.c"
    }
```

#### 2. Arquivo de Configuração Principal (`darwinconf.lua`)

O arquivo `darwinconf.lua` serve como blueprint central do projeto, definindo todas as configurações essenciais e carregando as receitas de build de forma modular.

**Responsabilidades:**
- Definição de metadados do projeto (nome, versão, licença, etc.)
- Configuração de parâmetros de build personalizáveis
- Carregamento automático de receitas de build modulares
- Integração com sistemas de containerização

**Exemplo de configuração `darwinconf.lua`:**
```lua 
PROJECT_NAME = "darwin"
CONTANIZER   =  darwin.argv.get_flag_arg_by_index({ "contanizer", }, 1,"docker" ) 
VERSION      = "0.7.0"
LICENSE      = "MIT"
URL          = "https://github.com/OUIsolutions/Ai-RagTemplate"
DESCRIPITION = "A Runtime to work with llms"
FULLNAME     = "VibeScript"
EMAIL        = "mateusmoutinho01@gmail.com"
SUMARY       = "A Runtime to work with llms"
YOUR_CHANGES = "--"

```

### Funcionalidades do Sistema

#### Execução de Receitas Individuais
As receitas de build podem ser executadas individualmente utilizando o seguinte comando:

```bash 
darwin run_blueprint darwinconf.lua --target <nome_da_receita> 
```

#### Listagem de Receitas Disponíveis
Para visualizar todas as receitas disponíveis em um projeto:

```bash
darwin list_blueprints 
```

#### Exemplo de Receita de Build

As receitas são definidas como funções Lua que encapsulam toda a lógica de build, incluindo configuração de ambiente, compilação e empacotamento:

```lua 
```lua

function linux_bin()

    os.execute("mkdir -p release")

    local image = darwin.ship.create_machine("alpine:latest")
    image.provider = CONTANIZER
    image.add_comptime_command("apk update")
    image.add_comptime_command("apk add --no-cache gcc g++ musl-dev curl")
    local compiler = "gcc"
    if LAUNGUAGE == "cpp" then
        compiler = "g++"
    end

    image.start({
        volumes = {
            { "././release", "/release" },
        },
        command = compiler..[[ --static /release/darwin.c  -o /release/darwin_linux_bin.out]]

    })
end

darwin.add_recipe({
    name="linux_bin",
    requires={"amalgamation"},
    description="make a static compiled linux binary of the project",
    outs={"release/darwin_linux_bin.out"},
    callback=linux_bin
```

Esta receita demonstra:
- Criação de ambiente isolado utilizando containers Alpine Linux
- Configuração automática de dependências de compilação
- Suporte multi-linguagem (C/C++)
- Build estático para máxima portabilidade
- Integração com sistema de volumes para output

## Especificação Técnica da Migração

### Objetivos Principais

1. **Padronização**: Migrar todos os projetos listados para o formato uniforme do Darwin Build System
2. **Modularização**: Implementar estrutura modular com arquivos `darwindeps.json` (quando necessário) e `darwinconf.lua`
3. **Organização**: Reestruturar receitas de build como arquivos individuais `.lua` dentro da pasta `builds/`
4. **Compatibilidade**: Manter compatibilidade com os modos de execução existentes

### Arquitetura de Build

#### Estrutura de Arquivos Requerida
```
projeto/
├── darwindeps.json          # Opcional: somente se houver dependências externas
├── darwinconf.lua          # Obrigatório: configuração principal
└── builds/                 # Obrigatório: pasta de receitas
    ├── receita1.lua
    ├── receita2.lua
    └── ...
```

#### Configuração Principal
O arquivo `darwinconf.lua` deve incluir obrigatoriamente:
```lua
-- Configurações do projeto
PROJECT_NAME = "nome_do_projeto"
VERSION = "x.x.x"
LICENSE = "MIT"
-- ... outras configurações

-- Carregamento automático de todas as receitas
darwin.load_all("builds")
```

### Modos de Execução Legados
Esses modos legados servem como base para entender os diferentes métodos de execução suportados pelo Darwin Build System.

#### Modo Pasta
Este modo utiliza uma estrutura de pasta organizada onde o Darwin carrega automaticamente todos os arquivos `.lua` da pasta `builds/`:

**Comandos de exemplo:**
```bash
# Execução básica
darwin run_blueprint build/ --mode folder

# Execução com múltiplas receitas e containerização personalizada
darwin run_blueprint build/ --mode folder amalgamation_build alpine_static_build windowsi32_build windows64_build rpm_static_build debian_static_build --contanizer podman
```

**Características:**
- Carregamento automático de todas as receitas `.lua` na pasta `builds/`
- Execução da função `main()` após carregamento
- Suporte para execução de receitas múltiplas em sequência

#### Modo Arquivo Único
Modo alternativo que utiliza o arquivo `darwinconf.lua` diretamente:

```bash
darwin run_blueprint darwinconf.lua <parâmetros_adicionais>
```

## Informações Contratuais

### Estrutura de Pagamento
- **Valor por projeto**: R$ 18,00
- **Total de projetos**: 23
- **Valor total do projeto**: R$ 414,00

### Metodologia de Entrega
**IMPORTANTE**: Os projetos devem ser submetidos individualmente para processamento gradual dos pagamentos e mitigação de riscos competitivos.

## Escopo de Projetos

### Lista de Projetos para Migração

Os seguintes 23 projetos da organização OUISolutions devem ser migrados para o novo formato Darwin:
1.  **[VibeScript](https://github.com/OUIsolutions/VibeScript)** - Runtime de desenvolvimento para integração com LLMs
2.  **[CWebStudio](https://github.com/OUIsolutions/CWebStudio)** - Framework web em C para desenvolvimento full-stack
3.  **[DoTheWorld](https://github.com/OUIsolutions/DoTheWorld)** - Biblioteca C para operações de sistema multiplataforma
4.  **[LuaCEmbed](https://github.com/OUIsolutions/LuaCEmbed)** - Sistema de embedding Lua em aplicações C
5.  **[BearHttpsClient](https://github.com/OUIsolutions/BearHttpsClient)** - Cliente HTTPS leve para C
6.  **[LuaDoTheWorld](https://github.com/OUIsolutions/LuaDoTheWorld)** - Binding Lua para DoTheWorld
7.  **[Lua-bear](https://github.com/OUIsolutions/Lua-bear)** - Wrapper Lua para BearHttpsClient
8.  **[LuaWebDriver](https://github.com/OUIsolutions/LuaWebDriver)** - Automação de browsers via Lua
9.  **[CWebStudioFirmware](https://github.com/OUIsolutions/CWebStudioFirmware)** - Firmware para CWebStudio
10. **[LuaSilverChain](https://github.com/OUIsolutions/LuaSilverChain)** - Sistema de templates para Lua
11. **[clpr](https://github.com/OUIsolutions/clpr)** - Parser de linha de comando para C
12. **[LuaSingleUnity](https://github.com/OUIsolutions/LuaSingleUnity)** - Integração Unity-Lua
13. **[C2Wasm](https://github.com/OUIsolutions/C2Wasm)** - Compilador C para WebAssembly
14. **[MDeclare](https://github.com/OUIsolutions/MDeclare)** - Sistema de declaração de módulos C
15. **[CAmalgamator](https://github.com/OUIsolutions/CAmalgamator)** - Amalgamador de arquivos C
16. **[SilverChain](https://github.com/OUIsolutions/SilverChain)** - Sistema de templates C
17. **[LuaMDeclare](https://github.com/OUIsolutions/LuaMDeclare)** - Binding Lua para MDeclare
18. **[LuaShip](https://github.com/OUIsolutions/LuaShip)** - Sistema de deployment Lua
19. **[PushBlind](https://github.com/OUIsolutions/PushBlind)** - Sistema de notificações push
20. **[LuaArgv](https://github.com/OUIsolutions/LuaArgv)** - Parser de argumentos para Lua
21. **[Universal-Garbage-Colector](https://github.com/OUIsolutions/Universal-Garbage-Colector)** - Coletor de lixo universal

## Critérios de Aceitação

### Requisitos Técnicos Obrigatórios
1. ✅ Arquivo `darwinconf.lua` configurado corretamente
2. ✅ Arquivo `darwindeps.json` presente quando necessário
3. ✅ Pasta `builds/` com receitas modulares
4. ✅ Chamada `darwin.load_all("builds")` implementada
5. ✅ Compatibilidade com modos de execução existentes

### Entregáveis por Projeto
- Estrutura de arquivos completa e funcional
- Documentação de configuração específica (se aplicável)
- Validação de builds existentes
- Relatório de migração resumido

---

**Documento de Especificação Técnica - v1.0**  
**OUISolutions Build System Migration Project**  
**Data:** 05 de Outubro de 2025

