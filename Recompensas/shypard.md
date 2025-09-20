# 🚢 Implementação da CLI Shipyard

**Projeto de Desenvolvimento** | **Recompensa:** R$ 70,00 | **Prazo:** 30 de Outubro de 2025

---

## 📋 Visão Geral

Este projeto consiste na implementação completa da funcionalidade da CLI [Shipyard](https://github.com/OUIsolutions/shipyard), uma ferramenta de gerenciamento de releases do GitHub construída para o ecossistema VibeScript. O objetivo é recriar todas as funcionalidades da ferramenta original utilizando as tecnologias e bibliotecas nativas do VibeScript.

## 🎯 Objetivo Principal

Desenvolver uma implementação funcional da CLI Shipyard que seja capaz de:

- Automatizar a criação de releases no GitHub
- Gerenciar versioning através de templates personalizáveis
- Manipular assets de release automaticamente
- Gerenciar tags de forma inteligente
- Processar configurações JSON declarativas

## 🛠️ Tecnologias Requeridas

### VibeScript
O projeto deve ser desenvolvido utilizando [VibeScript](https://github.com/OUIsolutions/VibeScript), a linguagem de script base do ecossistema OUI Solutions.

### Bibliotecas Nativas Obrigatórias

As seguintes [bibliotecas nativas](https://github.com/OUIsolutions/VibeScript/blob/main/docs/native_api/buildin_librarys.md) do VibeScript devem ser utilizadas:

#### 1. **LuaFluidJson** (`json`)
- **Propósito**: Parsing e serialização de dados JSON
- **Uso no Projeto**: Processar o arquivo de configuração `release.json`
- **Funcionalidades**: 
  - Leitura e validação do arquivo de configuração
  - Serialização de dados para APIs
  - Manipulação de estruturas de dados JSON

#### 2. **LuaArgv** (`argv`)
- **Propósito**: Parsing de argumentos de linha de comando
- **Uso no Projeto**: Processar argumentos da CLI e opções de configuração
- **Funcionalidades**:
  - Análise de parâmetros de entrada
  - Validação de argumentos
  - Geração de mensagens de ajuda

#### 3. **Execução de Comandos do Sistema** (`os.execute`)
- **Propósito**: Executar comandos do sistema operacional
- **Uso no Projeto**: Executar comandos do GitHub CLI (`gh`) para gerenciar releases
- **Funcionalidades**:
  - Criação de releases via `gh release create`
  - Upload de assets via `gh release upload`
  - Gerenciamento de tags e repositórios
  - Execução de comandos Git quando necessário

## 📁 Funcionalidades a Implementar

### 1. **Gerenciamento de Configuração**
- Leitura e validação do arquivo `release.json`
- Suporte a sistema de templates com replacers
- Validação de campos obrigatórios e opcionais

### 2. **Sistema de Templates**
- Implementação do sistema de substituição `{VARIABLE_NAME}`
- Suporte a templates para nomes de release e tags
- Validação de variáveis não definidas

### 3. **Integração com GitHub CLI**
- Execução de comandos `gh` via `os.execute()` para gerenciar releases
- Criação de releases usando `gh release create`
- Upload de assets usando `gh release upload`
- Gerenciamento de tags via comandos Git
- Validação de autenticação do GitHub CLI

### 4. **Interface de Linha de Comando**
- Parsing de argumentos de entrada
- Mensagens de erro informativas
- Sistema de help/ajuda
- Logging de operações

## 📝 Especificação do Arquivo de Configuração

O arquivo `release.json` deve suportar a seguinte estrutura:

```json
{
   "replacers": {
       "BIG_VERSION": "1",
       "SMALL_VERSION": "0", 
       "PATCH_VERSION": "0"
   },
   "release": "{BIG_VERSION}.{SMALL_VERSION}.{PATCH_VERSION}",
   "tag": "{BIG_VERSION}.{SMALL_VERSION}.{PATCH_VERSION}",
   "description": "Descrição do release",
   "assets": ["caminho/para/asset1", "caminho/para/asset2"]
}
```

### Campos de Configuração

| Campo | Descrição | Obrigatório |
|-------|-----------|-------------|
| `replacers` | Pares chave-valor para substituição de templates | ✅ |
| `release` | Template do nome do release usando variáveis | ✅ |
| `tag` | Template da tag Git usando variáveis | ✅ |
| `description` | Descrição/changelog do release | ✅ |
| `assets` | Array de caminhos para arquivos de assets | ❌ |

## 🔧 Comportamentos Esperados

### Pré-requisitos do Sistema
- **GitHub CLI (`gh`)** deve estar instalado e autenticado no sistema
- Validação da presença do `gh` antes da execução
- Verificação de autenticação via `gh auth status`

### Gerenciamento de Tags
- Se a tag especificada não existir, deve ser criada
- Se a tag existir, o release deve ser atualizado
- Validação de formato de tags

### Gerenciamento de Assets
- Assets existentes com o mesmo nome devem ser substituídos
- Validação da existência dos arquivos antes do upload
- Suporte a múltiplos tipos de arquivo
- Upload via comando `gh release upload <tag> <arquivo>`

### Execução de Comandos GitHub CLI
- Uso de `os.execute()` para executar comandos `gh`
- Captura e tratamento de códigos de retorno dos comandos
- Parsing de saídas de comando quando necessário
- Tratamento de erros de execução do GitHub CLI

### Sistema de Templates
- Uso da sintaxe `{VARIABLE_NAME}` para referenciar valores dos replacers
- Validação de variáveis não definidas
- Suporte a templates aninhados

## 📋 Entregáveis

### 1. **Código Fonte**
- Implementação completa da CLI em VibeScript
- Código bem documentado e estruturado
- Tratamento adequado de erros

### 2. **Testes**
- Casos de teste para as principais funcionalidades
- Validação de diferentes cenários de configuração
- Testes de integração com GitHub API

### 3. **Documentação**
- README com instruções de instalação e uso
- Documentação das funcionalidades implementadas
- Exemplos de uso práticos

## 🎯 Critérios de Aceitação

- [ ] CLI funcional que aceita arquivo `release.json` como parâmetro
- [ ] Parsing correto de argumentos usando LuaArgv
- [ ] Leitura e validação de JSON usando LuaFluidJson
- [ ] Sistema de templates funcionando corretamente
- [ ] Verificação de pré-requisitos (GitHub CLI instalado e autenticado)
- [ ] Criação de releases via comando `gh release create` usando `os.execute()`
- [ ] Upload de assets via comando `gh release upload` usando `os.execute()`
- [ ] Gerenciamento de tags implementado
- [ ] Tratamento adequado de códigos de retorno dos comandos `gh`
- [ ] Tratamento adequado de erros e mensagens informativas
- [ ] Documentação completa do projeto

## 📚 Recursos de Referência

- [Repositório Shipyard Original](https://github.com/OUIsolutions/shipyard)
- [Documentação VibeScript](https://github.com/OUIsolutions/VibeScript)
- [Bibliotecas Nativas VibeScript](https://github.com/OUIsolutions/VibeScript/blob/main/docs/native_api/buildin_librarys.md)
- [LuaFluidJson](https://github.com/OUIsolutions/LuaFluidJson)
- [LuaArgv](https://github.com/OUIsolutions/LuaArgv)
- [GitHub CLI Documentation](https://cli.github.com/manual/)
- [GitHub CLI Releases Commands](https://cli.github.com/manual/gh_release)
- [GitHub API Documentation](https://docs.github.com/en/rest/releases)

---

*Este documento define os requisitos e especificações para o desenvolvimento da CLI Shipyard. Qualquer dúvida sobre os requisitos deve ser esclarecida antes do início do desenvolvimento.*
