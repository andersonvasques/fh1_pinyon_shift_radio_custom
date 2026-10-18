# Guia Windows: Como Adicionar Músicas Customizadas no Forza Horizon 1 (Pinyon Shift)

Este guia detalha o passo a passo para substituir as músicas das rádios originais do Forza Horizon 1 rodando através do emulador Pinyon Shift, especificamente utilizando o **PinyonShift-Launcher**.

---

## ⚠️ Pré-requisitos

1. **Músicas no formato correto:** Todos os arquivos de áudio devem ser estritamente convertidos para **MP3, 48 kHz, 160 kbps CBR (Constant Bitrate) e Estéreo**. Outros formatos causarão erros ou dessincronização de áudio.
2. **Python:** Instalado no seu computador.
3. **FFmpeg:** Necessário para o script processar os áudios (deve estar instalado ou na mesma pasta do script).
4. **Ferramenta de Rádio:** O script `build-fh1-radio.py`, disponível na pasta `tools/` do repositório do Pinyon Shift.
5. Downloads oficiais: [Download Python](https://www.python.org/downloads/); [Download FFmpeg](https://www.gyan.dev/ffmpeg/builds/), desça até a parte de release builds e baixe o arquivo .zip ou .7z

---

## Passo 1: Preparando a pasta de músicas

Crie uma pasta em seu computador e coloque todas as músicas em MP3 que você deseja adicionar à rádio. 
*Exemplo:* `D:\Recomps\Customs\musics`

*(Nota: O script juntará todos esses arquivos em um único pacote `.fsb`. Para adicionar mais músicas no futuro, basta colocar os novos arquivos nesta mesma pasta (`D:\Recomps\Customs\musics`) e rodar o processo novamente).*

---

## Passo 2: Executando o Script de Geração do Mod

Abra o terminal (Prompt de Comando ou PowerShell), navegue até a pasta onde está o script `build-fh1-radio.py` (geralmente em `\PinyonShift-Launcher\tools\`) e execute o comando abaixo.

Preste atenção aos argumentos:
* `--station Radio2`: Substitui a rádio Horizon Pulse. (Use `Radio1` para Bass Arena ou `Radio3` para Rocks).
* `--replace`: Remove a playlist original da rádio escolhida e toca *apenas* as suas músicas.
* `--name my_musics`: Nomeia a pasta do mod. **Dica:** Use sempre letras minúsculas e sem espaços ou simbolos. Exemplo: `my_musics`.

**Exemplo de comando:**
```cmd
python .\build-fh1-radio.py "D:\Recomps\Customs\musics" --station Radio2 --replace --name my_musics --enable --game-root "D:\Recomps\Decompilers\PinyonShift-Launcher\.local\game\base" --state-root "D:\Recomps\Decompilers\PinyonShift-Launcher\.local\state"
```
O script processará as músicas e criará o banco de dados de áudio e a estrutura do mod.

---

## Passo 3: Ativando o Mod no PinyonShift-Launcher (A Etapa Crucial)

Se você utiliza um launcher customizado com versões "preview", o Pinyon Shift ignora a pasta de configurações padrão `.local\state` e passa a ler as configurações de uma pasta dedicada chamada `.local\preview`

Para que o jogo reconheça o seu mod, você precisa ativá-lo no arquivo de configuração correto:

1. Navegue até o diretório do arquivo ativo: `D:\Recomps\Decompilers\PinyonShift-Launcher\.local\preview\config\`
2. Abra o arquivo `pinyon_shift.toml` com um editor de texto (como o Bloco de Notas).
3. Encontre a linha `enabled_mods`. Ela provavelmente já conterá mods ativados pelo launcher
4. Adicione o nome do seu mod (`my_musics`) no final da lista, separado por vírgula e dentro das aspas. Deve ficar parecido com isto:

```toml
enabled_mods = "zz-archive-patches,immersive_camera,my_musics"
```

5. Salve e feche o arquivo.

(Dica de Organização: Durante testes, identificou-se que mods do launcher costumam ficar armazenados em \mods_src\builtin. Embora não seja estritamente obrigatório se o .toml estiver configurado corretamente, você pode mover a pasta `my_musics` gerada no Passo 2 (localizada em `.local\state\mods\`) para dentro de builtin para manter a organização alinhada com o launcher).

## Passo 4: Evitando o "Bug da Rádio Muda" (Ajuste do noRepeat)

O jogo possui um sistema anticópia na rádio que impede a repetição das últimas músicas tocadas. Se você adicionou poucas músicas (ex: 9 músicas) e o jogo tentar não repetir as últimas 10, a rádio ficará muda.

1. Navegue até o diretório onde o seu mod foi salvo e está sendo lido pelo jogo (seja na pasta `state\mods` ou na `mods_src\builtin`).
2. Siga o caminho até o arquivo XML gerado: `...\my_musics\media\audio\radio\RadioSystem.xml`
3. Abra-o no Bloco de Notas e pesquise (`Ctrl + F`) pelo nome da rádio que você alterou (ex: `"Radio2"`).
4. Localize a tag da playlist: `<Playlist noRepeat="X">`
5. Altere o valor de `"X"` para um número menor do que a quantidade de músicas que você adicionou. (Por exemplo, se você adicionou 9 músicas, altere para `noRepeat="3"`).
6. Salve o arquivo.

## Passo 5: Testando no Jogo

1. Abra o Forza Horizon 1 através do PinyonShift-Launcher.
2. Na tela inicial ou de pause, vá até SETTINGS (Configurações) > MODS.
3. Verifique se o seu mod `MY_MUSICS` aparece na lista como ativado.
4. Volte para o jogo, entre no carro, sintonize na estação configurada (ex: Horizon Pulse) e as suas músicas devem começar a tocar.

Observação sobre os Locutores (DJs): Mesmo que você substitua 100% da playlist de músicas, os comentários dos DJs entre as faixas continuarão no jogo, pois eles residem em um banco de áudio separado e original do jogo.