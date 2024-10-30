---
title: 'Tests'
slug: 'tests'
description: 'aplication-tests'
tags: ["Tests"]
pubDate: 'Oct 30 2024'
coverImage: './blog-placeholder-3.jpg'
---

> Quanto mais a codebase aumenta, pequenos erros que não esperamos acabam aparecendo, um jeito de previnir isso é escrevendo testes de acordo com que a aplicação cresce.


## Porque testar?
> Somos humanos e cometemos erros, testes são importantes porque ajuda a encobrir erros e verificar se o seu código esta funcionando da maneira certa, mas o mais importante que testes ajudam seu código continuar funcionando depois que adiciona novas features.

## Estrutura de Teste

Seu teste tem que ser curto e ideal para um única coisa

```javascript
it('given a date in the past, colorForDueDate() returns red', () => { 
    expect(colorForDueDate('2000-10-20')).toBe('red'); 
});
```

- Jest oferece o describe uma função que ajuda a estrutura do teste.
- Usando o describe podemos agrupar diversos testes pertencendo uma funcionalidade.

## Testes Unitários
- Testes unitários cobrem uma pequena parte do código, como por exemplo, funções individuais.
- A grande beleza dos testes unitários é uma forma rápida de executar e escrever

## Mocking

Quando estamos escrevendo sistemas e precisamos que algumas partes interagem com outra os mocks são essenciais para simular tais componentes ou funções integradas.

Usando o mocks para escrever seus testes quer dizer que temos X que para funcionar precisa de A e B, alem dos testes unitários de A e B, precisamos criar alguns dados, funções … fakes para ter o teste integrando os dois.

## Testes de componentes
Componentes são responsáveis por renderizar o seu aplicativo, usuários estão diretamente ligado aos componentes.

Components podem falhar tanto em testes unitários quanto de integração, mas porque são partes importantes do core do React Native, que cobrem separadamente.

## O que testar em componentes?

→ Integração: quando um usuário clica em um button

→ Renderização: o button esta no lugar certo e do tamanho ou cor certo

>Se tem um button que possui um Onpress, precisamos testar tanto a sua aparência quanto a sua funcionalidade, seja ela qual for

## Testes interação de usuarios
```javascript
function GroceryShoppingList() {
	const [groceryItem, setGroceryItem] = useState('');
	const [items, setItems] = useState([]);

	const addNewItemToShoppingList = useCallback(() => {
		setItems([groceryItem, ...items]);
		setGroceryItem('');
	}, [groceryItem, items]);

	return (
		<>
			<TextInput
				value={groceryItem}
				placeholder="Enter grocery item"
				onChangeText={text => setGroceryItem(text)}
			/>

			 <Button
				 title="Add the item to list"
				 onPress={addNewItemToShoppingList}
			/> {items.map(item => (
			<Text key={item}>{item}</Text>
			))}
		</>
	);
}
```

```javascript
test('given empty GroceryShoppingList, user can add an item to it', () =>{
	const {getByPlaceholder, getByText, getAllByText} = render(
		<GroceryShoppingList />,
	);

	fireEvent.changeText(
		getByPlaceholder('Enter grocery item'), 'banana',);

		fireEvent.press(getByText('Add the item to list'));

		const bananaElements = getAllByText('banana');
		expect(bananaElements).toHaveLength(1);
		// expect 'banana' to be on the list
});
```

Quando testamos testes de usuários, devemos nos perguntar, o que esta em essa página? quais alterações esta integrado com ela?

## Testes End-to-End

Testes End-to-End verifica se seu aplicativo esta funcionando da maneira certa na perspectivas do usuário.

Objetivo da realização de testes end-to-end é identificar dependências do sistema e garantir que a informação certa seja passada entre vários componentes.

Resumindo, visa provar o sistema de uma forma mais completa simulando o ambiente real