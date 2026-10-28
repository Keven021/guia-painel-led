# Guia de montagem – painel de LED

Aplicação web de uso interno que calcula as peças necessárias para montar painéis e colunas de LED e desenha os diagramas de montagem.

Você informa as faces (largura × altura em cm) e a aplicação devolve a lista de peças, os diagramas de receiving e de fontes e uma visão 3D da coluna. Não precisa de instalação, build nem servidor: é um único arquivo `index.html`.

## O que a aplicação faz

- **Faces:** de 1 a 4 faces, cada uma com suas próprias medidas. Cada face é calculada como um painel independente.
- **Lista de peças:** total da coluna e quebra por face, com reserva configurável (%).
- **Diagrama de receiving e cabos:** receiving centralizado no bloco, cabos flat em estrela, jump entre receiving com patch cord e ligação ao NUC.
- **Diagrama de fontes:** cabos DC em pares de módulos, cabo DC extra para o receiving (em laranja) e jump entre fontes com cabo PP.
- **Visão 3D da coluna:** faces na proporção real, alinhadas pela base. Arraste para girar.
- **Impressão:** botão para imprimir ou salvar em PDF.

## Regras de cálculo (configuráveis em "Regras do modelo")

| Item | Regra padrão |
| --- | --- |
| Módulo | 32 × 16 cm |
| Fonte | 1 para cada 8 módulos (máximo de 4 cabos DC, 2 módulos por cabo) |
| Receiving | 1 para cada 12 módulos, a no máximo 80 cm de cada módulo (se passar, entra mais um receiving) |
| Cabo flat | 1 por módulo, em 25, 40 ou 80 cm, escolhido pela distância até o receiving |
| Cabo DC dos módulos | 1 para cada 2 módulos |
| Cabo DC do receiving | 1 por receiving, saindo de uma fonte |
| Patch cord 1,5 m | 1 por receiving + 1 para o NUC |
| Cabo PP 1 m | 1 por fonte (jump entre fontes) |
| Ima, painel de 1 face | Ima 0mm, 4 por módulo |
| Ima, coluna (2 a 4 faces) | Faces ímpares Ima 4mm, faces pares Ima 10mm, 4 por módulo |
| Medida que não fecha em módulos | Arredonda para cima (ou para baixo) e avisa |

## Como usar

1. Em **Faces da coluna**, informe largura e altura da face 1. As setas dos campos andam pela medida do módulo.
2. Clique em **+ Adicionar face** para colunas com mais faces.
3. Confira a lista de peças, os avisos amarelos, os diagramas e a visão 3D.
4. Use **Imprimir / salvar PDF** para levar o guia para a bancada.

## Rodar localmente

Abra o `index.html` no navegador. Se quiser um servidor local:

```bash
python3 -m http.server 8000
# abra http://localhost:8000
```

A fonte (IBM Plex Sans) vem do Google Fonts. Sem internet, a página usa a fonte padrão do sistema.

## Publicar no GitHub Pages

1. Crie um repositório e envie estes arquivos.
2. Em **Settings → Pages**, escolha **Deploy from a branch**, branch `main`, pasta `/ (root)`.
3. Em alguns minutos o guia fica disponível em `https://SEU-USUARIO.github.io/NOME-DO-REPOSITORIO/`.

## Estrutura do código

Tudo está em `index.html`: HTML, CSS e JavaScript sem dependências.

- `calc(W, H)`: calcula uma face (módulos, grupos de fonte e receiving, cabos).
- `best(...)`, `place(...)`: escolhem e posicionam os blocos de receiving e fonte.
- `render()`: monta lista de peças, avisos, diagramas e grupos.
- `drawDiagram(...)`, `draw3d(...)`: diagramas em SVG e visão 3D em CSS.

## Próximos passos

- Cadastro de vários modelos de painel, cada um com suas regras.
- PDF do guia com uma página por face.
- Salvar e compartilhar guias.
