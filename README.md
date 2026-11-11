# Half-Life: Expanded Arsenal -> Xash3D FWGS (Android)

Port do codigo-fonte do mod **Arsenal** (HL SDK 2.3, gamedir `hl_expanded_arsenal`) para o
[hlsdk-portable](https://github.com/FWGS/hlsdk-portable) do FWGS, com FreeVGUI embutido
(`freevgui/`, commit 73bfb46, sem submodule) e um launcher Android minimo.

## Como funciona
- `android/` e um app launcher simples (`su.xash.arsenal`). Ele chama o Xash3D FWGS ja instalado
  (`su.xash.engine` ou `su.xash.engine.test`) com `gamedir=hl_expanded_arsenal` e aponta
  `gamelibdir` para as libs do proprio APK (`libserver.so` / `libclient.so`).
- GitHub Actions (`.github/workflows/apk.yml`) gera o APK debug para arm/arm64/x86/x86_64.
- Os assets do mod (maps, models, sound, sprites, events, liblist.gam, etc.) **nao** fazem parte
  deste repo: coloque a pasta `hl_expanded_arsenal` junto de `valve` no armazenamento do Xash3D.

## Estado
- Compila e linka (client + server) em Linux x86_64 com o hlsdk-portable. Nao foi testado em
  aparelho nem em NDK, e nao foi testado rodando com os assets.
- Todas as entidades das fontes compiladas pelo projeto original (armas, monstros, itens)
  estao presentes no server dll, exceto `trip_beam` (so debug), `my_monster` (template) e `weapon_nuclear`.

## Fonte original vs. este port
- Arquivos novos do mod (armas/monstros) foram copiados 1:1: ver `dlls/` (ak47, deagle, ...).
- Arquivos do SDK modificados pelo mod foram mesclados (3 vias) contra o hlsdk-portable, usando o
  SDK 2.3 da Valve como base. Onde o mod tinha codigo antigo do SDK e o portable tinha correcao
  propria, ficou o portable. Resolucoes manuais: `dlls/weapons.h`, `cdll_dll.h` (MAX_WEAPONS=64,
  slots=6, ammo=64), `player.cpp` (impulse 101 + municoes), `combat.cpp` (tipos de bala),
  `mp5.cpp`, `glock.cpp`, `healthkit.cpp` (item_healthsup), `cl_dll/ev_hldm.*` (eventos novos),
  `cl_dll/hl/hl_weapons.cpp` (predicao client das armas novas).
- NAO compilados (o projeto original tambem nao compilava): `mp7`, `nuclear`, `piece`, `ethereal`,
  `golden` (movidos para `dlls/unused/`) e o `crowbar.cpp` stock (o mod usa `crowbar2.cpp`).
- Descartados: `inputw32.cpp`, `soundsystem.cpp` (DirectSound, nao usado), winsock do `hud_servers.cpp`,
  codigo antigo do VGUI do SDK (usa o do hlsdk-portable + FreeVGUI).

## Pode ter ficado de fora
Ajustes de balanceamento do mod em arquivos "stock" que nao foram detectados pelo merge automatico
(ex.: HP de monstros, tempos de recarga de armas stock). Confira com `diff` contra o fonte original
se alguma coisa parecer diferente do jogo no PC.
