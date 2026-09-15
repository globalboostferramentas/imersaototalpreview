# Workshop Atom 360 · Você sabe quanto custa a sua vida?

Landing page e página de agradecimento do workshop presencial da Atom Educacional,
em Sorocaba (SP), no sábado 17 de outubro de 2026, das 9h às 18h.

São dois arquivos HTML estáticos, sem build e sem dependência de framework.
Todo o CSS e o JavaScript vivem dentro de cada página.

## Estrutura

```
index.html      landing page do workshop
obg.html        página de agradecimento, exibida depois do envio do formulário
llms.txt        resumo do evento para modelos de linguagem (vai na raiz do domínio)
midia/          imagens e logos usados pelas duas páginas
```

## Publicação

O conteúdo da raiz deste repositório corresponde à pasta
`/workshop-despertar-financeiro/` em `lp.atomeducacional.com.br`:

```
/workshop-despertar-financeiro/index.html
/workshop-despertar-financeiro/obg.html
/workshop-despertar-financeiro/midia/...
/llms.txt                                  (raiz do domínio, opcional)
```

Os caminhos são relativos, então a pasta pode ser servida de qualquer lugar,
desde que `index.html`, `obg.html` e `midia/` continuem no mesmo nível.

Faça backup da página publicada antes de substituir.

## Integrações

| O quê | Onde | Identificador |
| --- | --- | --- |
| Google Tag Manager | as duas páginas | `GTM-52S3HK7Q` |
| Formulário HubSpot | `index.html`, dois pontos de captura | portal `48226823`, form `56f6f3da-7196-45a6-9571-773f3d03ef22` |
| WhatsApp | `obg.html`, nos dois botões | `api.whatsapp.com/send/?phone=5515996464968` com mensagem pronta |

No HubSpot, o formulário redireciona para um checkout. O `index.html` passa por cima disso
no embed e manda para o `obg.html` da mesma pasta, repetindo os parâmetros da URL (UTMs):
`redirectUrl` pede ao HubSpot o novo destino, `__INTERNAL__CONTEXT.disableRedirect` impede o
embed de seguir a URL que o servidor devolver e `onFormSubmitted` faz a navegação. Essa flag
é interna do embed e não está documentada; se um dia o envio voltar a cair no checkout, é
por aqui que se começa a olhar. O certo é também trocar o redirecionamento dentro
do HubSpot para `/workshop-despertar-financeiro/obg.html`, porque qualquer outra página
que use esse formulário continua caindo no checkout.

## Eventos enviados ao dataLayer

| Evento | Página | Parâmetros |
| --- | --- | --- |
| `envio_formulario` | index | `posicao`: `topo` ou `final` |
| `clique_cta` | index | `posicao`: `masthead`, `hero`, `investimento`, `barra-fixa` |
| `clique_whatsapp` | obg | `posicao`: `topo` ou `rodape`, `pagina`: `obrigado` |

## Dados do evento em um lugar só

Data, horário, cidade, condição e programa aparecem em vários pontos das
páginas. Ao mudar qualquer um deles, procure também em:

- `<title>`, `meta description` e as tags `og:` do `index.html`
- o bloco `application/ld+json` do `index.html`, que repete data, condição e palestrantes
- a barra fixa de celular e o cabeçalho
- `llms.txt`
- `obg.html`, na ficha lateral e no texto do link de WhatsApp

## Acessibilidade e movimento

As animações de entrada dependem de `IntersectionObserver` e são desligadas por
completo quando o visitante tem `prefers-reduced-motion: reduce`. Sem JavaScript,
todo o conteúdo aparece normalmente.

## Valores

As páginas não exibem preço. Por decisão do cliente, a comunicação é apenas "mais de 50% de
desconto", e o valor com as formas de pagamento fica com a equipe, no contato depois do
formulário. Por isso o `Offer` do JSON-LD não traz `price` nem `priceCurrency`.

## Aviso legal

O rodapé do `index.html` carrega o aviso da Atom sobre natureza educacional do
conteúdo, risco em renda variável e ausência de garantia de resultado. Esse texto
é jurídico: não edite sem passar pelo time responsável.
