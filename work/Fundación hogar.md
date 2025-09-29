## Frontend architectures
### Component-driven architecture
Esta arquitectura organiza los proyectos por átomos -> moléculas -> organismos -> templates

#### Reglas para crear un componente
- Reusable
	Los componentes deben se reutilizables para permiter la propagación de cambios.
- Especialized
	Los componentes deben enfocarse en una función
- Context-agnostic
	Los componentes deben funcionar bien en si importar en qué lugar o área sean utilizados.
- Isolate
	Los componentes deben funcionar de forma aislada, y para modificar su comportamiento deben hacer uso de una api, es decir, su comportamiento debe seguir un flujo controlado.
- Replaceable
	Los componentes deben estar desacoplados y deben ser sencillos de remplazar.
# Nuestra arquitectura
Muestra una relacion entre los principales módulos de la aplicación
![[Excalidraw/Drawing 2025-09-19 15.48.39.excalidraw|700]]
## Core
El estado global de la aplicación (autenticación, protección de rutas, etc.).
Este parte se va a manejar con zustand
## Components
Aquí están los componentes reutilizables que forman nuestra aplicación.
Serían nuestros custom components
## Pages
Similar a los componentes, pero describen una relación entre un componente y una ruta.
```
page
|---Page.tsx
|---PageRouter.tsx
```
## Services
Aquí encontramos la parte de la aplicación que se encarga de comunicarse con el backend
Los servicios deben ser una interfaz.
```js
interface Service{
	findAll();
	update();
}

class ApiService implements Service{
// api implementation
}
class MockService implements Service{
// mock implementation
}

```
## Service hooks
Es una capa intermedia entre los componentes y los servicios. Los servicios no pueden comunicarse directamente con otras partes de la aplicación (estado global, hooks, etc.), así que agregamos una capa intermedia que permita "convertir" un servicio a un "objeto" que podamos usar en nuestros componentes.
```js
const useService = () => {
	// api service logic
	return { getAll, update, delete};
}
```
## State access
Es un capa intermediaria entre nuestra aplicación y el estado global, útil para encapsular lógica que se va a reutilizar en todo el código (useAuth, useRoute, etc.).
```js
const useAuth = () => {
	// auth logic
	return { user };
}
```
## Patrones de diseño
### Adapter
Se trata de un objeto **especial** que transforma la interfaz del objeto, de forma que otro objeto pueda entenderla.
Los adaptadores no solo convierten datos a varios formatos, sino que también ayudan a objetos con distintas interfaces a colaborar
Un adapter es un wrapper que permite a una interfaz ser compatible con otra
```ts
interface SMTPService {
	email(server, connections, credentials, to): void;
}
interface EmailService {
	send(to, email): void;
}
/*
Como puedes ver tenemos dos interfaces que describen un smtp service y un email service, el problema radica en que si queremos usar el smtp service, este servicio no ofrece un send method, además de que requiere de otros campos.
Para resolver este problema podemos crear un adapter para la clase smtp
*/
class SMTPEmailAdapter implements EmailAdapter{
	constructor(private smtpService: SMTPService){}
	send(to, email): void{
		// handle smtp implementation
		smtpService.email(server, connections, credentials, to);
	}
}
/*
Ahora tenemos un clase que es completamente compatible con nuestro EmailService.
*/

```
También podemos usar este patrón para la implementación de interfaces.
```ts
// our domain user
interface User{
	id: number;
	age: number;
	name: string
}

class ApiUserAdapter{
	static toDomain(raw: any): User{
		return {
			id: raw.id ?? raw.uuid?? unknown;
			// user implementation
		}
	}
}
```