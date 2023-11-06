# Convenções e boas práticas do React TypeScript

A seguir, listamos algumas convenções e boas práticas para seus projetos React TypeScript:

## ViteJS para criar um projeto React TypeScript

Use [ViteJS](https://vitejs.dev/) para criar um projeto React TypeScript. Você encontrará muitos blogs, cursos e vídeos que usam o _Create React App_, mas essa ferramenta já está obsoleta.

## Stack tecnológica

Especificamente para os projetos Movie Challenge e Burger Queen API Client, recomenda-se a seguinte stack tecnológica, embora esta possa ser a de qualquer projeto React TypeScript:

- ViteJs para construir o projeto.
- Jest e React Testing Library para testes de componentes.
- React Router DOM para as rotas da SPA.
- ESLint para linting
- Prettier para formatação dos códigos 
## Estrutura de pastas

Especificamente para os projetos Movie Challenge e Burger Queen API Client, recomenda-se a seguinte estrutura de pastas, embora esta possa ser a de qualquer projeto React TypeScript:

- _src/components_: para abrigar componentes do React.
- _src/services_: para serviços responsáveis por obter dados das APIs.
- _src/styles_: para armazenar estilos globais ou utilitários de estilo.
- _src/utils_: para funções de utilidade que não se encaixam em outras categorias.
- _src/assets_: para armazenar ativos como imagens, fontes e ícones.
- _src/models_: para armazenar tipos comuns e interfaces usadas em toda a aplicação.
- _src/hooks_: para armazenar hooks personalizados.
- _src/pages_: cada característica principal ou componente de página deve ter um arquivo correspondente dentro da pasta _pages_. Os componentes de página na pasta _pages_ servem como composições de componentes reutilizáveis da pasta _components_.

## Modelos em arquivos .d.ts

Definir tipos para os principais modelos da aplicação e armazená-los em arquivos com extensão `.d.ts` na pasta _src/models_. Por exemplo, você pode definir os seguintes arquivos:

./
└── src
   └── models
       ├── order.d.ts: com o tipo da pedido, do item da pedido e o tipo do estado da pedido.
       └── product.d.ts: com o tipo do produto e o tipo do tipo de produto.

Recomenda-se o uso de `type` em vez de `interface` porque esta implementação não é POO.

Essa extensão .d.ts é recomendada para arquivos que apenas definem tipos ou interfaces, para que o processador tsc não crie os arquivos JS correspondentes.

## Organização de componentes

Definir componentes em arquivos com extensão `.tsx`. Definir um único componente por arquivo. Usar o nome do componente como nome do arquivo _.tsx_.

Criar um diretório _src/components_ para os componentes e um subdiretório lá para os arquivos _.tsx_, _.spec.tsx_ e _.module.css_ de cada componente. Por exemplo, para um componente hipotético _MovieCard_, teríamos a seguinte estrutura:

./
└── src
   └── components
       └── MovieCard
            ├── MovieCard.tsx
            ├── MovieCard.module.css
            └── MovieCard.spec.ts

## Tipagem de componentes

Tipar cada componente com `React.FC` e criar um tipo para as props de cada componente e usá-lo no tipo genérico de `React.FC`. Por exemplo, o seguinte bloco de código define um tipo _ProductListProps_ para as props do componente _ProductList_, que é tipado usando `React.FC` e especifica que as props deste componente são do tipo _ProductListProps_ com `<ProductListProps>`.

```tsx
type ProductListProps = {
  products: Product[];
  onProductClicked: (payload: OrderProduct) => void;
};

const ProductList: React.FC<ProductListProps> = ({
  products,
  onProductClicked,
}) => {

};
```

## Proteção de rotas

Proteja as rotas de sua aplicação validando que apenas usuários que fizeram login ou com a função correta possam acessá-las. Por exemplo, no Burger Queen API Client, uma garçonete não pode acessar a rota de administração de produtos.

O seguinte bloco de código define um componente _ProtectedRoute_ que você pode usar como base para proteger as rotas.

```tsx
export const ProtectedRoute = () => {
  const { token, userId } = getSession();
  const navigate = useNavigate();
  const validateUser = async () => {
    try {
      await getUser(userId);
    } catch (error) {
      navigate(PATHNAMES.LOGIN);
    }
  };

  useEffect(() => {
    validateUser();
  });

  return token ? <Outlet /> : <Navigate to="/login" replace />;
};
```

## Outras convenções e boas práticas

Neste [artigo](https://levelup.gitconnected.com/react-code-conventions-and-best-practices-433e23ed69aa), você encontrará outras convenções e boas práticas. Destacamos as seguintes:

### Use PascalCase em componentes, interfaces ou aliases de tipo.

```tsx
// Componente React
const BannersEditForm = () => {
  ...
}

// Interface TypeScript
interface TodoItem {
  id: number;
  name: string;
  value: string;
}

// Tipo alias TypeScript
type TodoList = TodoItem[];
```

### Use camelCase para tipos de dados em JavaScript/TypeScript, como variáveis, arrays, objetos, funções, etc.

```tsx
const getLastDigit = () => { ... }

const userTypes = [ ... ]
```

### Use camelCase para nomes de pastas e arquivos que não são de componente, e PascalCase para nomes de arquivos de componentes.

```tsx
src/utils/form.ts
src/hooks/useForm.ts
src/components/banners/edit/Form.tsx
```

### Agrupe o estado sempre que possível

```tsx
// ❌
const [username, setUsername] = useState('')
const [password, setPassword] = useState('')

// ✅
const [user, setUser] = useState({})
```
