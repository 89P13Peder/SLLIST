Verificar si SLLLIST_H no está definido, en caso de que no lo esté
se define en la línea posterior.
```c++
#ifndef SLLLIST_H
#define SLLLIST_H
```
Se incluyen las librerías iostream y utility.
Iostream para el uso de la salida y entrada estándar.
Utility para hacer uso de diferentes plantillas.
```c++
#include <iostream>
#include <utility>
```
Se define SLList, que es una plantilla de clase con el parámetro de tipo Object.
También se define la clase SLList.
```c++
template<typename Object>
class SLList {
```
Como atributo público se declara una estructura interna Node, la cual contiene un Object,
y su nodo que almacenará el puntero del siguiente objeto en la lista.
Dos constructores que reciben un objeto y su puntero, uno para copiar el valor y otro para moverlo.

```c++
public:
    struct Node {
        Object data;
        Node *next;

        Node(const Object &d = Object{}, Node *n = nullptr);

        Node(Object &&d, Node *n = nullptr);
    };


```
Se define una clase interna pública "iterator", esta se encargará de darnos los iteradores
que recorrerán la lista.       
- ```iterator();``` es el constructor por defecto.
- ```Object &operator*();``` Es la sobrecarda del operador de indirección(apuntador) que accederá al objeto al que apunte el iterador.
- ```iterator &operator++();``` Sobrecarga pre-incremento, para avanzar al siguiente nodo. Se avanza al siguiente nodo y devuelve una referencia al objeto iterador modificado.
- ```const iterator operator++(int);``` Sobrecarga post-incremento para avanzar al siguiente nodo. Se devuelve el valor actual y se avanza al siguiente nodo.
- ```bool operator==(const iterator &rhs) const;``` y ```bool operator!=(const iterator &rhs) const;``` Sobrecargan los operadores de igualdad y desigualdad para comparar los iteradores.
- `Node *current;`  Apuntador al nodo actual.
- ` iterator(Node *p);` Constructor del iterador que toma un puntero al nodo inicial.
- `friend class SLList<Object>;` declara a SLList<Object> como una clase amiga para permitirle acceder a los miembros privados del iterador.

```c++
public:
    class iterator {
    public:
        iterator();

        Object &operator*();

        iterator &operator++();

        const iterator operator++(int);

        bool operator==(const iterator &rhs) const;

        bool operator!=(const iterator &rhs) const;

    private:
        Node *current;

        iterator(Node *p);

        friend class SLList<Object>;
    };


```
Se continúa delcarando los miembros públicos de la clase SLList:
- `SLList();` Declaración del constructor por defecto sin prámetros. Crea una lista enlazada vacía.
-  `SLList(std::initializer_list <Object> init_list);` Se inicializa una lista de objetos con los elementos que se indiquen y se crea la lista enlazada sencilla. Los elementos se añaden a la lista en el orden en que aparecen en la lista de inicialización.
- `~SLList();` Destructor por defecto.  libera la memoria asignada a la lista enlazada cuando el objeto SLList se destruye.
-  `iterator begin();` Método que devuelve el iterador del primer elemento de la lista.
- `iterator end();` Método que devuelve el iterador del último elemento de la lista.
- `int size() const;` Método que devuelve el tamaño de la lista sin modificar nada en ella (const).
- `bool empty() const;` Método que comprueba si la lista está vacía.
- `void clear();` Método que limpia o elimina toda la lista.
- `Object &front();` Referencia al primer elemento de la lista.
- `void push_front(const Object &x);` Método que agrega un elemento cosntante x al principio de la lista, haciendo una copia de este.
- `void push_front(Object &&x);` Método que agrega un objeto al principio de la lista mediante movimiento de x.
- `void pop_front();` Método que se encargará de eliminar el primer elemento de la lista.
- `iterator insert(iterator itr, const Object &x);`Método que inserta un nuevo elemento antes de la posición indicada por el iterador itr en la lista enlazada. Se hace una copia del objeto x y esta copia se inserta en la lista.
- `iterator insert(iterator itr, Object &&x);`Método que inserta un nuevo elemento antes de la posición indicada por el iterador itr, pero en este caso se realiza un movimiento del objeto x en lugar de una copia. Después de la inserción, el objeto x se considera "vacio" o "inválido" y no se debe utilizar más.
- `iterator erase(iterator itr);` Método para eliminar el elemento en la posición indicada por el iterador.
- `void print();` Método para imprimir todos los elementos de la lista.
```c++

public:
    SLList();

    SLList(std::initializer_list <Object> init_list);

    ~SLList();

    iterator begin();

    iterator end();

    int size() const;

    bool empty() const;

    void clear();

    Object &front();

    void push_front(const Object &x);

    void push_front(Object &&x);

    void pop_front();

    iterator insert(iterator itr, const Object &x);

    iterator insert(iterator itr, Object &&x);

    iterator erase(iterator itr);

    void print();


```
- `Node *head;`Puntero al primer nodo de la lista.
- `Node *tail;` Puntero al último nodo de la lista.
- `int theSize;` Variable entero que indica el tamaño de la lista, o número de elementos en la lista.
- `void init();` Función privada que inicializa la lista. 
```c++
private:
    Node *head;
    Node *tail;
    int theSize;

    void init();
```
- `#include "list.cpp"`: Se incluye la implementación de la clase SLList
- `#endif` : Finaliza el bloque `#ifndef` que se inicializó antes para evitar la inclusión múltiple del archivo de encabezado.
```c++
#include "list.cpp"

#endif
```