# Desafio: Sistema de Importação e Relatórios de Vendas

## Contexto

A empresa recebe arquivos de vendas de diferentes marketplaces e precisa processar esses dados para gerar relatórios gerenciais. Cada venda pode conter múltiplos produtos (itens).

## Requisitos Funcionais

### 1. Importação de Arquivos

O sistema deve ler arquivos nos formatos CSV e JSON contendo dados de vendas.

**Estrutura de uma venda:**
- ID da venda
- Data da venda
- Marketplace (nome da plataforma)
- Lista de itens, onde cada item contém:
  - Nome do produto
  - Quantidade
  - Valor unitário

**Exemplo CSV (marketplace-a.csv):**
```csv
venda_id,data,marketplace,produto,quantidade,valor_unitario
V001,2024-01-15,MercadoLivre,Notebook,2,2500.00
V001,2024-01-15,MercadoLivre,Mouse,5,45.00
V002,2024-01-16,MercadoLivre,Teclado,1,350.00
```

**Exemplo JSON (marketplace-b.json):**
```json
[
  {
    "sale_id": "V003",
    "date": "2024-01-15",
    "platform": "Premium",
    "items": [
      {"product": "Monitor", "qty": 3, "unit_price": 800.00},
      {"product": "Webcam", "qty": 3, "unit_price": 250.00}
    ]
  },
  {
    "sale_id": "V004",
    "date": "2024-01-16",
    "platform": "Premium",
    "items": [
      {"product": "Cadeira Gamer", "qty": 15, "unit_price": 1200.00}
    ]
  }
]
```

**Observação:** Note que os formatos têm nomenclaturas diferentes para os mesmos conceitos. Seu código deve normalizar essas diferenças.

### 2. Processamento e Regras de Negócio

Para cada venda, o sistema deve:

1. **Calcular o subtotal de cada item** (quantidade × valor unitário)
2. **Calcular o valor bruto total da venda** (soma de todos os subtotais)
3. **Aplicar descontos** (se aplicável):
   - Vendas com mais de 10 unidades no total (somando todos os itens) têm 3% de desconto sobre o valor bruto
4. **Calcular o valor após desconto**
5. **Aplicar taxas administrativas**:
   - Vendas com valor superior a R$ 1.000,00 têm 5% de taxa administrativa
   - Marketplace "Premium" sempre tem taxa de 8%
   - As taxas são aplicadas sobre o valor após desconto
6. **Calcular o valor líquido final** (valor após desconto + taxas)

**Exemplo de cálculo:**
```
Venda V001 (MercadoLivre):
- Item 1: 2 × R$ 2.500,00 = R$ 5.000,00
- Item 2: 5 × R$ 45,00 = R$ 225,00
- Valor bruto: R$ 5.225,00
- Quantidade total: 7 unidades (não aplica desconto)
- Valor após desconto: R$ 5.225,00
- Taxa administrativa (> R$ 1.000): 5% = R$ 261,25
- Valor líquido: R$ 5.486,25
```

### 3. Relatórios

O sistema deve gerar um relatório contendo:

- **Total de vendas por marketplace** (quantidade de vendas e valor líquido total)
- **Produto mais vendido** (em quantidade total de unidades)
- **Receita líquida total** (soma de todos os valores líquidos)
- **Detalhamento de cada venda processada** (ID, data, marketplace, valor bruto, descontos aplicados, taxas, valor líquido)

O relatório deve poder ser exportado em formato **TXT** ou **JSON** (escolha do desenvolvedor ou ambos).

### 4. Requisitos Técnicos

- Usar **Composer** para autoloading (PSR-4)
- Código deve ser executável via **linha de comando**
- Tratar erros apropriadamente:
  - Arquivo não encontrado
  - Formato de arquivo inválido
  - Dados inconsistentes (campos obrigatórios vazios, valores negativos, etc.)
- O processamento deve acontecer **em memória** (sem necessidade de banco de dados)
- Organizar o código em classes seguindo princípios de orientação a objetos

## Entregáveis

1. **Código fonte** organizado em diretórios apropriados
2. **composer.json** configurado corretamente
3. **README.md** explicando:
   - Como instalar dependências
   - Como executar o sistema
   - Exemplos de uso
4. **Pelo menos 2 arquivos de exemplo** (1 CSV e 1 JSON) com dados de teste

## Critérios de Avaliação

- Organização e estrutura do código
- Uso apropriado de orientação a objetos (classes, composição, encapsulamento)
- Clareza e legibilidade do código
- Tratamento adequado de erros
- Qualidade da documentação (README)
- Facilidade de execução

## Observações

- Você tem total liberdade para definir a arquitetura e estrutura do projeto
- Não existe uma única solução correta
- Pense em como o código seria mantido e expandido no futuro
- Considere boas práticas de programação e convenções do PHP

## Dicas

- Comece identificando as principais entidades do domínio
- Pense em como diferentes partes do sistema se relacionam
- Teste seu código com os arquivos de exemplo antes de entregar
