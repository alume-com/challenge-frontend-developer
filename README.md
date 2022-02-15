# Challenge Frontend Developer
Desafio técnico para seleção de desenvolvedores frontend para a Alume.

## O que deve ser feito?
Você trabalha para uma empresa que faz reservas de espaços em foguetes para cargas e passageiros. Ela criou um acordo com a SpaceX e precisa receber pedidos de passagens para um vôo. **O seu papel, como desenvolvedor frontend, é apenas criar uma página com um formulário onde o usuário pode selecionar um vôo e enviar os seus dados para comprar a passagem.**

O desafio deve ser feito utilizando React e Typescript.
  
## Requisitos
### Criar uma página `/buy-ticket`
Você deve, dentro de um projeto React, criar uma rota para a página `buy-ticket` utilizando [React Router](https://v5.reactrouter.com/web/guides/quick-start).

Em resumo, a página deve ler os dados de lançamentos e renderizar um formulário onde o usuário possa colocar as informações de compra. Se os dados passarem pela validação corretamente devem ser enviados para um servidor a ser indicado mais abaixo.

Ela deve ser feita utilizando a biblioteca de componentes [Material UI](https://mui.com/).

A responsividade da página deve ser feita utilizando o componente de [Grid](https://mui.com/components/grid/#responsive-values).

A página deve mostrar um loading que indique o carregamento e envio dos dados e que cubra a página inteira utilizando o component [Backdrop](https://mui.com/components/backdrop/).

### Ler [o endpoint de lançamentos futuros da Space Devs](https://ll.thespacedevs.com/2.2.0/swagger#operations-launch-launch_upcoming_list)
A página deve carregar os dados do endpoint da Space Devs, filtrando por alguns parâmetros: `include_suborbital=true`, para que inclua lançamentos suborbitais e `lsp__name=spacex`, para que sejam recebidos apenas os lançamentos da SpaceX. Você pode usar quaisquer bibliotecas que tiver mais experiência para carregar os dados deste endpoint (Exemplo: Axios).

Uma vez carregados os dados, deve-se **filtrar os lançamentos para apenas aqueles que acontecerão nos próximos 6 meses, baseado no parâmetro `window_start`** e **mapear cada lançamento para um objeto no mesmo formato abaixo**:

```json
{
  "label": "Dragon CRS-2 SpX-23, em 10/02/2022 de Kennedy Space Center, FL, USA",
  "id": 1
}
```

As propriedades dos dados carregados que você usará no mapeamento são:

* **id**: Dentro do objeto `mission`, a propriedade `id` (`mission.id`);
* **name**: Dentro do objeto `mission`, a propriedade `name` (`mission.name`);
* **window_start**: No próprio objeto do lançamento, a propriedade `window_start` (`window_start`);
* **from**: Dentro do objeto `pad` e depois dentro do objeto `location`, a propriedade `name` (`pad.location.name`);

**Lembrando que a propriedade `window_start` deve estar no formato pt-BR**. Você deve usar [a feature de locale do Luxon para fazer isso](https://moment.github.io/luxon/#/formatting?id=intl-1).

Esses dados mapeados serão usados no primeiro campo do formulário, um [campo de autocomplete](https://mui.com/components/autocomplete/#combo-box) em que o usuário vai poder selecionar o vôo para compra. Ele deve ser obrigatório, selecionar apenas uma das opções e ter o name `mission_id`.

### Criar o formulário
Dentro da página deve ser criado um formulário simples que receba as informações de compra. O primeiro campo nós já descrevemos acima: Um autocomplete onde o usuário selecionará um único vôo da lista de próximos vôos da SpaceX carregados do endpoint.

Os demais campos são:

* **Nome completo do passageiro**: Um [campo de texto](https://mui.com/components/text-fields/) com name `fullname`, limite de 100 caracteres e obrigatório;
* **CPF**: Um [campo de texto](https://mui.com/components/text-fields/) do tipo `number` com name `identification`, máscara do formato de CPF e obrigatório;
* **Peso**: [campo de texto](https://mui.com/components/text-fields/) do tipo `number` com name `weight`, `endAdornment` escrito "gramas", limite de cinco caracteres e obrigatório;
* **Problemas de saúde**: Um [campo de texto com múltiplas linhas](https://mui.com/components/text-fields/#multiline) com name `health_problems` e limite de 500 caracteres;
* **Aceite**: Um [checkbox](https://mui.com/components/checkboxes/) obrigatório com o seguinte label: "Garanto que os dados indicados aqui são verdadeiros e que a inclusão ou omissão de dados podem causar o cancelamento da passagem."

Todos os campos de texto devem ter variante `outlined`;

No final, haverá um [botão](https://mui.com/components/buttons/) de variante `contained` com label "Finalizar compra" que envia os dados para o servidor. Deve ser enviado uma chamada para `https://alume-teste.free.beeceptor.com/buy-ticket` com o método `POST` e os dados do formulário no body no seguinte formato:

```json
{
  "mission_id": 1234,
  "fullname": "José Silva",
  "identification": "863.425.906-48",
  "weight": 8200,
  "health_problems": "Hérnia de disco e pressão alta"
}
```

### Adicionar validação
Nós recomendamos utilizar as biliotecas [Formik](https://formik.org/) e [Yup](https://github.com/jquense/yup) para gerência de estado do formulário e validação pela simplicidade deles, mas podem ser utilizadas outras bibliotecas que esteja familiarizado.

Os erros de validação devem ser mostrados no próprio campo em que há o erro e o botão de finalizar compra deve estar desabilitado enquanto o formulário não é válido.

### Criar um modal de sucesso no envio dos dados
Após o envio o endpoint vai retornar `HTTP STATUS 201` para indicar que o recurso foi criado com sucesso. Quando for recebida essa resposta a página deve mostrar um [Alert Dialog](https://mui.com/components/dialogs/#alerts) com a mensagem de sucesso: "Compra efetuada com sucesso".

## Recursos a serem usados

### [Material UI](https://mui.com/)
Biblioteca de componentes baseada no Material Design, da Google.  

### [Luxon](https://github.com/moment/luxon)
O Luxon é uma biblioteca feita pelo mesmo time do MomentJS. Depois da depreciação deste, Luxon se tornou a biblioteca recomendada para manipulação de data e hora.

### [Lauch Library 2.2](https://ll.thespacedevs.com/2.2.0/swagger#operations-launch-launch_upcoming_list)
API aberta e gratuita feita e atualizada por fãs de exploração espacial. Tem uma série de recursos sobre lançamentos, bases de lançamento, programas espaciais, etc. No nosso teste usaremos um único endpoint, `GET /launch/upcoming/`, [cuja documentação você pode acessar clicando aqui](https://ll.thespacedevs.com/2.2.0/swagger#operations-launch-launch_upcoming_list).

### [Formik](https://formik.org/)
Biblioteca para formulários em React. Adiciona uma série de facilidades.

### [Yup](https://github.com/jquense/yup)
Biblioteca para validação de dados. Integra nativamente com o Formik e tem muitas validações prontas.

## Envio do teste

Você pode enviar o link para o seu repositório para developers@alume.com. Caso tenha dúvidas sobre algum aspecto do teste você pode enviá-las para o mesmo e-mail.
