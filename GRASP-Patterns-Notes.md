
# Patrones GRASP en Java

## Introducción
GRASP, que significa "General Responsibility Assignment Software Patterns" (Patrones Generales de Asignación de Responsabilidades del Software), es un conjunto de patrones utilizados para asignar responsabilidades en el diseño orientado a objetos. Fueron introducidos por Craig Larman en su libro "Applying UML and Patterns". Los patrones GRASP son ocho, y cada uno de ellos aborda un aspecto específico de la asignación de responsabilidades. A continuación se describen brevemente, pero nos centraremos en los tres primeros.

## Tabla de Contenidos
1. [Información Experta (Expert)](#1-informacion-experta-expert)
2. [Creador (Creator)](#2-creador-creator) 
3. [Controlador (Controller)](#3-controlador-controller) 
4. [Otros Patrones](#otros-patrones)

## 1. Información Experta (Expert)
### Descripción
Este patrón se centra en asignar responsabilidades de comportamiento a la clase que tiene la información necesaria para llevar a cabo esa responsabilidad. Aquí están algunas propiedades clave del patrón Experto en Información:
-   **Localización de la Información**: La responsabilidad debe asignarse a la clase que tiene la información necesaria para cumplir con esa responsabilidad.
    
-   **Alta Cohesión**: Fomenta una alta cohesión en las clases, ya que las responsabilidades están relacionadas con la información que la clase ya posee.
    
-   **Bajo Acoplamiento**: Ayuda a mantener un bajo acoplamiento entre las clases, ya que evita que una clase dependa de la información de otra clase para cumplir con sus responsabilidades.
    
-   **Responsabilidad Encapsulada**: La información y el comportamiento están encapsulados dentro de la misma clase, lo que facilita la comprensión y el mantenimiento del código.
    
-   **Facilidad de Mantenimiento**: Como la información y el comportamiento están agrupados, los cambios en la información generalmente afectan solo a la clase que la posee, lo que facilita el mantenimiento.
    
-   **Claridad en la Asignación de Responsabilidades**: Hace que sea claro y lógico cuál clase es responsable de qué comportamiento, basado en la información que posee.

### Ejemplo de Código
Supongamos que estamos desarrollando un sistema de gestión de inventarios y tenemos una clase `Producto` que contiene información sobre el precio y la cantidad de los productos en stock. Según el patrón Experto en Información, la responsabilidad de calcular el valor total del inventario de un producto debería asignarse a la clase `Producto`, ya que es la que tiene la información necesaria (precio y cantidad).

```java
public class Producto { 
	private double precio; 
	private int cantidad; 

	public Producto(double precio, int cantidad) {
		this.precio = precio; 
		this.cantidad = cantidad; 
	} 
	public double calcularValorInventario() { 
		return precio * cantidad; 
	} 
	// Otros métodos y atributos 
}
```
En este ejemplo, la clase `Producto` es el "Experto en Información" porque tiene toda la información necesaria para calcular el valor total del inventario.

## 2. Creador (Creator)
### Descripción
Asigna la responsabilidad de crear una instancia de una clase a la clase que tiene la información necesaria para crearla o a una clase que utiliza intensamente la instancia creada.

### Ejemplo de Código
Supongamos que estamos diseñando un sistema de gestión de pedidos para una tienda en línea.  En este sistema, tenemos las siguientes clases principales: Clientes, Pedidos y Productos.
Queremos asignar la responsabilidad de crear instancias de la clase 'Pedido'. Siguiendo el patrón creador, debemos identificar qué clase tiene la información necesaria para crear un 'Pedido' o cuál utiliza intensamente la instancia creada. En este caso, la clase 'Cliente' es una buena candidata, ya que es el cliente quien realiza el pedido.
```java
// Clase Pedido
public class Pedido {
    private List<Producto> productos;
    private Cliente cliente;

    // Constructor
    public Pedido(Cliente cliente) {
        this.cliente = cliente;
        this.productos = new ArrayList<>();
    }

    // Métodos para agregar productos al pedido
    public void agregarProducto(Producto producto) {
        productos.add(producto);
    }

    // Otros métodos del Pedido
}

// Clase Cliente
public class Cliente {
    private String nombre;
    private List<Pedido> pedidos;

    // Constructor
    public Cliente(String nombre) {
        this.nombre = nombre;
        this.pedidos = new ArrayList<>();
    }

    // Método para crear un nuevo pedido
    public Pedido crearPedido() {
        Pedido pedido = new Pedido(this);
        pedidos.add(pedido);
        return pedido;
    }

    // Otros métodos del Cliente
}

// Clase Producto
public class Producto {
    private String nombre;
    private double precio;

    // Constructor
    public Producto(String nombre, double precio) {
        this.nombre = nombre;
        this.precio = precio;
    }

    // Otros métodos del Producto
}

   ```


En el ejemplo, la clase `Pedido` tiene un constructor que requiere una instancia de `Cliente`.
La Clase `Cliente` tiene un método `crearPedido()` que crea y devuelve una nueva instancia de `Pedido`, añadiendo el pedido a la lista de pedidos del cliente.

Mediante el patrón Creador, asignamos la responsabilidad de crear un `Pedido` a la clase `Cliente`  quien tiene la información necesaria (su propia Referencia) y porque es el `Cliente` quien utiliza intensamente la instancia de `Pedido`.

## 3. Controlador (Controller)
### Descripción
Define qué clase debe manejar una acción específica del sistema. Suele ser una clase que representa un caso de uso o un controlador de sesión.
 -   **Intermediario entre la UI y la lógica de negocio:** El controlador recibe solicitudes de la interfaz de usuario (UI), las procesa y coordina con la lógica de negocio. Esto ayuda a mantener la UI y la lógica de negocio desacopladas.
   -   **Agrupación de operaciones relacionadas:** Un controlador agrupa operaciones que están relacionadas con un caso de uso específico, facilitando su manejo y mantenimiento.
    -   **Manejo de sesiones y casos de uso:** Puede manejar estados de sesión del usuario y coordinar casos de uso complejos que involucran múltiples pasos.
    -   **Reducción de dependencias:** Al actuar como intermediario, reduce las dependencias directas entre componentes del sistema, promoviendo un diseño más modular.
    -   **Facilita las pruebas unitarias:** Las operaciones agrupadas en el controlador pueden probarse de manera aislada, lo que facilita la escritura de pruebas unitarias.

### Ejemplo de Código
  Supongamos que estamos diseñando un sistema de reservas de vuelos. En este sistema, los usuarios pueden buscar vuelos, reservar un vuelo, y cancelar un vuelo. Tenemos las siguientes clases principales:
   Usuario, Vuelo, Reserva y SistemaReservas.
   Queremos asignar la responsabilidad de manejar las solicitudes del usuario, como buscar vuelos y reservar un vuelo. Según el patrón Controlador, estas responsabilidades deberían asignarse a una clase que represente un caso de uso o un controlador de sesión.
   
   Podemos crear una clase `ControladorReservas` que actúe como intermediario entre el usuario y el sistema de reservas. Esta clase manejará las solicitudes del usuario y coordinará las operaciones necesarias.

 ```java
   // Clase Usuario
public class Usuario {
    private String nombre;
    private String email;

    // Constructor
    public Usuario(String nombre, String email) {
        this.nombre = nombre;
        this.email = email;
    }

    // Otros métodos del Usuario
}

// Clase Vuelo
public class Vuelo {
    private String numeroVuelo;
    private String origen;
    private String destino;

    // Constructor
    public Vuelo(String numeroVuelo, String origen, String destino) {
        this.numeroVuelo = numeroVuelo;
        this.origen = origen;
        this.destino = destino;
    }

    // Getters
    public String getOrigen() {
        return origen;
    }

    public String getDestino() {
        return destino;
    }

    // Otros métodos del Vuelo
}

// Clase Reserva
public class Reserva {
    private Usuario usuario;
    private Vuelo vuelo;

    // Constructor
    public Reserva(Usuario usuario, Vuelo vuelo) {
        this.usuario = usuario;
        this.vuelo = vuelo;
    }

    // Otros métodos de la Reserva
}

// Clase SistemaReservas
public class SistemaReservas {
    private List<Vuelo> vuelosDisponibles;
    private List<Reserva> reservas;

    // Constructor
    public SistemaReservas() {
        this.vuelosDisponibles = new ArrayList<>();
        this.reservas = new ArrayList<>();
    }

    // Método para agregar vuelos disponibles
    public void agregarVuelo(Vuelo vuelo) {
        vuelosDisponibles.add(vuelo);
    }

    // Método para buscar vuelos
    public List<Vuelo> buscarVuelos(String origen, String destino) {
        return vuelosDisponibles.stream()
                .filter(v -> v.getOrigen().equals(origen) && v.getDestino().equals(destino))
                .collect(Collectors.toList());
    }

    // Método para hacer una reserva
    public Reserva hacerReserva(Usuario usuario, Vuelo vuelo) {
        Reserva reserva = new Reserva(usuario, vuelo);
        reservas.add(reserva);
        return reserva;
    }

    // Otros métodos del SistemaReservas
}

// Clase ControladorReservas (Controller)
public class ControladorReservas {
    private SistemaReservas sistemaReservas;
    private Map<String, Object> sessionAttributes;

    // Constructor
    public ControladorReservas(SistemaReservas sistemaReservas) {
        this.sistemaReservas = sistemaReservas;
        this.sessionAttributes = new HashMap<>();
    }

    // Método para manejar la búsqueda de vuelos
    public List<Vuelo> manejarBuscarVuelos(String origen, String destino) {
        return sistemaReservas.buscarVuelos(origen, destino);
    }

    // Método para manejar la reserva de un vuelo
    public Reserva manejarHacerReserva(Usuario usuario, Vuelo vuelo) {
        return sistemaReservas.hacerReserva(usuario, vuelo);
    }

    // Método para agregar atributos de sesión
    public void agregarAtributoSesion(String key, Object value) {
        sessionAttributes.put(key, value);
    }

    // Método para obtener atributos de sesión
    public Object obtenerAtributoSesion(String key) {
        return sessionAttributes.get(key);
    }

    // Otros métodos del ControladorReservas
}
   ```
   En el ejemplo, `SistemasReservas`contiene la lógica principal del sistema, como la gestión de vuelos disponibles y reservas.
   La clase `ControladorReservas` actúa como intermediario y menaja las solicitudes del usuario, como buscar vuelos (`manejarBuscarVuelos`) y hacer una reserva (`manejarHacerReserva`).
   Los usuarios interactúan con `ControladorReservas` en lugar de interactuar directamente con `SistemaReservas`, lo que permite una separación clara de responsabilidady facilita el mantenimiento del sistema.
   Se ha añadido un mapa (`sessionAttributes`) para manejar atributos de sesión, lo cual es útil para mantener el estado entre socitudes del usuario.
   Los métodos como àgregarAtributoSesion` y `obtenerAtributoSesion` permiten almacenar y recuperar información de sesión.
# Atributos de Sesion
   Hace un rato mencionamos que un controlador es útil para manejar atributos de sesion. Los atributos de sesión son datos que se almacenan durante una sesión del usuario y que se puede utilizar en varias solicitudes durante esa sesión. Esto es útil para mantener información persistente sobre el usuario mientras interactúa con el sistema, como el carrito de compras, preferencias del usuario, o información de autenticación.
   Imaginemos que estamos implementando una funcionalidad de reserva de vuelos donde un usuario puede buscar vuelos y agregar uno o más vuelos a un carrito de reservas antes de confirmar la reserva. 
   Vamos a usar los atributos de sesión para almacenar el carrito de reservas del usuario durante la sesión.
   
   
   
   ```java
   // Clase Usuario
public class Usuario {
    private String nombre;
    private String email;

    // Constructor
    public Usuario(String nombre, String email) {
        this.nombre = nombre;
        this.email = email;
    }

    // Otros métodos del Usuario
}

// Clase Vuelo
public class Vuelo {
    private String numeroVuelo;
    private String origen;
    private String destino;

    // Constructor
    public Vuelo(String numeroVuelo, String origen, String destino) {
        this.numeroVuelo = numeroVuelo;
        this.origen = origen;
        this.destino = destino;
    }

    // Getters
    public String getNumeroVuelo() {
        return numeroVuelo;
    }

    public String getOrigen() {
        return origen;
    }

    public String getDestino() {
        return destino;
    }

    // Otros métodos del Vuelo
}

// Clase Reserva
public class Reserva {
    private Usuario usuario;
    private Vuelo vuelo;

    // Constructor
    public Reserva(Usuario usuario, Vuelo vuelo) {
        this.usuario = usuario;
        this.vuelo = vuelo;
    }

    // Otros métodos de la Reserva
}

// Clase SistemaReservas
public class SistemaReservas {
    private List<Vuelo> vuelosDisponibles;
    private List<Reserva> reservas;

    // Constructor
    public SistemaReservas() {
        this.vuelosDisponibles = new ArrayList<>();
        this.reservas = new ArrayList<>();
    }

    // Método para agregar vuelos disponibles
    public void agregarVuelo(Vuelo vuelo) {
        vuelosDisponibles.add(vuelo);
    }

    // Método para buscar vuelos
    public List<Vuelo> buscarVuelos(String origen, String destino) {
        return vuelosDisponibles.stream()
                .filter(v -> v.getOrigen().equals(origen) && v.getDestino().equals(destino))
                .collect(Collectors.toList());
    }

    // Método para hacer una reserva
    public Reserva hacerReserva(Usuario usuario, Vuelo vuelo) {
        Reserva reserva = new Reserva(usuario, vuelo);
        reservas.add(reserva);
        return reserva;
    }

    // Otros métodos del SistemaReservas
}

// Clase ControladorReservas (Controller)
public class ControladorReservas {
    private SistemaReservas sistemaReservas;
    private Map<String, Object> sessionAttributes;

    // Constructor
    public ControladorReservas(SistemaReservas sistemaReservas) {
        this.sistemaReservas = sistemaReservas;
        this.sessionAttributes = new HashMap<>();
    }

    // Método para manejar la búsqueda de vuelos
    public List<Vuelo> manejarBuscarVuelos(String origen, String destino) {
        return sistemaReservas.buscarVuelos(origen, destino);
    }

    // Método para manejar la reserva de un vuelo
    public Reserva manejarHacerReserva(Usuario usuario, Vuelo vuelo) {
        return sistemaReservas.hacerReserva(usuario, vuelo);
    }

    // Método para agregar vuelos al carrito de reservas en la sesión
    public void agregarVueloAlCarrito(Vuelo vuelo) {
        List<Vuelo> carrito = (List<Vuelo>) sessionAttributes.getOrDefault("carritoReservas", new ArrayList<>());
        carrito.add(vuelo);
        sessionAttributes.put("carritoReservas", carrito);
    }

    // Método para obtener los vuelos en el carrito de reservas
    public List<Vuelo> obtenerCarritoReservas() {
        return (List<Vuelo>) sessionAttributes.getOrDefault("carritoReservas", new ArrayList<>());
    }

    // Método para confirmar las reservas en el carrito
    public List<Reserva> confirmarReservas(Usuario usuario) {
        List<Vuelo> carrito = obtenerCarritoReservas();
        List<Reserva> reservasConfirmadas = new ArrayList<>();
        for (Vuelo vuelo : carrito) {
            Reserva reserva = sistemaReservas.hacerReserva(usuario, vuelo);
            reservasConfirmadas.add(reserva);
        }
        // Limpiar el carrito después de confirmar las reservas
        sessionAttributes.put("carritoReservas", new ArrayList<>());
        return reservasConfirmadas;
    }

    // Otros métodos del ControladorReservas
}

   ```

   Agregar Vuelo al Carrito
   El método `agregarVueloAlCarrito(Vuelo vuelo)` agrega un vuelo al carrito de reservas del usuario. Si no hay un carrito existente en la sesión, se crea uno nuevo.
   Obtener Carrito de Reserva
   EL método `obtenerCarritoReservas()` devuelve la lista de vuelos actualmente en el carrito de reservas. Si no hay vuelos en el carrito, devuelve una lista vacía.
   Confirmar Reservas
   El método `confirmarReservas(Usuario usuario)` toma los vuelos del carrito de reservas, crea una reserva para cada vuelo, y luego limpia el carrito. Devuelve la lista de reservas confirmadas.
   # Ejemplo de Uso

```java

    public static void main(String[] args) {
        SistemaReservas sistemaReservas = new SistemaReservas();
        ControladorReservas controlador = new ControladorReservas(sistemaReservas);

        // Agregar vuelos disponibles
        Vuelo vuelo1 = new Vuelo("AA123", "NYC", "LAX");
        Vuelo vuelo2 = new Vuelo("BB456", "NYC", "SFO");
        sistemaReservas.agregarVuelo(vuelo1);
        sistemaReservas.agregarVuelo(vuelo2);

        // Buscar vuelos
        List<Vuelo> vuelos = controlador.manejarBuscarVuelos("NYC", "LAX");
        System.out.println("Vuelos encontrados: " + vuelos.size());  // Debe ser 1

        // Usuario crea carrito y agrega vuelos
        Usuario usuario = new Usuario("John Doe", "john@example.com");
        controlador.agregarVueloAlCarrito(vuelo1);

        // Obtener vuelos en el carrito
        List<Vuelo> carrito = controlador.obtenerCarritoReservas();
        System.out.println("Vuelos en el carrito: " + carrito.size());  // Debe ser 1

        // Confirmar reservas
        List<Reserva> reservas = controlador.confirmarReservas(usuario);
        System.out.println("Reservas confirmadas: " + reservas.size());  // Debe ser 1
    }
```



   Finalmente podemos decir que el patrón Controlador proporciona una estructura clara para manejar las interacciones del usuario y coordinar la lógica de negocio, manteniendo un diseño modular y fácil de mantener. Aunque el controlador puede parecer simple inicialmente, su importancia radica en su papel como intermediario y coordinador dentro del sistema.


## Otros Patrones
  4. Bajo Acoplamiento (Low Coupling): Minimiza las dependencias entre las clases para que los cambios en una clse tengan el menor impacto posible en otras clases.
   5. Alta Cohesión (High Cohesion): Mantiene las responsabilidades de una clase centradas y bien definidas, lo que facilita su mantenimiento y reutilización.
   6. Polimorfismo(Polymorphism): Asigna la responsabilidad de los comportamientos que varían según el tipo a las clases específicas que implementan esos comportamientos, utilizando herencia e interfaces.
   7. Indirección (Indirection): Introduce una clase intermedia para mediar entre dos clases o componentes, reduciendo la dependencia directa entre ellos.
   8. Protección de Variaciones (Protected Variations): Protege elementos del sistema de los cambios en otros elementos utilizando abstracciones y encapsulamiento.

## Referencias
- [Applying UML and Patterns - Craig Larman](https://www.craiglarman.com/wiki/index.php?title=Applying_UML_and_Patterns)
