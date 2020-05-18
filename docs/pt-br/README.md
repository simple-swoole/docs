# Simps

Simps é um framework PHP minimalista baseado em corotinas usando o `Swoole 4.4 +`. Comparado com outros frameworks, ele 
não possui muitos pacotes e somente alguns arquivos simples.

Simps não fornece componentes comuns, devido à existência do `composer`, uma ferramenta para o gerenciamento de 
dependências do `PHP`. Usando o `composer` é possível baixar componentes prontos e customizar o framework de acordo com 
as suas necessidades para o projeto.

## Intenção Original

O projeto não precisa usar um framework com excesso de peso, o Swoole nativo é suficiente. Como começar a usar o Swoole
diretamente? Esse framework nasceu para responder essa pergunta.

Tomando o servidor HTTP `Swoole` como exemplo, alguns usuários não sabem como iniciar quando veem o código de exemplo no
documento; o servidor `HTTP` é relativamente simples e é necessário apenas prestar atenção à resposta da solicitação, 
portanto, apenas é necessário escutar um evento `onRequest`. Este evento é acionado quando uma nova solicitação `HTTP` 
é recebida, mas ainda há usuários que não sabem como resolver a rota ao usá-la.

Esse framework fornece a função de resolução de rota, os usuários precisam apenas prestar atenção às regras de negócio.

## Origem do nome

`Simple + Swoole = Simps`，Simples `Swoole` framework.