# FF4 Native FPS Unlock v1.0

by **oHenriCard**

FINAL FANTASY IV (3D Remake) - Steam (app 312750, `FF4.exe`, build 24179664)

Português (PT-BR) primeiro · English below

---

## Português (PT-BR)

### O que o mod faz
- **60+ FPS reais** (60, 120, 144, 180, 240... ou a taxa do seu monitor), sem acelerar o jogo.
- **DirectX 12 ou Vulkan** opcionais (o OpenGL do jogo é traduzido pelo Mesa 3D), ou OpenGL nativo.
- **Pular as logos** Square Enix / Matrix na abertura (e, opcionalmente, o vídeo de abertura).
- **Instalador e desinstalador** (`Install.cmd` / `Uninstall.cmd`).

### Por que a batalha roda a 15 FPS (e por que o mod não "conserta" isso do jeito ingênuo)
O motor do jogo é um port do FF4 de Nintendo DS. Cada cena escolhe a própria taxa de lógica:
**título 60, campo 30, batalha 15**. Não existe delta-time: animações de batalha, barras de ATB e
timers contam *ticks*. Por isso os mods que simplesmente aumentam o FPS deixam o jogo 2x a 4x mais
rápido (a batalha fica 4x mais rápida a 60 FPS).

Este mod mantém a lógica **exatamente** na taxa que cada cena pediu (lida do motor a cada tick:
60/30/15) e faz o **renderizador** rodar na taxa do monitor: cada chamada OpenGL do jogo é gravada
por tick e reproduzida várias vezes com vértices e câmera interpolados entre o tick anterior e o atual.
Cada quadro mostrado é um render real da geometria do jogo (não é geração de quadros por imagem).
Resultado medido (RTX 5060, monitor 180 Hz): batalha com lógica a 15,00 ticks/s e ~176 FPS na tela;
campo a 30,00 ticks/s e 170-180 FPS.

### Instalação
1. Feche o jogo.
2. Rode `Install.cmd` (ele acha a pasta do jogo pela Steam; ou arraste a pasta do jogo sobre ele).
3. Responda: API gráfica (0 OpenGL, 1 DirectX 12, 2 Vulkan), FrameRate (0 = monitor),
   pular logos (padrão: sim) e pular o vídeo de abertura (padrão: não).
4. Inicie o jogo pela Steam normalmente.

