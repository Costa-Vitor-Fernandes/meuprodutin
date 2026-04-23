# Requisitos do Produto (Product Requirements)

## 1. Atores do Sistema
- **Criador (Seller):** Usuário que cria uma loja, faz upload de produtos, recebe pagamentos quando vende, e tem acesso as informações de contato dos compradores.
- **Comprador (Buyer):** Usuário que acessa a loja do criador, faz login e efetua a compra.

## 2. Produtos e Armazenamento
- O sistema suporta dois tipos de produtos digitais:
  1. **Arquivos para Download:** Armazenados em buckets S3 (Cloudflare R2/AWS S3). O limite de tamanho por upload respeitará o limite gratuito do nosso provedor.
  2. **Links Externos:** Redirecionamento ou acesso a um link secreto pós-compra.
- **Preços:** Apenas produtos pagos. Não haverá suporte para produtos 100% gratuitos ou formato "Pay what you want" no MVP.
- **Proteção contra Pirataria:** No MVP, a abordagem será simples. As URLs de download expiram depois de um tempo (S3 Presigned URLs), mas não haverá bloqueios complexos (DRM) que impeçam o comprador de compartilhar o arquivo físico após o download.

## 3. Autenticação e Conta
- **Social Login:** Acesso via provedores sociais (Google, Apple, etc.)
- **Login Obrigatório:** O comprador deve obrigatoriamente realizar o login (ou criar conta via Social Auth) para concluir o checkout. Isso garante o acesso vitalício às compras na dashboard do comprador.

## 4. Multi-tenant (Lojas e Domínios)
- O criador escolhe um "slug" único que vira um subdomínio (`[slug].meuprodutin.com`).
- Não haverá suporte para domínios próprios customizados (`www.lojadocriador.com`). Tudo ficará sob o domínio principal da plataforma.
- O mapeamento de subdomínios será feito via middleware.

## 5. Pagamentos e Financeiro
- **Gateway:** Stripe Connect.
- **Método:** Destination Charges (MEUPRODUTIN processa o pagamento e transfere a parte do criador automaticamente).
- **Taxas:** As taxas de processamento de cartão (Stripe) são absorvidas pelo *Criador* (descontadas do montante dele).
- **Monetização da Plataforma:** No MVP, a MEUPRODUTIN cobrará uma taxa em porcentagem sobre cada venda (Application Fee).
- **Planos Futuros:** A plataforma prevê cobrar mensalidade de criadores (lojas) a partir de um certo limite de upload/armazenamento de arquivos.
- **Estornos (Chargebacks):** Seguirá o padrão do Stripe Connect (Destination Charges), onde o estorno e taxas relacionadas são repassadas/debitadas do saldo do criador.
- **Tipos de Cobrança:** O MVP deve suportar tanto pagamentos únicos (One-time) quanto pagamentos recorrentes (Assinaturas).

## 6. Experiência do Criador e Loja
- **Customização:** Template padrão, com personalização mínima (foco em conversão e simplicidade de desenvolvimento).
- **Rastreamento:** Suporte essencial para inserção de Pixels e Analytics (Facebook Pixel, Google, TikTok, etc.).
- **Gestão de Clientes:** O criador terá acesso à lista de e-mails de seus compradores para ações de remarketing.

## 7. Comunicação e Administração
- **Webhooks:** Integração com Webhooks do Stripe para garantir a sincronização de pagamentos assíncronos e renovações/falhas de assinatura.
- **Administração (Super Admin):** Não haverá painel de administração no MVP. A gestão global da plataforma (banimento, conferência) será feita manualmente, acessando o banco de dados e o Dashboard do Stripe.
