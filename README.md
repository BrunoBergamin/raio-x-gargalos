# Raio-X dos 5 Gargalos

Landing page com **quiz de diagnóstico interativo**: em 7 minutos o visitante descobre qual dos 5
gargalos (Posicionamento, Oferta, Geração de Leads, Conversão Comercial, Processos & IA) está
travando o crescimento do negócio dele. O quiz calcula um score de 0 a 10 por pilar, aponta o
gargalo principal, estima o custo em R$, dá uma ação prática e abre o WhatsApp com o resultado.

Tudo em **HTML, CSS e JavaScript puro**, sem framework e sem build. É só abrir ou subir num
servidor estático (GitHub Pages, Vercel, etc).

## Estrutura

```
.
├── index.html              Página (hero, problema, como funciona, pilares, quiz, autor, CTA)
├── css/estilo.css          Todo o visual (dark mode)
├── js/quiz.js              Motor do quiz: perguntas, cálculo, resultado, WhatsApp e salvamento
├── js/main.js              Animações leves da landing (contadores, fade)
├── favicon.svg             Ícone da aba (lupa / raio-x)
└── google-apps-script.gs   Script pronto para salvar os leads numa planilha do Google
```

## O que editar (o essencial)

Tudo fica em [`js/quiz.js`](js/quiz.js), no topo, no objeto `CONFIG`:

1. **Número do WhatsApp** que recebe o lead. Troque `"SEU_NUMERO"` pelo seu número no formato
   internacional só com dígitos. Ex.: `5515999999999` (55 Brasil, 15 DDD, depois o número).
2. **Salvar no Google Sheets** (opcional, pode ligar depois): cole a URL do Apps Script em
   `sheetsEndpoint`. Enquanto estiver vazio (`""`), o salvamento fica desligado e só o WhatsApp
   funciona.

As **perguntas**, os **pilares** e os **textos do resultado** também estão em `js/quiz.js`, em
listas fáceis de mexer (`PERGUNTAS`, `PILARES`, `DIAGNOSTICO`).

## Salvar os leads no Google Sheets

Passo a passo (uns 5 minutos):

1. Crie uma planilha nova no Google Sheets.
2. Menu **Extensões > Apps Script**.
3. Apague o que estiver lá e cole todo o conteúdo de [`google-apps-script.gs`](google-apps-script.gs).
4. **Implantar > Nova implantação**: tipo **App da Web**, executar como **Eu**, acesso
   **Qualquer pessoa**.
5. Copie a URL gerada (termina em `/exec`) e cole em `CONFIG.sheetsEndpoint` no `js/quiz.js`.

A cada diagnóstico concluído, uma linha nova é gravada com nome, e-mail, WhatsApp, gargalo
principal, nota de cada pilar, custo estimado e origem.

## Próximo passo: banco de dados (Supabase)

A função `salvarLead()` no `js/quiz.js` é o único ponto que envia os dados. Para migrar do Sheets
para um banco **Supabase (Postgres)** no futuro, basta trocar o destino do `fetch` dentro dessa
função por uma chamada à API do Supabase (uma tabela `leads` com as mesmas colunas). O resto do
quiz não muda. O Sheets serve bem como começo enquanto o volume é baixo.

## Observação

Os números de prova social (negócios radiografados, custo estimado) são **ilustrativos** e devem
ser ajustados para a sua realidade antes de divulgar.
