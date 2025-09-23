# Script de População do Banco de Dados (Seed)

## Visão Geral

O script de seed é um comando Django que popula o banco de dados com dados iniciais para a API Rotas da Ibiapaba. Ele cria categorias, localizações, estabelecimentos e usuários de exemplo.

## Requisitos

- Projeto Django configurado
- Banco de dados configurado
- Acesso aos comandos de gerenciamento do Django

## Como Usar

Execute o seguinte comando na raiz do projeto:

```bash
python manage.py seed
```

## O que o Script Faz

1. **Limpa dados existentes**:

   - Remove todos os estabelecimentos
   - Remove todas as categorias
   - Remove todas as localizações
   - Remove todas as mídias sociais
   - Remove usuários não administradores

2. **Cria categorias básicas**:

   - Hotel
   - Restaurante
   - Ponto Turístico

3. **Cria estabelecimentos de exemplo**:
   - Hotel Vista Linda (Viçosa do Ceará)
   - Restaurante Sabor da Serra (Tianguá)

Cada estabelecimento inclui:

- Conta de usuário
- Detalhes de localização
- Informações de mídia social
- Associações de categoria

## Dados Criados

### Categorias

- Hotel (ID: 1)
- Restaurante (ID: 2)
- Ponto Turístico (ID: 3)

### Localizações

- Viçosa do Ceará

  - CEP: 62300000
  - Endereço: Rua Principal, 100, Centro

- Tianguá
  - CEP: 62320000
  - Endereço: Avenida Central, 250, Centro

### Estabelecimentos

1. Hotel Vista Linda

   - Usuário: hotelvistalinda
   - Categorias: Hotel, Ponto Turístico
   - Localização: Viçosa do Ceará
   - Instagram: @hotelvistalinda
   - WhatsApp: 88912345678

2. Restaurante Sabor da Serra
   - Usuário: sabordaserra
   - Categoria: Restaurante
   - Localização: Tianguá
   - Instagram: @restaurantebomgosto
   - WhatsApp: 88987654321

## Observações

- Todos os usuários criados têm a senha: `password123`
- Executar o comando de seed apagará os dados existentes antes de criar novos registros
- O script é destinado apenas para desenvolvimento e testes
