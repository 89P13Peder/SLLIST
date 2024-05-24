
```c++
template<typename Object>
SLList<Object>::Node::Node(const Object &d, Node *n)
        : data{d}, next{n} {}
```
Definición del constructor de Node. Este toma una referencia constante `"d"` al objeto y un puntero `"n"` al siguiente nodo.

```c++
template<typename Object>
SLList<Object>::Node::Node(Object &&d, Node *n)
        : data{std::move(d)}, next{n} {}
```
Definición del constructor de Node que toma el objeto por valor o rvalue `"d"` y el puntero `"n"`. Se hace uso de la función `move()` para mover el objeto `"d"` al miembro data y se inicializa el miembro next con el puntero `"n"`.

```c++
template<typename Object>
SLList<Object>::iterator::iterator() : current{nullptr} {}
```
Se define el constructor predeterminado del iterador inicializando el puntero `current` como nulo, lo que significa que el iterador está en un estado nulo.
```c++
template<typename Object>
Object &SLList<Object>::iterator::operator*() {
    if(current == nullptr)
        throw std::logic_error("Trying to dereference a null pointer.");
    return current->data;
}
```
Implementación de la sobrecarga del operador de indirección para permitir el acceso al dato almacenado en el nodo al que apunta el iterador.
En caso de que la dirección a la que apunte sea nula, se lanza una excepción.

```c++
template<typename Object>
typename SLList<Object>::iterator &SLList<Object>::iterator::operator++() {
    if(current)
        current = current->next;
    else
        throw std::logic_error("Trying to increment past the end.");
    return *this;
}
```
Implementación de la sobrecarga del operador de pre-incremento. En caso de que el nodo actual no sea nulo, se avanza al siguiente. De lo contrario este lanzará una excepción.
Finalmente retorna una referencia al objeto iterator actual.
```c++
template<typename Object>
typename SLList<Object>::iterator SLList<Object>::iterator::operator++(int) {
iterator old = *this;
++(*this);
return old;
}
```
En esta implementación, se guarda una copia del iterador actual en una variable llamada 'old' antes de incrementarlo. Esto permite conservar la información del nodo actual antes de avanzar al siguiente nodo. Luego, el iterador actual se incrementa al siguiente nodo utilizando un pre-incremento. Finalmente, se devuelve la información del nodo original (almacenada en 'old'), lo que representa el estado del nodo antes de la operación de incremento.
```c++
template<typename Object>
bool SLList<Object>::iterator::operator==(const iterator &rhs) const {
    return current == rhs.current;
}
```
Esta es la implementación de sobrecarga del operador de comparación permite saber si dos iteradores apuntan al mismo nodo. Principalmente para saber si dos iteradores son iguales
antes de realizar ciertas operaciones en la lista enlazada.
```c++
template<typename Object>
bool SLList<Object>::iterator::operator!=(const iterator &rhs) const {
    return !(*this == rhs);
}
```
Esta es la implementación de sobrecarga del operador de desigualdad, esta recibe por parametro una referencia a un iterador constante, y esta función no realizará cambios en este.
Se compara si ambos iteradores apuntan al mismo nodo en la lista enlazada. Luego la expresión `!` negará el resultado obtenido en la anterior comparación, en caso de que esta sea true (los iteradores apuntan al mismo nodo) se volverá false y viceversa.
Se devolverá la negación de la comparación. Si los iteradores no apuntan al mismo nodo, esta línea devolverá true, lo que indica que los iteradores son desiguales. Si apuntan al mismo nodo, devolverá false, indicando que los iteradores son iguales.
```c++
template<typename Object>
SLList<Object>::iterator::iterator(Node *p) : current{p} {}
```

Esta es la implementación del constructor de la clase `iterator`, el cual toma un puntero a un nodo como parámetro, y luego se inicializa el miembro de datos `current` de la clase `iterator` con el valor del puntero `p`.
El miembro de datos `current` representa el nodo al que apunta el iterador. Al inicializar `current` con el puntero `p`, se establece que el iterador apunta al nodo especificado por `p`.
```c++
template<typename Object>
SLList<Object>::SLList() : head(new Node()), tail(new Node()), theSize(0) {
head->next = tail;
}
```
Implementación del contructor por defecto de SLList, este inicializa los miembros de datos `head`(primer nodo de la lista), `tail`(último nodo de la lista) y `theSize`(tamaño de la lista, se inicializa en 0 ya que está vacía).
`{ head->next = tail; } ` Es la instrucción adicional que accede al miembro next de head y lo iguala a tail, para indicar que no hay elementos entre head y tail.

