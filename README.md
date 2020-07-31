# windows-dot-profile

Como emular um ~/.bashrc ou ~/.profile no Windows

## Ideia

A idéia aqui é emular o comportamento dos arquivos `~/.bashrc`, `~/.profile` e similares
em sistemas Windows.

Em sistemas Unix, sempre que você conecta, abre uma sessão SSH, ou abre um novo terminal,
um determinado arquivo (na verdade uma sequência deles) é executado para configurar o
ambiente conforme necessário.

Isso é bastante útil para configurar as variáveis de ambiente, por exemplo a variável `$PATH`
para caminhos personalizados de executáveis que estarão disponíveis, configurar aliases
e uma série de outras coisas.

Mas, como você já deve saber, o Windows não tem este recurso por padrão. Ou seja, não há
um arquivo específico para você simplesmente adicionar código de customização nele.

Então aqui compartilhamos uma maneira de fazer isso.

## Prática

Execute o Editor de Registros do Windows (`regedit.exe`).

Nele você deve procurar pela chave (somente para o usuário corrente):
`HKEY_CURRENT_USER\Software\Microsoft\Command Processor`

Nele você deve procurar pela chave (para todos os usuários do computador):
`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Command Processor`

Então adicione um novo **Valor da Cadeia de Caracteres** chamado `AutoRun`, com
o seguinte conteúdo:
`IF EXIST %UserProfile%\profile.cmd ( %UserProfile%\profile.cmd )`

Este é um simples script `BAT` que verifica a existência de um arquivo no diretório
do usuário chamado `profile.cmd`, e caso exista o executa.

Agora, basta adicionar este arquivo em seu diretório de usuário:
```cmd
@echo off

echo ----------------------------------------------------------------------------------------------
echo Aqui tu mandas
echo Saiba mais em https://superuser.com/questions/144347/is-there-windows-equivalent-to-the-bashrc-file-in-linux
echo Saiba mais em https://stackoverflow.com/questions/20530996/aliases-in-windows-command-prompt
echo ----------------------------------------------------------------------------------------------
```

Este é um script `.BAT` padrão. Então o código que você colocar aqui, será executado sempre que
você abrir um novo **Prompt de Comando**.

O nome do arquivo pode ser o que você preferir, mas fica aqui algumas sugestões:
- `profile.cmd`
- `winrc.cmd`
- `promptrc.cmd`

## Powershell

No PowerShell já temos um recurso para isso.
Se você mandar imprimir no powershell a variável `$Profile`
```ps1
echo $Profile
```

Verá o nome do arquivo que ele carrega sempre que inicia, e pode ser um valor parecido com 
esses:
- `C:\Users\<UserName>\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1`
- `C:\Users\<UserName>\Documents\PowerShell\Microsoft.PowerShell_profile.ps1`

Então basta adicionar seu código lá, que ele será sempre executado quando iniciar
