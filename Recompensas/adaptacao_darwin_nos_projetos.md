Este projeto, visa adaptar os projetos da oui para o novo formato
do darwin:
### Contexto:
Para seguir como exemplo,vamos usar o prório [darwin](https://github.com/OUIsolutions/Darwin) como base.
onde você tem dois arquivos principais:
 - `darwindeps.json` 
arquivo responsável por gerenciar as dependencias do projeto, onde você tem 
onde é possível passar as releases, e urls de outros projetos que serão usados como dependencias.
 exemplo de `darwindeps.json`:
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
]
```
 - `darwinconf.lua`
arquivo responsável por configurar o projeto, ele é o "blueprint" de cada
projeto,onde você define as receitas de build.
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

LAUNGUAGE     = "c"
darwin.load_all("builds")
```
nesse exemplo, ele esta definindo as configurações iniciais, e posteriormente carregando as receitas de build. onde cada receita pode ser
 buildada com 
```bash 
darwin run_blueprint darwinconf.lua --target <receita> 
```
bem como todas as receitas podem ser listadas com:
```bash
darwin list_blueprints 
```
exemplo de receita: 
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
})
```

### Objetivos:
portar todos os projetos listados abaixo para o novo formato do darwin:
contendo apenas  `darwindeps.json`(se nescessário) e um `darwinconf.lua` 
que da um `darwin.load_all("builds")` em todas as receitas.
lembre-se: cada receita devew ser um único arquivo `.lua` dentro da pasta `builds/`


### Dica 
Normalmente os projetos da oui são buildados em dois formatos, o modo single file (darwinconf.lua) e o modo pasta
- Modo pasta
no modo pasta, os projetos normalmente são buildados com um comando como esses:
```bash
darwin run_blueprint build/ --mode folder
```
```bash
darwin run_blueprint build/ --mode folder amalgamation_build alpine_static_build windowsi32_build windows64_build rpm_static_build debian_static_build --contanizer podman
```
onde o `build/` é a pasta onde o script de build,e o darwin carrega todos arquivos `.lua` dentro dela, de maneira aleatória, e após isso, ele chama a função main .

- Modo single file (darwinconf.lua)
no modo single file, o projeto é buildado com um comando como esse:
```bash
darwin run_blueprint darwinconf.lua  <resto dos parametros>
```
Valor por Projeto Portado: R$ 18,00
Valor Total : R$ 18 x 23 = R$ 414,00
Atenção: 
Submenta os portes um a um para ir obtendo os valores de pagamentos conforme 
faz o porte, e evitando o risco de perder para a concorrência

### Projetos a serem portados:

#### Lista Principal:
1.  [VibeScript](https://github.com/OUIsolutions/VibeScript)
2.  [CWebStudio](https://github.com/OUIsolutions/CWebStudio)
3.  [DoTheWorld](https://github.com/OUIsolutions/DoTheWorld)
4.  [LuaCEmbed](https://github.com/OUIsolutions/LuaCEmbed)
5.  [BearHttpsClient](https://github.com/OUIsolutions/BearHttpsClient)
6.  [LuaDoTheWorld](https://github.com/OUIsolutions/LuaDoTheWorld)
7.  [Lua-bear](https://github.com/OUIsolutions/Lua-bear) 
9.  [LuaWebDriver](https://github.com/OUIsolutions/LuaWebDriver)
11. [CWebStudioFirmware](https://github.com/OUIsolutions/CWebStudioFirmware)
12. [LuaSilverChain](https://github.com/OUIsolutions/LuaSilverChain)
13. [clpr](https://github.com/OUIsolutions/clpr)
14. [LuaSingleUnity](https://github.com/OUIsolutions/LuaSingleUnity)
15. [C2Wasm](https://github.com/OUIsolutions/C2Wasm)
16. [MDeclare](https://github.com/OUIsolutions/MDeclare)
17. [CAmalgamator](https://github.com/OUIsolutions/CAmalgamator)
18. [SilverChain](https://github.com/OUIsolutions/SilverChain)
19. [LuaMDeclare](https://github.com/OUIsolutions/LuaMDeclare)
20. [LuaShip](https://github.com/OUIsolutions/LuaShip)
21. [PushBlind](https://github.com/OUIsolutions/PushBlind)
22. [LuaArgv](https://github.com/OUIsolutions/LuaArgv)
23. [Universal-Garbage-Colector](https://github.com/OUIsolutions/Universal-Garbage-Colector)

