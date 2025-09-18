
O Objetivo desse projeto, é fazer a tipagem lua de todos os projetos OpenSource da OUI Tecnologia.

# Objetivos:
criar um arquivo chamado `types.lua` em cada projeto contendo toda a documentação pública do projeto em formato de tipagem lua.
Para títlo de exemplos, basei-se no arquivo [types.lua](https://github.com/SamuelHenriqueDeMoraisVitrio/LuaBerrante/blob/main/types.lua) do projeto [Lua Berrante](https://github.com/SamuelHenriqueDeMoraisVitrio/LuaBerrante/tree/main) 
exemplo de tipagem lua.
```lua 

-- Params in newDriver

---@class RequestResponse
---@field status_code integer
---@field headers table<string, string>
---@field read_body fun(): string
---@field read_body_json fun(): table
---@field read_body_chunk fun(size:integer): string

---@class RequestProps
---@field url string
---@field method string | nil
---@field headers table<string, string> | nil
---@field body string | table | nil

---@class TelegramBotInfo
---@field id string
---@field is_bot boolean
---@field first_name string
---@field username string
---@field token string
---@field id_chat string
---@field url string

---@class BerranteTelegramResponse
---@field status_code integer
---@field body string | nil
---@field json table
---@field in_error boolean
---@field chat_id string

---@class BerranteTelegramBot
---@field request fun(props: RequestProps):RequestResponse
---@field infos TelegramBotInfo
---@field getMe fun():BerranteTelegramResponse
---@field sendMessage fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field sendPhoto fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field sendAudio fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field sendDocument fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field sendVideo fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field sendVoice fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field sendLocation fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field sendContact fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field forwardMessage fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field forwardMessages fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field copyMessage fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field copyMessages fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field sendAnimation fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field sendVideoNote fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field sendPaidMedia fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field sendMediaGroup fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field sendVenue fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field sendPoll fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field sendDice fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field sendSticker fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field editMessageText fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field editMessageCaption fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field editMessageMedia fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field editMessageReplyMarkup fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field deleteMessage fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field answerCallbackQuery fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field answerInlineQuery fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field getFile fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field getUserProfilePhotos fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field getChat fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field getChatAdministrators fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field getChatMember fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field getChatMemberCount fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field leaveChat fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]
---@field getUpdates fun(body:table, destination:(string[])[]):BerranteTelegramResponse[]

---@class LuaBerranteTelegramFlags
---@field token string
---@field id_chat string
---@field url string | nil

---@class LuaBerranteModule
---@field newTelegramSession fun(flags: LuaBerranteTelegramFlags, request_maker: fun(props: RequestProps):RequestResponse): BerranteTelegramBot | nil
```
O arquivo `types.lua` deve estar na raiz do projeto, e deve conter a tipagem de todas as funções, classes, tabelas e variáveis públicas do projeto.

# Recompensas por Projeto:

- [LuaWebDriver](https://github.com/OUIsolutions/LuaWebDriver)
valor: R$ 60 
descrição: A WebDriver lib for control chrome browser (similar to selenium)
objeto global: `webdriver`

- [LuaDoTheWorld](https://github.com/OUIsolutions/LuaDoTheWorld)
valor: R$ 60 (25 reais)
descrição: A utility library for file and directory operations, including reading, writing, listing, and more.
objeto global: `dtw`


- [Serjao Berranteiro](https://github.com/SamuelHenriqueDeMoraisVitrio/SerjaoBerranteiroServer)
valor: R$ 30
dica: O arquivo serjao_berranteiro.lua da release, ja contem a maioria das funções públicas do projeto.



- [LuaFluidJson](https://github.com/OUIsolutions/LuaFluidJson)
valor: R$ 20 (20 reais)
descrição: A library for parsing and serializing JSON data, useful for configuration management.
objeto global: `json`

- [LuaArgv](https://github.com/OUIsolutions/LuaArgv)
valor: R$ 15 (15 reais)
descrição: A library for parsing command-line arguments, enabling script customization via CLI.
objeto global: `argv`


- [LuaShip](https://github.com/OUIsolutions/LuaShip)
valor: R$ 25 (25 reais)
descrição: A Podman/Docker wrapper for container management, useful for advanced deployment scenarios.
objeto global: `ship`

- [Lua-Bear](https://github.com/OUIsolutions/Lua-Bear)
valor: R$ 25 (25 reais)
descrição: A Complete Https Client
objeto global: `luabear`


- [LuaBerrante](https://github.com/SamuelHenriqueDeMoraisVitrio/LuaBerrante)
valor: R$ 25 (25 reais)
descrição: A lib to control Telegram over lua
objeto global: `luaberrante`

- [LuaHeritage](https://github.com/mateusmoutinho/LuaHeritage)
valor: R$ 20 (20 reais)
descrição: A lib make complex POO in lua
objeto global: `heregitage`

- [clpr](https://github.com/OUIsolutions/clpr)
valor: R$ 20 (20 reais)
descrição: A lib for multprocess execution in comand line
objeto global: `clpr` 

prazo máximo para todos os projetos: 31 de outubro de 2025