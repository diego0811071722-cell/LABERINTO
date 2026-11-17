# LABERINTO

**Llega del punto A al punto B.**

Ejemplo en Python que encuentra y muestra una ruta por el laberinto. `0` representa un camino libre y `1` una pared.

```python
import random

lab = [
    [1, 1, 1, 1, 1, 1, 1, 1, 1],
    [1, 0, 0, 0, 0, 0, 0, 0, 1],
    [1, 0, 1, 1, 1, 0, 1, 0, 1],
    [1, 0, 0, 0, 1, 0, 0, 0, 1],
    [1, 0, 1, 0, 1, 1, 1, 0, 1],
    [1, 0, 0, 0, 0, 0, 0, 0, 1],
    [1, 1, 1, 1, 1, 1, 1, 1, 1],
]

A = (1, 1)
B = (5, 7)
portales = {}  # Ejemplo: {(3, 3): (5, 6)}


def recorrer_laberinto(lab, A, B, portales):
    """Devuelve una ruta desde A hasta B, o None si no hay salida."""
    filas = len(lab)
    columnas = len(lab[0])

    def transitable(posicion):
        x, y = posicion
        return 0 <= x < filas and 0 <= y < columnas and lab[x][y] == 0

    if not transitable(A) or not transitable(B):
        return None

    pila = [A]
    visitados = {A}

    while pila:
        actual = pila[-1]
        if actual == B:
            return pila.copy()

        x, y = actual
        movimientos = [(x - 1, y), (x + 1, y), (x, y - 1), (x, y + 1)]
        random.shuffle(movimientos)
        siguiente = None

        for posicion in movimientos:
            if not transitable(posicion):
                continue
            destino = portales.get(posicion, posicion)
            if transitable(destino) and destino not in visitados:
                siguiente = destino
                break

        if siguiente is None:
            pila.pop()
        else:
            visitados.add(siguiente)
            pila.append(siguiente)

    return None


def mostrar_laberinto(lab, ruta, A, B):
    ruta = set(ruta or [])
    for x, fila in enumerate(lab):
        linea = []
        for y, casilla in enumerate(fila):
            posicion = (x, y)
            if posicion == A:
                linea.append("A")
            elif posicion == B:
                linea.append("B")
            elif casilla == 1:
                linea.append("#")
            elif posicion in ruta:
                linea.append(".")
            else:
                linea.append(" ")
        print("".join(linea))


ruta = recorrer_laberinto(lab, A, B, portales)
if ruta:
    print("¡Encontré la salida!")
    mostrar_laberinto(lab, ruta, A, B)
else:
    print("No hay una ruta hasta la salida.")
```
