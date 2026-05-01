# Mapeamento de Dados - Olist Dataset (Completo)

Este documento descreve a estrutura de **todos** os arquivos CSV do dataset original e o plano de consolidação.

## Origem dos Dados (Kaggle)
**URL:** [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

### 1. olist_customers_dataset.csv
- `customer_id`: Chave primária (id do cliente por pedido).
- `customer_unique_id`: Identificador único do cliente (não muda entre pedidos).
- `customer_zip_code_prefix`: Primeiros 5 dígitos do CEP.
- `customer_city`: Cidade.
- `customer_state`: Estado.

### 2. olist_geolocation_dataset.csv
- `geolocation_zip_code_prefix`: Prefixo do CEP.
- `geolocation_lat`: Latitude.
- `geolocation_lng`: Longitude.
- `geolocation_city`: Cidade.
- `geolocation_state`: Estado.

### 3. olist_order_items_dataset.csv
- `order_id`: ID do pedido.
- `order_item_id`: Número sequencial identificando a quantidade de itens no mesmo pedido.
- `product_id`: ID do produto.
- `seller_id`: ID do vendedor.
- `shipping_limit_date`: Data limite para envio pelo vendedor.
- `price`: Preço do item.
- `freight_value`: Valor do frete do item.

### 4. olist_order_payments_dataset.csv
- `order_id`: ID do pedido.
- `payment_sequential`: Sequencial de pagamentos (um pedido pode ter vários cartões, etc).
- `payment_type`: Método (credit_card, boleto, voucher, etc).
- `payment_installments`: Número de parcelas.
- `payment_value`: Valor total pago.

### 5. olist_order_reviews_dataset.csv
- `review_id`: ID único da avaliação.
- `order_id`: ID do pedido.
- `review_score`: Nota de 1 a 5.
- `review_comment_title`: Título do comentário.
- `review_comment_message`: Texto do comentário.
- `review_creation_date`: Data de criação.
- `review_answer_timestamp`: Data da resposta.

### 6. olist_orders_dataset.csv
- `order_id`: ID único do pedido.
- `customer_id`: FK para a tabela de clientes (um por pedido).
- `order_status`: Status (created, approved, invoiced, shipped, delivered, unavailable, canceled).
- `order_purchase_timestamp`: Data da compra.
- `order_approved_at`: Data de aprovação do pagamento.
- `order_delivered_carrier_date`: Data de entrega para a transportadora.
- `order_delivered_customer_date`: Data de entrega ao cliente.
- `order_estimated_delivery_date`: Data estimada de entrega.

### 7. olist_products_dataset.csv
- `product_id`: ID único do produto.
- `product_category_name`: Nome da categoria em português.
- `product_name_lenght`: Comprimento do nome.
- `product_description_lenght`: Comprimento da descrição.
- `product_photos_qty`: Qtd de fotos.
- `product_weight_g`: Peso em gramas.
- `product_length_cm`: Comprimento em cm.
- `product_height_cm`: Altura em cm.
- `product_width_cm`: Largura em cm.

### 8. olist_sellers_dataset.csv
- `seller_id`: ID único do vendedor.
- `seller_zip_code_prefix`: Prefixo do CEP do vendedor.
- `seller_city`: Cidade.
- `seller_state`: Estado.

### 9. product_category_name_translation.csv
- `product_category_name`: Nome em português.
- `product_category_name_english`: Nome em inglês.

## Destino Analítico e Idempotência
Para garantir que o ETL seja resiliente e idempotente, cada tabela no banco de dados analítico terá uma chave primária baseada nos IDs originais. O uso de `UPSERT` evitará duplicações em caso de múltiplas execuções do pipeline sobre o mesmo lote de dados.