```c++
template<typename Object>
SLList<Object>::SLList(std::initializer_list <Object> init_list) {
    head = new Node();
    tail = new Node();
    head->next = tail;
    theSize = 0;
    for(const auto& x : init_list) {
        push_front(x);
    }
}
```
Se define el constructor que toma como parámetro una lista de inicialización para crear un objeto
SLList pasando una lista de elementos como argumento. Se crean los nodos head y tail para definir el primer y último nodo de la lista enlazada.
Se establece que el puntero nextt del nodo head apunte al siguiente nodo (tail), esto significa que entre head y tail no hay otros nodos, por lo que la lista enlazada está vacía.
Se inicializa la lista con 0 elementos.
Se itera sobre cada elemento de `initializer_list` `init_list` y se inserta cada elemento al principio de la lista con la función `push_front`
para construir la lista enlazada con los elementos proporcionados en el `std::initializer_list`.
```c++
template<typename Object>
SLList<Object>::~SLList() {
clear();
delete head;
delete tail;
}
```
Esta es la implemetación del destructor de la clase SLList que elimina todoso los nodos de la lista y reestablecer el tamaño de la lista enlazada a 0.
Esto para liberar primero la memoria de los nodos antes de eliminar los punteros de head y tail.
En `delete head;` y `delete tail;` se libera la memoria asignada para los nodos `head` y `tail`.

```c++
template<typename Object>
typename SLList<Object>::iterator SLList<Object>::begin() { return {head->next}; }

template<typename Object>
typename SLList<Object>::iterator SLList<Object>::end() { return {tail}; }

template<typename Object>
int SLList<Object>::size() const { return theSize; }

template<typename Object>
bool SLList<Object>::empty() const { return size() == 0; }

template<typename Object>
void SLList<Object>::clear() { while (!empty()) pop_front(); }
```
Estas son implementaciones de varios métodos de la clase SLList:

1. `begin()`: Devuelve un iterador que apunta al primer elemento de la lista. Esto creando un iterador que apunta al nodo siguiente del nodo `head`.
2. `end()`: Devuelve un iterador que apunta al último nodo de la lista `tail`.
3. `size()`: Devuelve el tamaño actual de la lista.
4. `empty()`: Devuelve `true` si la lista está vacía, de lo contrario devuelve `false`.
5. `clear()`: Se hace uso de la función pop_front() para eliminar cada nodo de esta conforme se itera en ella mientres esta no esté vacía.

```c++
template<typename Object>
Object &SLList<Object>::front() {
if(empty())
throw std::logic_error("List is empty.");
return *begin();
}
```
Este método de la clase SLList, devuelve una referencia al primer elemento de la lista.
Se usa la funicón empty() para comprobar que la lista enlazada está vacía, si lo está se manda la exepción
`std::logic_error("List is empty.");`. Si no está vacía se devuelve la referencia al primer elemento.
```c++
template<typename Object>
void SLList<Object>::push_front(const Object &x) { insert(begin(), x); }
```
Este método agrega un nuevo elemento al inicio de la lista enlazada.
Se usa el méttodo `begin()` para acceder al iterador de el primer elemento de la lista, este iterador se pasa como
argumento en el método `insert()`. El argumento x se pasa como una referencia constante, lo que 
significa que se inserta una copia del elemento `x` en la lista enlazada.

```c++
template<typename Object>
void SLList<Object>::push_front(Object &&x) { insert(begin(), std::move(x)); }
```
Este método agrega un nuevo elemento al inicio de la lista, esta vez utilizando la semátnica de movimiento,
En vez de pasar directamente x como en el método anterior, se usa la función `std::move(x));` 
para convertir a x en un rvalue lo que permite tomar los recursos de x y moverlos a la lista
enlazada en vez de copiarlos. Esto es útil cuando hay objetos de mucha capacidad de alacenamiento
y se requiere evitar la sobrecarga de copia.

