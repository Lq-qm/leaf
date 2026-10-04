# leaf

```
⠀⠀⠀⠀⠀⠀⠀⠀⣀⣠⣤⣤⣄⣀⣀⡀⠀⠀⠀
⠀⠀⠀⠀⠀⢀⣶⣿⣿⣿⣿⣿⣿⣿⣿⣿⣷⣶⠶
⠀⠀⠀⠀⢠⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⡿⠃⠀
⠀⠀⠀⢀⣿⣿⣿⣿⣿⣿⣿⣿⣿⡿⠋⠀⠀⠀
⢀⣠⠞⠋⠉⠛⠻⠿⣿⣿⣿⠿⠟⠋⠀⠀⠀⠀⠀
⠞⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
```

Script de limpeza simples para sistemas Linux (Arch/Manjaro,
Debian/Ubuntu, Fedora/RHEL).

Remove caches de ferramentas de pacotes e linguagens, caches de
navegadores e arquivos seguros de limpar. Ao final da execucao, exibe a
soma do espaco total liberado.

## Requisitos

- bash
- sudo

As demais ferramentas (uv, pip, pacman, yay, apt, dnf, flatpak) sao
detectadas em runtime: se nao estiverem instaladas, a secao
correspondente e pulada.

## Uso

```sh
./leaf
```

Sem confirmação na lixeira:

```sh
./leaf -y
```

## Opções

| Flag       | Descrição                                        |
|------------|--------------------------------------------------|
| -y, --yes  | Não pede confirmação para esvaziar a lixeira     |

## O que é limpo

| Alvo             | Detalhe                                                          |
|------------------|------------------------------------------------------------------|
| uv               | `uv cache clean`                                                 |
| pip              | `pip cache purge`                                                |
| pacman           | pacotes órfãos + `sudo pacman -Sc`                                |
| yay              | `yay -Sc`                                                        |
| apt              | `sudo apt-get clean` (Debian/Ubuntu)                             |
| dnf              | `sudo dnf clean all` (Fedora/RHEL)                               |
| rpm              | apenas verificação: não possui cache próprio (gerenciado pelo dnf/yum) |
| Brave            | Cache, Code Cache, CacheStorage                                  |
| Firefox          | cache2, startupCache, storage/default (todos os perfis)          |
| Chrome           | Cache, Code Cache, CacheStorage (todos os perfis)                |
| journal          | `journalctl --vacuum-time=7d` (mantém os últimos 7 dias)         |
| temporários      | `/tmp` e `/var/tmp`, apenas arquivos com mais de 1 dia           |
| lixeira          | `~/.local/share/Trash` (com confirmação, exceto com -y)          |
| miniaturas       | `~/.thumbnails` e `~/.cache/thumbnails`                          |
| cache do usuário | `~/.cache`                                                       |
| flatpak          | runtimes não utilizados (`flatpak uninstall --unused`)           |

## Personalização

O cabeçalho do script pode ser personalizado via variáveis de ambiente:

| Variável      | Padrão                                   |
|---------------|------------------------------------------|
| LEAF_TITLE    | leaf - limpeza do sistema                |
| LEAF_SUBTITLE | caches - pacotes - navegadores - temporários |
| LEAF_ART      | arte da folha (vazio para ocultar)       |

Exemplo:

```sh
LEAF_TITLE="meu limpa" LEAF_SUBTITLE="feito por mim" ./leaf
LEAF_ART="" ./leaf   # sem arte no cabeçalho
```

## Observações

- Feche os navegadores antes de executar para garantir que nenhum
  arquivo de cache esteja em uso.
- Caches e miniaturas são regenerados automaticamente pelas aplicações.
- O total de espaço liberado soma apenas o que foi medido em bytes
  (diretórios, temporários, journal). As limpezas de pacotes (uv, pip,
  pacman, yay, flatpak) não entram na soma.
- O uso de `sudo` é exigido nas seções de pacman, journal e
  temporários.
