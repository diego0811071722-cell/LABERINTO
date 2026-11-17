# LABERINTO
LLEGA DEL PUNTO A AL PUNTO B

def recorrer_laberinto(lab, A, B, portales):

    N = len(lab)
    M = len(lab[0])

    pila = [A]
    visitados = {A}

    while pila:

        actual = pila[-1]

        # Comprobar si llegó al destino
        if actual == B:
            return pila

        x, y = actual

        movimientos = [
            (x-1, y),
            (x+1, y),
            (x, y-1),
            (x, y+1)
        ]

        random.shuffle(movimientos)

        siguiente = None

        # Buscar una casilla disponible
        for nx, ny in movimientos:

            if not (0 <= nx < N and 0 <= ny < M):
                continue

            if lab[nx][ny] == 1:
                continue

            posicion = (nx, ny)

            # Comprobar portal
            destino = portales.get(posicion, posicion)

            if destino in visitados:
                continue

            if lab[destino[0]][destino[1]] == 0:

                siguiente = destino
                break

        # Avanzar o retroceder
        if siguiente is not None:

            visitados.add(siguiente)
            pila.append(siguiente)

        else:

            pila.pop()

    return None