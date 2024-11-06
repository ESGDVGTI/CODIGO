produtos = [
    {'id': 1, 'nome': 'televisao', 'categoria': 'Eletrodomésticos', 'preço': 3200.00, 'quantidade': 8},
    {'id': 2, 'nome': 'iphone 16', 'categoria': 'Telefonia', 'preço': 7500.00, 'quantidade': 5},
    {'id': 3, 'nome': 'camiseta', 'categoria': 'Vestuário', 'preço': 120.00, 'quantidade': 10},
    {'id': 4, 'nome': 'tenis', 'categoria': 'Vestuário', 'preço': 500.00, 'quantidade': 7},
    {'id': 5, 'nome': 'cama box', 'categoria': 'Móveis', 'preço': 1200.00, 'quantidade': 14}
]

# Estoque
estoque = [
    {'id': 1, 'nome': 'televisao', 'quantidade': 8},
    {'id': 2, 'nome': 'iphone 16', 'quantidade': 5},
    {'id': 3, 'nome': 'camiseta', 'quantidade': 10},
    {'id': 4, 'nome': 'tenis', 'quantidade': 7},
    {'id': 5, 'nome': 'cama box', 'quantidade': 14}
]

# Dicionário de carrinho de compras
carrinho = []

# Função para exibir opções do menu
def exibiropcoes():
    print('1 - Adicionar item')
    print('2 - Remover item')
    print('3 - Exibir carrinho')
    print('4 - Finalizar compra')
    print('5 - Sair')

# Função para exibir todos os produtos disponíveis
def exibirsprodutos():
    for p in produtos:
        print(f"ID: {p['id']} - Nome: {p['nome']} - Preço: R${p['preço']} - Estoque: {p['quantidade']}")

# Função para atualizar o estoque após a compra
def atualizar_estoque(id, quantidade_comprada):
    for item in estoque:
        if item['id'] == id:
            if item['quantidade'] >= quantidade_comprada:
                item['quantidade'] -= quantidade_comprada
                return True
            else:
                print(f"Estoque insuficiente para o produto: {item['nome']}.")
                return False
    return False

# Função para calcular o total do carrinho, incluindo o desconto se aplicável
def calcular_total():
    somatorio = 0
    for item in carrinho:
        for produto in produtos:
            if produto['id'] == item['id']:
                somatorio += produto['preço'] * item['quantidade']
                break

    # Aplicar desconto de 10% se o total for maior que R$ 500
    if somatorio > 500:
        desconto = somatorio * 0.10
        somatorio -= desconto
        print(f"Desconto de 10% aplicado. Total com desconto: R${somatorio:.2f}")
    else:
        print(f"Total sem desconto: R${somatorio:.2f}")
    
    return somatorio

# Menu de interações
opcao = '1'

print('Carrinho\n')

# Loop principal do sistema
while opcao != '5':
    exibiropcoes()
    opcao = input('Escolha uma opção: ')

    if opcao == '1':
        exibirsprodutos()
        id_produto = int(input('Digite o ID do produto para adicionar: '))
        quantidade = int(input('Digite a quantidade: '))
        
        # Verificar se o produto está no estoque suficiente
        for produto in produtos:
            if produto['id'] == id_produto:
                if produto['quantidade'] >= quantidade:
                    carrinho.append({'id': id_produto, 'quantidade': quantidade})
                    print(f'Produto "{produto["nome"]}" adicionado ao carrinho.')
                else:
                    print(f"Estoque insuficiente para o produto: {produto['nome']}.")
                break

    elif opcao == '2':
        print("Produtos no carrinho:")
        for item in carrinho:
            for produto in produtos:
                if produto['id'] == item['id']:
                    print(f"ID: {produto['id']} - {produto['nome']} - Quantidade: {item['quantidade']}")
                    break

        id_produto_remover = int(input('Digite o ID do produto para remover: '))
        for item in carrinho:
            if item['id'] == id_produto_remover:
                # Recuperar o nome do produto antes de remover
                for produto in produtos:
                    if produto['id'] == item['id']:
                        nome_produto = produto['nome']
                        break
                carrinho.remove(item)
                print(f'Produto "{nome_produto}" removido do carrinho.')
                break

    elif opcao == '3':
        if carrinho:
            print("Produtos no carrinho:")
            for item in carrinho:
                for produto in produtos:
                    if produto['id'] == item['id']:
                        print(f"Nome: {produto['nome']} - Quantidade: {item['quantidade']} - Preço unitário: R${produto['preço']} - Subtotal: R${produto['preço'] * item['quantidade']:.2f}")
                        break
            # Calcular e exibir total
            calcular_total()
        else:
            print("Carrinho vazio!")

    elif opcao == '4':
        if carrinho:
            print("Finalizando compra...")
            total = calcular_total()
            sucesso = True
            # Atualizar estoque com os itens comprados
            for item in carrinho:
                if not atualizar_estoque(item['id'], item['quantidade']):
                    sucesso = False
                    break
            
            if sucesso:
                print(f"Compra finalizada com sucesso! Total: R${total:.2f}")
                print("Estoque atualizado:")
                for item in estoque:
                    print(f"{item['nome']} - Quantidade restante: {item['quantidade']}")
                carrinho.clear()  # Limpar carrinho para nova compra
            else:
                print("Erro ao finalizar a compra. Verifique o estoque.")
        else:
            print("Carrinho vazio! Não é possível finalizar a compra.")
        
    elif opcao == '5':
        print("Saindo...")
        break
    else:
        print("Opção inválida! Tente novamente.")
