# vendor

Bibliotecas de terceiros servidas pelo proprio dominio, em vez de CDN.

| arquivo | versao | origem |
|---|---|---|
| `bootstrap-4.6.2.min.css` | 4.6.2 | `https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css` |
| `sweetalert-2.1.2.min.js` | 2.1.2 | `https://cdn.jsdelivr.net/npm/sweetalert@2.1.2/dist/sweetalert.min.js` |

Sobrou so o **CSS** do Bootstrap. O JavaScript dele e o jQuery foram removidos,
veja abaixo.

## Por que sairam do CDN

O site carregava Bootstrap 4.5.2 e o JS do Bootstrap de `stackpath.bootstrapcdn.com`,
jQuery 3.5.1 de `code.jquery.com` e o SweetAlert de `unpkg.com/sweetalert`, **sem
numero de versao**. Tres problemas, resolvidos de uma vez ao trazer os arquivos:

1. A URL do SweetAlert sem versao entregava o que estivesse publicado no npm no
   momento do acesso. Qualquer publicacao nova passava a rodar no site sozinha.
2. Nenhuma tag tinha `integrity`, entao qualquer um desses hosts podia executar
   codigo aqui. Servindo do proprio dominio, o arquivo e exatamente o que esta
   neste repositorio.
3. O servico de CDN da StackPath foi descontinuado. O host ainda responde, mas a
   dependencia tinha prazo de validade.

Como bonus, a CSP em `_headers` voltou a ser `'self'`: nenhum host externo
precisa estar liberado.

## jQuery e o JS do Bootstrap sairam

Os dois existiam **so** para alternar as abas Fila e Pilha da pagina inicial. O
codigo do site nunca chamou jQuery diretamente. Eram 129 KB (61 do Bootstrap, 68
do jQuery) carregados em toda visita para trocar duas abas.

A troca agora e um bloco de ~25 linhas no fim da `index.html`: tira `active` da
aba e do painel anteriores, poe na nova, e adiciona `show` no quadro seguinte
para a transicao de opacidade acontecer. O fade continua sendo do CSS do
Bootstrap, que ficou.

O atributo `data-toggle="tab"` saiu do markup junto, porque nao havia mais nada
lendo ele. As classes (`nav-tabs`, `tab-pane`, `fade`) e os atributos ARIA
continuam: as primeiras sao estilo, os segundos sao acessibilidade.

## Como atualizar

Baixar do jsdelivr na versao nova, trocar o arquivo, ajustar o nome nos HTML e
rodar a validacao antes de publicar. Versoes ficam no nome do arquivo de
proposito: assim o diff de uma atualizacao e obvio no histórico.
