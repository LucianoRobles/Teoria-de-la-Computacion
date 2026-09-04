# Máquinas de Turing — Lenguaje Regular y Lenguaje Independiente del Contexto

## Índice

1. [Máquina de Turing para un Lenguaje Regular](#1-máquina-de-turing-para-un-lenguaje-regular)
2. [Máquina de Turing para un Lenguaje Independiente del Contexto](#2-máquina-de-turing-para-un-lenguaje-independiente-del-contexto)
3. [Comparativo entre MTaccept y MTcalc](#3-comparativo-entre-mtaccept-y-mtcalc)

---

## 1. Máquina de Turing para un Lenguaje Regular

Archivo: [`MT_LR.jff`](MT_LR.jff)

### Lenguaje

Sea:

$$
L_1=\{w\in\{0,1\}^* \mid w \text{ termina en }01\}
$$

El lenguaje contiene todas las cadenas formadas por `0` y `1` cuyos dos últimos símbolos son `01`.

### Ejemplos

Pertenecen al lenguaje:

```text
01
101
001
11001
0101
```

No pertenecen al lenguaje:

```text
0
1
10
011
010
```

---

### Expresión Regular

$$
(0|1)^*01
$$

La expresión indica que puede aparecer cualquier secuencia de `0` y `1`, pero la palabra debe terminar en `01`.

---

### Gramática Regular

Definimos:

$$
G=(V_N,V_T,S,P)
$$

con:

$$
V_N=\{S,A\}
$$

$$
V_T=\{0,1\}
$$

Axioma:

$$
S
$$

Producciones:

$$
S\rightarrow 0S \mid 1S \mid 0A
$$

$$
A\rightarrow 1
$$

No se utiliza $\lambda$, ya que:

$$
\lambda\notin L_1
$$

Además, para este tipo de gramática regular se respeta la condición de que $\lambda$, si fuera necesaria, solo se utilice desde el axioma.

### Ejemplo de derivación

Para generar `101`:

$$
S\Rightarrow 1S\Rightarrow 10A\Rightarrow 101
$$

---

### Máquina de Turing

#### Alfabeto de entrada

$$
\Sigma=\{0,1\}
$$

#### Alfabeto de cinta

$$
\Gamma=\{0,1,\square\}
$$

#### Estados

- `q0`: estado inicial.
- `q1`: el último símbolo leído fue `0`.
- `q2`: los últimos dos símbolos leídos fueron `01`.
- `accept`: estado de aceptación.
- `reject`: estado de rechazo.

Estado inicial:

$$
q_0
$$

Estado final:

$$
F=\{accept\}
$$

---

### Tabla de transiciones

| Desde | Lee | Escribe | Mov. | Hacia |
|---|---|---|---|---|
| q0 | 0 | 0 | R | q1 |
| q0 | 1 | 1 | R | q0 |
| q0 | □ | □ | S | reject |
| q1 | 0 | 0 | R | q1 |
| q1 | 1 | 1 | R | q2 |
| q1 | □ | □ | S | reject |
| q2 | 0 | 0 | R | q1 |
| q2 | 1 | 1 | R | q0 |
| q2 | □ | □ | S | accept |

### Idea de funcionamiento

La máquina recorre la palabra de izquierda a derecha y usa los estados como memoria:

- `q0`: no hay un `0` pendiente como posible comienzo del sufijo `01`.
- `q1`: el último símbolo leído fue `0`.
- `q2`: los últimos símbolos leídos fueron `01`.

La palabra se acepta únicamente si se alcanza el final de la cinta estando en `q2`.

---

## 2. Máquina de Turing para un Lenguaje Independiente del Contexto

Archivo: [`MT_LIC.jff`](MT_LIC.jff)

### Lenguaje

Sea:

$$
L_2=\{0^n1^{2n}\mid n\geq 1\}
$$

El lenguaje contiene cadenas formadas primero por `n` ceros y luego por exactamente el doble de unos.

Por cada `0` deben aparecer exactamente dos `1`.

### Ejemplos

Pertenecen al lenguaje:

```text
011
001111
000111111
000011111111
```

No pertenecen al lenguaje:

```text
01
0011
00111
0011111
01011
111
```

---

### Expresión Regular

Este lenguaje no posee una expresión regular clásica porque no es regular.

La condición:

$$
\#1=2\cdot \#0
$$

requiere mantener una relación entre cantidades arbitrarias de símbolos.

---

### Gramática Independiente del Contexto

Definimos:

$$
G=(V_N,V_T,S,P)
$$

con:

$$
V_N=\{S\}
$$

$$
V_T=\{0,1\}
$$

Axioma:

$$
S
$$

Producciones:

$$
S\rightarrow 0S11 \mid 011
$$

Cada aplicación recursiva agrega:

- un `0` a la izquierda;
- dos `1` a la derecha.

### Ejemplo de derivación para $n=1$

$$
S\Rightarrow 011
$$

### Ejemplo de derivación para $n=2$

$$
S\Rightarrow 0S11
\Rightarrow 0(011)11
\Rightarrow 001111
$$

### Ejemplo de derivación para $n=3$

$$
S\Rightarrow 0S11
\Rightarrow 00S1111
\Rightarrow 000111111
$$

---

### Máquina de Turing

Para reconocer el lenguaje se utilizan símbolos auxiliares:

- `X`: representa un `0` ya procesado.
- `Y`: representa un `1` ya procesado.

#### Alfabeto de entrada

$$
\Sigma=\{0,1\}
$$

#### Alfabeto de cinta

$$
\Gamma=\{0,1,X,Y,\square\}
$$

#### Estados

- `q0`: busca el próximo `0` sin procesar.
- `q1`: busca el primer `1` correspondiente a ese `0`.
- `q2`: busca el segundo `1` correspondiente al mismo `0`.
- `q3`: vuelve hacia el comienzo de la cinta.
- `q4`: verifica que no queden `1` sin procesar.
- `accept`: estado de aceptación.
- `reject`: estado de rechazo.

Estado inicial:

$$
q_0
$$

Estado final:

$$
F=\{accept\}
$$

---

### Tabla de transiciones

| Desde | Lee | Escribe | Mov. | Hacia |
|---|---|---|---|---|
| q0 | X | X | R | q0 |
| q0 | 0 | X | R | q1 |
| q0 | Y | Y | R | q4 |
| q0 | 1 | 1 | S | reject |
| q0 | □ | □ | S | reject |
| q1 | 0 | 0 | R | q1 |
| q1 | Y | Y | R | q1 |
| q1 | 1 | Y | R | q2 |
| q1 | □ | □ | S | reject |
| q2 | Y | Y | R | q2 |
| q2 | 1 | Y | L | q3 |
| q2 | 0 | 0 | S | reject |
| q2 | □ | □ | S | reject |
| q3 | 0 | 0 | L | q3 |
| q3 | 1 | 1 | L | q3 |
| q3 | X | X | L | q3 |
| q3 | Y | Y | L | q3 |
| q3 | □ | □ | R | q0 |
| q4 | Y | Y | R | q4 |
| q4 | 1 | 1 | S | reject |
| q4 | 0 | 0 | S | reject |
| q4 | □ | □ | S | accept |

### Idea de funcionamiento

La máquina realiza el siguiente procedimiento:

1. Busca un `0` sin procesar.
2. Lo reemplaza por `X`.
3. Busca un primer `1` y lo reemplaza por `Y`.
4. Busca un segundo `1` y lo reemplaza por `Y`.
5. Regresa al comienzo de la cinta.
6. Repite el proceso.
7. Cuando no quedan `0` sin procesar, verifica que tampoco queden `1` sin procesar.
8. Si todo quedó correctamente emparejado, acepta.

Ejemplo:

```text
001111
↓
X0YY11
↓
XXYYYY
↓
ACCEPT
```

Por cada `X` deben existir exactamente dos `Y`.

---

## Resumen

| Característica | Lenguaje Regular | Lenguaje Independiente del Contexto |
|---|---|---|
| Lenguaje | $w$ termina en `01` | $0^n1^{2n}$ |
| Alfabeto | `{0,1}` | `{0,1}` |
| ER | $(0 \mid 1)^*01$ | No posee ER regular |
| Gramática | Regular | Independiente del contexto |
| Símbolos auxiliares en la MT | No necesita | `X`, `Y` |
| Movimiento principal | Hacia la derecha | Hacia derecha e izquierda |
| Uso de la cinta como memoria | No necesario | Sí |
| Estrategia | Recordar el sufijo mediante estados | Emparejar cada `0` con dos `1` |

---

## 3. Comparativo entre MTaccept y MTcalc

### MTaccept

Una Máquina de Turing aceptadora se utiliza para reconocer o decidir lenguajes.

Recibe una cadena de entrada y finaliza indicando si pertenece o no al lenguaje:

- `accept`
- `reject`

Ejemplo, sobre el lenguaje de la sección 2:

$$
L_2=\{0^n1^{2n}\mid n\geq 1\}
$$

| Entrada | Resultado |
|---|---|
| `001111` | `accept` |
| `00111` | `reject` |

---

### MTcalc

Una Máquina de Turing calculadora se utiliza para realizar una transformación o cálculo sobre la entrada.

El resultado importante no es `accept` o `reject`, sino el contenido final de la cinta.

Ejemplo: duplicar la cantidad de unos.

$$
f(1^n)=1^{2n}
$$

| Entrada | Salida en la cinta |
|---|---|
| `11` | `1111` |
| `111` | `111111` |

---

### Comparación

| Característica | MTaccept | MTcalc |
|---|---|---|
| Finalidad | Reconocer un lenguaje | Calcular o transformar |
| Salida | `accept` / `reject` | Resultado en la cinta |
| Modificación de cinta | Puede utilizarla como memoria | Se utiliza para producir la salida |
| Interesa la cinta final | No necesariamente | Sí |
