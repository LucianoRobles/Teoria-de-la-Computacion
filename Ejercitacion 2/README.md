# Ejercitación 2

## Ejercicio 1 — Duplicar la cantidad de unos

Archivo: [`Ejercicio_01.jff`](Ejercicio_01.jff)

Máquina de Turing **calculadora** (MTcalc): lo que interesa no es `accept` / `reject`,
sino el contenido final de la cinta.

### Lenguaje

Dominio de entrada:

$$
L=\{1^n \mid n\geq 0\}
$$

Función calculada:

$$
f(1^n)=1^{2n}
$$

Por cada `1` de la entrada, la cinta termina con dos `1`.

| Entrada | Salida en la cinta |
|---|---|
| `1` | `11` |
| `11` | `1111` |
| `111` | `111111` |
| `λ` | `λ` |

### Producciones

Gramática regular del dominio de entrada:

$$
S\rightarrow 1S \mid \lambda
$$

Gramática regular del conjunto de salidas posibles:

$$
S\rightarrow 11S \mid \lambda
$$

### Máquina de Turing

- Alfabeto de entrada: $\Sigma=\{1\}$
- Alfabeto de cinta: $\Gamma=\{0,1,\square\}$
- Estado inicial: `p`
- Estado final: `s`

El `0` no es un símbolo de entrada: se usa como **marca auxiliar** para señalar
un `1` ya procesado.

Estados:

- `p`: busca hacia la izquierda el próximo `1` sin procesar.
- `q`: recorre hasta el final de la cinta para agregar la marca del par.
- `r`: convierte todas las marcas `0` en `1`.
- `s`: estado final.

### Transiciones

| Desde | Lee | Escribe | Mov. | Hacia |
|---|---|---|---|---|
| p | 1 | 0 | R | q |
| p | 0 | 0 | L | p |
| p | □ | □ | R | r |
| q | 0 | 0 | R | q |
| q | 1 | 1 | R | q |
| q | □ | 0 | L | p |
| r | 0 | 1 | R | r |
| r | □ | □ | S | s |

### Idea de funcionamiento

1. `p` lee un `1`, lo marca como `0` y pasa a `q`.
2. `q` avanza hasta el final de la cinta y escribe un `0` extra (el segundo del par).
3. Vuelve a `p`, que retrocede sobre las marcas hasta encontrar otro `1` sin procesar.
4. Cuando `p` llega al blanco de la izquierda ya no quedan `1`: la cinta tiene `2n` marcas.
5. `r` recorre hacia la derecha convirtiendo cada `0` en `1` y termina en `s`.

### Traza de ejemplo

Entrada `11`. El símbolo entre corchetes es la celda bajo el cabezal:

```text
p:  [1]1
q:  0[1]
q:  01[□]
p:  0[1]0
q:  00[0]
q:  000[□]
p:  00[0]0
p:  0[0]00
p:  [0]000
p:  [□]0000
r:  [0]000
r:  1[0]00
r:  11[0]0
r:  111[0]
r:  1111[□]
s:  1111
```

Resultado: `1111`.
