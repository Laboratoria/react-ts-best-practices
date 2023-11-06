# Convenciones y buenas prácticas de React TypeScript

A continuación listamos algunas convenciones y buenas prácticas para tus proyectos React TypeScript:

## ViteJS para crear un proyecto React TypeScript

Usa [ViteJS](https://vitejs.dev/) para crear un proyecto React TypeScript. Encontrarás muchos blogs, cursos y videos que usan _Create React App_ pero esta herramienta ya está en desuso.

## Stack tecnológico

Específicamente para los proyectos Movie Challenge y Burger Queen API Client se recomienda el siguiente stack tecnológico, aunque este podría ser el de cualquier proyecto React TypeScript:

- ViteJs para construir el proyecto.
- Jest y React Testing Library para las pruebas de componentes.
- React Router DOM para las rutas de la SPA.
- ESLint para linting
- Prettier para el formato de archivos

## Estructura de carpetas

Específicamente para los proyectos Movie Challenge y Burger Queen API Client se recomienda la siguiente estructura de carpetas, aunque este podría ser la de cualquier proyecto React TypeScript:

- _src/components_: para albergar componentes de React.
- _src/services_: para servicios responsables de obtener datos de las APIs.
- _src/styles_: para almacenar estilos globales o utilidades de estilo.
- _src/utils_: para funciones de utilidad que no encajen en otras categorías.
- _src/assets_: para almacenar activos como imágenes, fuentes e iconos.
- _src/models_: para almacenar tipos comunes e interfaces utilizadas en toda la aplicación.
- _src/hooks_: para almacenar custom hooks.
- _src/pages_: cada característica principal o componente de página debe tener un archivo correspondiente dentro de la carpeta _pages_. Los componentes de página en la carpeta pages sirven como composiciones de componentes reutilizables de la carpeta components.

## Modelos en archivos .d.ts

Definir tipos para los principales modelos de la aplicación y almacenarlos en archivos con extensión `.d.ts` en la carpeta _src/models_. Por ejemplo puedes definir los siguientes archivos:

./
└── src
   └── models
       ├── order.d.ts: con el type de la orden, del item de la orden y el type de estado de la orden.
       └── product.d.ts: con el type del producto y el type de tipo de producto.

Se recomienda el uso de type sobre interface porque esta implementación no es OOP.

Esta extensión .d.ts es recomendada para los archivos que sólo definen types o interfaces, para que el procesador tsc no cree los archivos JS correspondientes.

## Organización de componentes

Definir componentes en archivos de extensión `.tsx`. Definir un solo componente por archivo. Usar como nombre del archivo _.tsx_ el nombre del componente

Crear un directorio _src/components_ para los componentes y allí un subdirectorio para los archivos _.tsx_, _.spec.tsx_ y _.module.css_ de cada componente. Por ejemplo, para un hipotetico componente _MovieCard_ se tendría la siguiente estructura:

./
└── src
   └── components
       └── MovieCard
            ├── MovieCard.tsx
            ├── MovieCard.module.css
            └── MovieCard.spec.ts

## Tipar componentes

Tipar cada componente con `React.FC` y crear un type para los props de cada componente y usarlo en el tipo genérico de `React.FC`. Por ejemplo, el siguiente bloque de código define un tipo _ProductListProps_ para los props del componente _ProductList_ que es tipado usando `React.FC` y se especifíca que los props de este componente son de tipo _ProductListProps_ con `<ProductListProps>`.

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

## Protección de rutas

Protege las rutas de tu aplicación validando que solo usuarios que hayan iniciado sesión o con el rol correcto puedan accederlas. Por ejemplo, en Burger Queen API Client una usuaria mesera no puede ingresar a la ruta de administración de productos.

El siguiente bloque de código define un componente _ProtectedRoute_ que puedes usar como base para proteger las rutas.

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

## Otras convenciones y buenas prácticas

En este [artículo](https://levelup.gitconnected.com/react-code-conventions-and-best-practices-433e23ed69aa) encontrarás otras convenciones y buenas prácticas, destacamos las siguientes:

### Usa PascalCase en componentes, interfaces o alias de tipo.

```tsx
// React component
const BannersEditForm = () => {
  ...
}

// Typescript interface
interface TodoItem {
  id: number;
  name: string;
  value: string;
}

// Typescript type alias
type TodoList = TodoItem[];
```

### Usa camelCase para tipos de datos en JavaScript como variables, matrices, objetos, funciones, etc.

```tsx
const getLastDigit = () => { ... }

const userTypes = [ ... ]
```

### Usa camelCase para nombres de carpetas y archivos que no son de componente, y PascalCase para nombres de archivos de componentes.

```tsx
src/utils/form.ts
src/hooks/useForm.ts
src/components/banners/edit/Form.tsx
```

### Agrupa el estado en donde sea posible

```tsx
// ❌
const [username, setUsername] = useState('')
const [password, setPassword] = useState('')

// ✅
const [user, setUser] = useState({})
```
