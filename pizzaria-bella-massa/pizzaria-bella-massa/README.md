# Pizzaria Bella Massa

Sistema de pedidos de pizzaria feito em **PHP + PostgreSQL**, com visão do
**cliente** e do **administrador**. Projeto simples, organizado seguindo a
separação entre front-end (HTML/CSS/JS) e back-end (PHP).

## Estrutura de pastas

```
pizzaria-bella-massa/
├── assets/
│   ├── css/        (style.css, admin.css)
│   ├── js/         (carrinho, viacep, checkout, rastreio, admin-*, ...)
│   └── img/
├── config/
│   └── database.php   (conexão PDO com o PostgreSQL)
├── includes/
│   ├── header.php     (menu do site)
│   └── footer.php     (rodapé)
├── actions/           (back-end: scripts PHP sem tela)
│   ├── login.php, cadastro.php, logout.php
│   ├── calcular_frete.php, processar_pedido.php
│   ├── status_pedido.php, atualizar_status.php
│   ├── produtos_salvar.php, produtos_excluir.php
│   ├── dados_grafico.php, gerar_pizza_ia.php
├── admin/             (painel restrito)
│   ├── dashboard.php, produtos.php, pedidos.php
├── index.php          (cardápio)
├── checkout.php       (finalizar pedido)
├── login.php, cadastro.php
├── pedido-confirmado.php
└── banco.sql          (script do banco de dados)
```

## Como rodar

### 1. Criar o banco de dados
Abra o PostgreSQL (pelo psql ou pela sua ferramenta) e crie o banco:

```sql
CREATE DATABASE pizzaria;
```

Depois conecte-se a esse banco e rode o arquivo `banco.sql`, que cria as
tabelas e já insere alguns produtos e usuários de teste.

No psql, por exemplo:
```
\c pizzaria
\i banco.sql
```

### 2. Configurar a conexão
Abra `config/database.php` e ajuste se necessário (host, porta, usuário e,
principalmente, a **senha** do seu PostgreSQL):

```php
$DB_HOST  = '127.0.0.1';
$DB_PORT  = '5432';
$DB_NOME  = 'pizzaria';
$DB_USER  = 'postgres';
$DB_SENHA = 'postgres';   // <-- troque pela sua senha
```

### 3. Rodar o servidor PHP
Dentro da pasta do projeto:

```
php -S localhost:8000
```

E acesse no navegador: http://localhost:8000

> É preciso ter o PHP com a extensão **pdo_pgsql** habilitada.

## Usuários de teste

| Perfil  | E-mail                  | Senha       |
|---------|-------------------------|-------------|
| Admin   | admin@bellamassa.com    | admin123    |
| Cliente | cliente@teste.com       | cliente123  |

## O que está implementado

Obrigatórios:
- Cardápio dinâmico (lido do banco)
- Carrinho com JavaScript (adicionar, remover, total em tempo real, localStorage)
- Integração ViaCEP no checkout
- Login/cadastro com sessões ($_SESSION) e senha com hash
- Banco PostgreSQL com as 4 tabelas e suas chaves (PK/FK)

Desejável/Opcional:
- Cálculo do frete feito no back-end (PHP) — `actions/calcular_frete.php`
- Processamento seguro do pedido (o preço é sempre recalculado no servidor)
- Área administrativa (dashboard, produtos com CRUD, gestão de pedidos)

Extras (bônus):
- **Extra 1** — Checkout via WhatsApp (link wa.me montado pelo PHP)
- **Extra 2** — Dashboard com gráfico (Chart.js + GROUP BY/SUM)
- **Extra 3** — Rastreio do pedido ao vivo (polling com setInterval)
- **Extra 4** — Gerador de sabores. Funciona localmente sem chave; se você
  colocar uma chave do Gemini em `actions/gerar_pizza_ia.php`, ele usa a IA
  de verdade via cURL.

## Observações
- As fotos dos produtos foram trocadas por emojis (🍕 / 🥤), porque a tabela
  `produtos` não tem coluna de imagem (seguindo a modelagem pedida).
- O número do WhatsApp do restaurante está como exemplo em
  `pedido-confirmado.php` (variável `$whatsappRestaurante`) — troque pelo real.
