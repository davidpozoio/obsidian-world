## Libraries added
- @tanstack/react-query
## TanstackQuery
### configuración
Añadir Componente para manejar estado en la App.tsx
```js
const queryClient = new QueryClient();

export function App() {
	return (
		<QueryClientProvider client={queryClient}>
			<Router />
		</QueryClientProvider>
	);
}
```