Arquivos copiados para a pasta do jogo: `version.dll`, `FF4NativeFPSUnlock.ini` e a pasta `mesa\`.
Se já existir um `version.dll` de outro mod, ele é guardado como `version.dll.FF4NFU-backup`.

### Desinstalação
Rode `Uninstall.cmd`. Ele remove só os arquivos deste mod (o `version.dll` só é apagado se for o
deste mod), restaura um `version.dll` de terceiros que tenha sido guardado e deixa o jogo como era.

### Opções (`FF4NativeFPSUnlock.ini`, na pasta do jogo)
| Chave | Padrão | Descrição |
|---|---|---|
| `Enabled` | 1 | 0 desliga o mod sem desinstalar |
| `Renderer` | OpenGL | `OpenGL`, `DirectX12` ou `Vulkan` |
| `FrameRate` | 0 | 0 = taxa do monitor; ou 60, 120, 144, 165, 240... |
| `SkipSplash` | 1 | pula as logos Square Enix / Matrix |
| `SkipMovie` | 0 | pula o vídeo de abertura (`opening.mkv`) |
| `Interpolate` | 1 | 0 = só pacing preciso, quadros repetidos |
| `Mode` | auto | `auto`, `vsync` ou `timed` |
| `VSync` | -1 | -1 = como o jogo define, 0 desliga, 1 liga |
| `Log` | 1 | grava `FF4NativeFPSUnlock.log` na pasta do jogo |

As demais chaves (`MaxDelta2D`, `MaxDelta3D`, `MatrixLerp`, `SpriteFrameGuard`, `Strict2D`,
`SpriteFrameGuard3D`, `OverlayCompat`, `DebugDump`) estão comentadas no próprio ini.

### Dicas e compatibilidade
- **Outros mods de FPS** (ex.: o "Set FPS" de mods QoL via `dinput8.dll`): deixe o FPS deles no
  original. Eles aceleram o jogo; este mod já entrega os quadros extras sem mudar a velocidade.
- **DirectX 12 / Vulkan** precisam de Windows 10/11 e GPU com D3D12 / Vulkan. O log mostra a API em
  uso, ex.: `renderer=D3D12 (sua GPU) ... Mesa`.
- Se a GPU ou a camada de tradução não conseguir apresentar todos os quadros, o mod descarta quadros
  intermediários em vez de deixar a lógica mais lenta: a velocidade do jogo nunca cai.
- **ReShade**: instale para a API **OpenGL**. Em modo DirectX 12 o mod aponta o `ProxyLibrary` do
  ReShade para o Mesa automaticamente (`OverlayCompat=1`).
- Durante o vídeo de abertura o jogo usa o próprio limitador (o vídeo é 24 FPS).
- O mod confere o código do executável ao iniciar; se a Steam atualizar o jogo e os endereços
  mudarem, ele se desativa sozinho (veja o log) em vez de arriscar um crash.

---

## English

### What it does
- **Real 60+ FPS** (60, 120, 144, 180, 240... or your monitor's refresh rate) without speeding the game up.
- Optional **DirectX 12 or Vulkan** (the game's OpenGL is translated by Mesa 3D), or native OpenGL.
- **Skip the Square Enix / Matrix logos** at boot (and, optionally, the opening video).
- **Installer and uninstaller** (`Install.cmd` / `Uninstall.cmd`).

### Why battles run at 15 FPS
The engine is a port of the Nintendo DS version. Every scene picks its own logic rate: **title 60,
field 30, battle 15**. There is no delta-time - battle animations, ATB gauges and timers count ticks -
so mods that simply raise the frame rate make the game 2x-4x faster.

This mod keeps the logic at exactly the rate each scene asked for (read from the engine every tick)
and runs the **renderer** at the display rate: the game's OpenGL calls are recorded per tick and
re-rendered with vertices and camera interpolated between the previous and the current tick. Every
presented frame is a true render of the game's geometry. Measured (RTX 5060, 180 Hz): battle logic at
15.00 ticks/s with ~176 FPS presented; field at 30.00 ticks/s with 170-180 FPS.

### Install / uninstall
Close the game, run `Install.cmd`, answer the questions (API, FrameRate, skip logos, skip video) and
start the game from Steam. `Uninstall.cmd` removes only this mod's files and restores a third-party
`version.dll` that was backed up during install.

### Notes
- Leave other FPS mods (e.g. a `dinput8.dll` "Set FPS" option) at the original value.
- DirectX 12 / Vulkan need Windows 10/11 and a D3D12 / Vulkan capable GPU.
- If presents are too slow for the refresh rate, in-between frames are dropped; the logic never slows down.
- ReShade: install it for the OpenGL API.
- The mod verifies the executable at startup and disables itself (see the log) if a game update moved the code.

---

## Technical notes / Notas técnicas
- Loads as `version.dll` (imported by `SDL2.dll`), hooks the game's `OPENGL32.dll` and `SDL2.dll`
  imports. Nothing in the game files is modified on disk.
- Tick rate: `FF4.exe+0x1DF360` (int, used by the limiter `SDL_Delay(1000/fps - elapsed)` at
  `+0x29435`). Game-frame swap: return address `+0x291C8`.
- Logos: state jump table `+0x126CD4`; state 0 is redirected to the end of state 5 (`+0x126C49`),
  which still requests the next part (`requestPart(2)`) and fades out, so the boot flow is unchanged.
- Opening video: `release_movie_objects()` at `+0x41C40` - the same call the game makes when you
  confirm "skip this scene".
- The battle code polls `glGetError()`: errors raised by the mod's replays are always cleared so they
  can never reach the game.
- Sprites / particles with identical signatures are paired with the nearest one of the previous tick,
  after removing the camera motion (the engine transforms everything on the CPU, so walking moves every
  vertex; the motion is the median displacement of the scenery). Unpaired ones follow the camera.
- Build: `source/build.sh` (MSYS2 MinGW-w64 i686 gcc).
- Mesa 3D 26.2.4 x86 (MIT licence, `mesa/LICENSE-mesa.txt`).

---

## Créditos / Credits
- **oHenriCard** - mod, engenharia reversa e testes / mod, reverse engineering and testing.
- **Mesa 3D** (mesa3d.org) - renderizadores DirectX 12 (d3d12) e Vulkan (Zink), licença MIT /
  DirectX 12 (d3d12) and Vulkan (Zink) renderers, MIT licence. Build x86 de pal1000/mesa-dist-win.
- FINAL FANTASY IV (3D Remake) © SQUARE ENIX. Mod não oficial, sem afiliação / unofficial mod, not affiliated.
