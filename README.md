# Juego de la Víbora (Snake) - Versión Modificada

Este proyecto consiste en la modificación y documentación del clásico juego Snake utilizando la biblioteca `freegames` y `turtle` de Python.

---

## 👥 Integrantes y Módulos Desarrollados

* **Desarrollador 1:** Implementación de generación de colores aleatorios para la víbora y la comida (excluyendo el rojo).
* **Desarrollador 2:** Implementación del movimiento aleatorio de la comida dentro del área de juego.

---

## 🛠️ Modificaciones Realizadas

### 1. Colores Aleatorios (Desarrollador 1)
* Se creó una lista con 5 colores distintos (`blue`, `green`, `purple`, `orange`, `black`), excluyendo explícitamente el color `red` (reservado para la colisión).
* Se seleccionan colores aleatorios en cada ejecución para la víbora y la comida mediante el módulo `random`, garantizando que `snake_color` y `food_color` sean siempre diferentes entre sí.

### 2. Movimiento Aleatorio de la Comida (Desarrollador 2)
* En cada ciclo del juego (`move()`), la comida tiene la posibilidad de desplazarse aleatoriamente un paso a la vez (10 unidades en `$x$` o `$y$`).
* Se incorporaron validaciones de límites para garantizar que el movimiento de la comida permanezca dentro de la ventana de juego (`-190 < x < 180` y `-190 < y < 180`).

---

## 📂 Archivo del Proyecto (`snake.py`)

```python
"""
Juego de la Vibora (Snake)
Modificaciones:
- Movimiento aleatorio de la comida.
- Colores aleatorios excluyendo el rojo para comida y serpiente.
"""

from random import randrange, choice
from turtle import *
from freegames import square, vector

# Estado inicial del juego
food = vector(0, 0)
snake = [vector(10, 0)]
aim = vector(0, -10)

# Lista de 5 colores disponibles (excluyendo rojo)
COLORS = ['blue', 'green', 'purple', 'orange', 'black']

# Selección de colores aleatorios y distintos entre sí
snake_color = choice(COLORS)
food_color = choice([c for c in COLORS if c != snake_color])

def change(x, y):
    """Cambia la dirección de la serpiente."""
    aim.x = x
    aim.y = y

def inside(head):
    """Verifica si la posición indicada se encuentra dentro de los límites del tablero."""
    return -200 < head.x < 190 and -200 < head.y < 190

def move_food():
    """Mueve la comida un paso aleatorio sin salir del área de juego."""
    options = [vector(10, 0), vector(-10, 0), vector(0, 10), vector(0, -10)]
    step = choice(options)
    next_position = food + step

    if inside(next_position):
        food.move(step)

def move():
    """Avanza la serpiente un segmento y actualiza la lógica del juego."""
    head = snake[-1].copy()
    head.move(aim)

    # Detección de colisiones (paredes o cuerpo)
    if not inside(head) or head in snake:
        square(head.x, head.y, 9, 'red')
        update()
        return

    snake.append(head)

    # Lógica al comer la fruta
    if head == food:
        print('Snake:', len(snake))
        food.x = randrange(-15, 15) * 10
        food.y = randrange(-15, 15) * 10
    else:
        snake.pop(0)
        # La comida intenta moverse en cada frame si no fue comida
        move_food()

    clear()

    # Dibujar cuerpo de la serpiente
    for body in snake:
        square(body.x, body.y, 9, snake_color)

    # Dibujar comida
    square(food.x, food.y, 9, food_color)
    update()
    ontimer(move, 100)

# Configuración de ventana y eventos de entrada
setup(420, 420, 370, 0)
hideturtle()
tracer(False)
listen()
onkey(lambda: change(10, 0), 'Right')
onkey(lambda: change(-10, 0), 'Left')
onkey(lambda: change(0, 10), 'Up')
onkey(lambda: change(0, -10), 'Down')

move()
done()
