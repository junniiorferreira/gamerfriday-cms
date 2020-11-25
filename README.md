# DT3 - Black Friday

> Página para Black Friday da DT3


## Depenências

* [`Vue`](https://vuejs.org/) - usada para controle de estado da aplicação
* [`Netlify CMS`](https://www.netlifycms.org/) - usada para gerenciar o conteúdo da página


## Principais Arquivos

* `src/assets/` - arquivos `.css`, *fontes* e *imagens*
* `src/components/` - componentes `vue` que compões a interface da aplicação
* `src/content/` - conteúdo da página (produtos, categorias e marcas)
* `src/App.vue` - Arquivo principal da aplicação


## Desenvolvimento

``` bash
# instale as dependências do projeto
$ npm install

# veja mudanças em localhost:8080
$ npm run serve

# faça o build de produção (copie o conteúdo da pasta `dist` para produção)
$ npm run build
```

## Produção

As promoções devem estar definidas no frontend e no painel no WordPress.

Ao salvar o produto, caso todos campos da promoção estejam preenchidos (Data, Hora, Preço Novo, Preço Antigo),
será agendado a modança de preço no wp-cron com 5 minutos de antecedência.

Estes eventos são agendados através da função wp_schedule_single_event que utiliza o Unix Timestamp como unidade de tempo.
[https://www.unixtimestamp.com/](https://www.unixtimestamp.com/)

### Reset dos preços

Os preços retornarão aos seus valores originais no horário informado utilizando a função:

```
wp_schedule_single_event( 1574683200 + 10848, 'dt3_stop_black_friday');
```

Onde é informado o horário atual + a correção para o fuso horário usado na instalação wordpress.

