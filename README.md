# Parcial de Arquitectura de HardWare
Por Diego Collazos y José David Jayk Vanegas


## Función de raices de un polinomio de grado 2 

(Mirar esta funcionalidad bien ejecutada en el siguiente repositorio: https://github.com/DiegoColBdz/Raices-Parcial2023.git)
(Nos falto corregirlo en este repositorio)

```c++
char inputBuffer[50];
int bufferIndex = 0;
bool recording = false;
```
Aqui declaramos variables para manejar la entrada de coeficientes.
`inputBuffer` es un arreglo de caracteres que almacenará la entrada del usuario.
`bufferIndex` es un índice para rastrear la posición actual en `inputBuffer`.
`recording` es una bandera que indica si estamos en modo de grabación de coeficientes.

```c++
    unsigned int coeficientes[3];
    int numeroActual = 0;
```
Creamos un arreglo `coefficients` para almacenar los coeficientes a, b, y c del polinomio de grado 2.
`coefficientIndex` es un índice para rastrear qué coeficiente se está ingresando actualmente.
```c++
void calculateRoots(int a, int b, int c) {
    int discriminant = b * b - 4 * a * c;
    if (discriminant > 0) {
        // Dos raíces enteras
        int root1 = (-b + sqrt(discriminant)) / (2 * a);
        int root2 = (-b - sqrt(discriminant)) / (2 * a);
        printf("Raíces enteras: %d y %d\n", root1, root2);
    } else if (discriminant == 0) {
        // Una raíz real
        int root = -b / (2 * a);
        printf("Raíz entera única: %d\n", root);
    } else {
        // Raíces complejas (no aplicable en este caso)
        printf("El polinomio no tiene raíces enteras.\n");
    }
}
```
Definimos una función `calculateRoots` que toma tres coeficientes enteros `a, b y c` y calcula las raíces del polinomio de grado 2 utilizando la fórmula cuadrática o mejor conocida como formula del estudiante. La función verifica si el discriminante es mayor que 0 (dos raíces reales), igual a 0 (una raíz real) o menor que 0 (raíces complejas), y muestra las raíces en la pantalla.

```c++
void processKey(char key) {
    if (recording) {
        // Verificamos si estamos en modo de grabación
        if (key == '*') {
            // Si el usuario presiona '*', indica que ha terminado de ingresar el coeficiente actual
            inputBuffer[bufferIndex] = '\0';  // Null-terminate the string
            printf("Coeficiente %c: %s\n", 'a' + coefficientIndex, inputBuffer);

            // Analizamos y almacenamos el coeficiente ingresado en la matriz 'coefficients'
            if (sscanf(inputBuffer, "%d", &coefficients[coefficientIndex]) == 1) {
                // La función 'sscanf' analiza el contenido de 'inputBuffer' y extrae un número entero
                coefficientIndex++;
                if (coefficientIndex < 3) {
                    // Si todavía hay coeficientes por ingresar, solicitamos el siguiente
                    printf("Por favor, ingrese el coeficiente %c: ", 'a' + coefficientIndex);
                    bufferIndex = 0;  // Reiniciamos el índice del búfer para el próximo coeficiente
                } else {
                    // Si se han ingresado todos los coeficientes, mostramos un mensaje con los coeficientes
                    printf("Coeficientes ingresados: a=%d, b=%d, c=%d\n", coefficients[0], coefficients[1], coefficients[2]);

                    // Calculamos las raíces llamando a la función 'calculateRoots' con los coeficientes
                    calculateRoots(coefficients[0], coefficients[1], coefficients[2]);

                    coefficientIndex = 0;  // Reiniciamos el índice de coeficiente para futuras entradas
                    recording = false;    // Salimos del modo de grabación
                }
            } else {
                // Si la entrada no es un número válido, mostramos un mensaje de error
                printf("Entrada inválida. Por favor, ingrese un número válido.\n");
                bufferIndex = 0;  // Reiniciamos el índice del búfer para ingresar nuevamente el coeficiente
            }
        } else {
            // Si no se presiona '*', significa que estamos ingresando dígitos del coeficiente actual
            inputBuffer[bufferIndex++] = key;  // Almacenamos el dígito en el búfer
        }
    } else {
        // Si no estamos en modo de grabación y el usuario presiona '*', iniciamos la grabación
        if (key == '*') {
            recording = true;  // Entramos en modo de grabación
            printf("Por favor, ingrese el coeficiente %c: ", 'a' + coefficientIndex);  // Solicitamos el primer coeficiente
            bufferIndex = 0;  // Reiniciamos el índice del búfer para la nueva entrada
        }
    }
}
```
1. La función `processKey` se encarga de manejar las teclas presionadas por el usuario y procesar la entrada de coeficientes.
2. La variable `recording` se utiliza para rastrear si estamos en modo de grabación, es decir, si el usuario está ingresando un coeficiente.
3. Si estamos en modo de grabación `(recording == true)`, la función verifica si se ha presionado la tecla *. Si es así, significa que el usuario ha terminado de ingresar el coeficiente actual y se procede a procesar y almacenar ese coeficiente.
4. Se utiliza `inputBuffer` para almacenar temporalmente la entrada del usuario como una cadena de caracteres. Cuando se presiona *, se agrega un carácter nulo ('\0') al final de la cadena para convertirla en una cadena de caracteres válida.
5. Se utiliza la función `sscanf` para analizar la cadena `inputBuffer` y extraer un número entero. Si la conversión tiene éxito `(retorna 1)`, el coeficiente se almacena en el arreglo `coefficients` en la posición correspondiente (`coefficientIndex`) y se incrementa `coefficientIndex.`
6. Si todavía hay coeficientes por ingresar `(coefficientIndex < 3)`, se solicita al usuario que ingrese el siguiente coeficiente y se reinicia `bufferIndex` para comenzar a recopilar la entrada nuevamente.
7. Si se han ingresado todos los coeficientes `(coefficientIndex == 3)`, se muestra un mensaje con los coeficientes ingresados, se calculan las raíces llamando a `calculateRoots`, se reinicia `coefficientIndex` y se sale del modo de grabación
8. Si la entrada del usuario no es un número válido `(la conversión con sscanf falla)`, se muestra un mensaje de error y se reinicia `bufferIndex` para que el usuario pueda ingresar nuevamente el coeficiente.
9. Si no estamos en modo de grabación `(recording == false)` y el usuario presiona *, comenzamos el proceso de grabación iniciando `recording`, mostrando un mensaje para ingresar el primer coeficiente y reiniciando `bufferIndex` para la nueva entrada.

En sintesis `processKey` maneja la entrada del usuario para que pueda ingresar coeficientes uno por uno, verificar la validez de la entrada y calcular las raíces una vez que todos los coeficientes han sido ingresados correctamente.

```c++
int main() {
    printf("Ingrese los coeficientes del polinomio de grado 2.\n");
    while (true) {
        for (int i = 0; i < numRows; i++) {
            rowPins[i] = 0;
            
            for (int j = 0; j < numCols; j++) {
                if (!colPins[j]) {
                    processKey(keyMap[i][j]);
                    ThisThread::sleep_for(500ms);  // Evita lecturas múltiples mientras la tecla está presionada
                }
            }

            rowPins[i] = 1;
        }
    }
}
```
En la función principal `main`, inicializamos el programa mostrando un mensaje "Ingrese los coeficientes del polinomio de grado 2". Luego, entramos en un bucle infinito que escanea las teclas del teclado matricial. Para cada fila y columna, llamamos a `processKey` para procesar las teclas presionadas y evitar lecturas múltiples mientras una tecla está presionada.