```c++
template<typename Object>
void SLList<Object>::pop_front() {
if(empty())
throw std::logic_error("List is empty.");
erase(begin());
}
```
Este método elimina el primer elemento de la lista.
Si esta está vacía, se lanza la exepción `std::logic_error("List is empty.");`
En caso de que no lo esté, se elimina el primer elemento de la lista usando el método `begin()`
que obtiene un iterador que apunta al primer elemento de la lista y este se pasa como argumento
al método `erase()`, el cual elimina el elmento apuntado por ese iterador.

```c++
template<typename Object>
typename SLList<Object>::iterator SLList<Object>::insert(iterator itr, const Object &x) {
Node *p = itr.current;
Node *newNode = new Node{x, p->next};
p->next = newNode;
theSize++;
return iterator(newNode);
}
```
Este método inserta un nuevo elemento antes de la posición indicada por el iterador pasado como argumento.
- `Node *p = itr.current;` Puntero al nodo actual al que apunta el iterador `itr`. El miembro de datos `current`, del iterador contiene un puntero al nodo actual.
- `Node *newNode = new Node{x, p->next};` Se crea un nuevo nodo con el valor de `x`
y su puntero que apunta al nodo del siguiente elemento del actual.
- `p->next = newNode;` Se actualiza el puntero next para que apunte al nodo `newNode`, para enlazar al nuevo nodo antes del nodo actual.
- `theSize++;` Aumentamos el tamaño de la lista para indicar que se ha añadido un nuevo nodo.
Finalmente se retorna un iterador que apunta al nuevo nodo insertado.

```c++
template<typename Object>
typename SLList<Object>::iterator SLList<Object>::erase(iterator itr) {
if (itr == end())
throw std::logic_error("Cannot erase at end iterator");
Node *p = head;
while (p->next != itr.current) p = p->next;
Node *toDelete = itr.current;
p->next = itr.current->next;
delete toDelete;
theSize--;
return iterator(p->next);
}
```

Este método elimina el elemento indicado en la posición indicada por el iterador pasado como argumento.
Si el iterador apunta al final de la lista, ya no se puede seguir iterando en ella ya que este ha llegado al final de la lista enlazada, por lo tanto
si esto es verdadero se lanza la exepción `throw std::logic_error("Cannot erase at end iterator");`
- `Node *p = head;` Inicializa el puntero p al inicio de esta, para poder recorrer la lista enlazada.
- `while (p->next != itr.current) p = p->next;` Esta línea es un bucle que va avanzando entre punteros
desde el inicio hasta que el puntero del nodo actual sea igual al puntero del iterador del nodo al que queremos eliminar.
- `Node *toDelete = itr.current;` Aquí guardamos en la variable `toDelete` el  nodo a eliminar.
- `p->next = itr.current->next;` Esto actualiza el puntero, para que apunte al nodo que sigue después del que queremos eliminar.
Es como dar un "salto".
- Usando el operador `delete`, se elimina el nodo requerido.
- se disminuye una vez el tamaño de la lista ya eliminado el nodo.
- Se retorna un iterador que apunta al elemento siguiente al eliminado.

```c++
template<typename Object>
void SLList<Object>::print() {
iterator itr = begin();
while (itr != end()) {
std::cout << *itr << " ";
++itr;
}
std::cout << std::endl;
}
```
Este es el método para imprimir todos los elementos almacenados de la lista enlazada.
- `iterator itr = begin();` Inicializa un iterador al principio de la lista con el método `begin()`.
- `while (itr != end())` Este es el bucle para recorrer toda la lista mediante el uso del iterador inicializado anteriormente.
- `std::cout << *itr << " ";` Se imprime el valor del elemento al que apunta el iterador. Imprime el contenido del nodo al que apunta el iterador en otras palabras.
- `++itr;` Se aumenta el iterador para que este apunte al siguiente elemento de la lisa.

```c++
template<typename Object>
void SLList<Object>::init() {
theSize = 0;
head->next = tail;
}
```
Este método simplemente inicializa una lista vacía que conecta el `nodo` de head con el nodo de `tail`.