# Práctica: Sincronización de Hilos en Java


## Descripción de la Práctica
Esta práctica explora los conceptos fundamentales de concurrencia y sincronización en Java, abordando condiciones de carrera, exclusión mutua, espera activa, semáforos y barreras de sincronización a través de cinco ejercicios prácticos basados en una variable compartida.


## Ejercicios y Mecanismos Utilizados

### 1. Suma sin sincronización (`SumaSinSincronizacion.java`)
* **Mecanismo:** Ninguno. Cuatro hilos incrementan 50,000 veces cada uno la variable estática compartida `x` sin mecanismos de control.
* **Resultado observado:** Se produce una condición de carrera. El valor obtenido es impredecible, menor al valor esperado ($200\,000$) y varía en cada ejecución debido a la pérdida de incrementos entre hilos.

### 2. Exclusión mutua con Lock (`SumaLock.java`)
* **Mecanismo:** `ReentrantLock` utilizando los métodos `lock()` y `unlock()` para proteger la sección crítica (`x++`).
* **Resultado observado:** Se garantiza la exclusión mutua, permitiendo que solo un hilo modifique la variable a la vez. El valor final obtenido es exactamente $200,000$ de forma determinista.

### 3. Spin Lock con tryLock (`SumaSpinLock.java`)
* **Mecanismo:** Espera activa (Spin Lock) construida mediante un ciclo `while` con `CANDADO.tryLock()` y optimizada con `Thread.onSpinWait()`.
* **Resultado observado:** Los hilos realizan un sondeo activo intentando adquirir el candado. Se logra la exclusión mutua y el valor final obtenido es exactamente $200,000.

### 4. Sincronización con semáforo (`SumaSemaforo.java`)
* **Mecanismo:** `Semaphore` configurado con un único permiso (`acquire()` y `release()`), operando como un semáforo binario.
* **Resultado observado:** Se controla de forma segura el acceso exclusivo a la sección crítica mediante permisos. El valor final obtenido es exactamente $200,000.

### 5. Sincronización por barrera (`SumaBarrera.java`)
* **Mecanismo:** `CyclicBarrier` combinada con un `ReentrantLock`. El candado protege el incremento de `x`, mientras que la barrera detiene a los hilos hasta que todos alcanzan el punto de encuentro.
* **Resultado observado:** Los hilos se sincronizan al concluir su fase de trabajo. La acción de barrera imprime correctamente que todos llegaron y muestra el valor acumulado de $200,000 antes de continuar.
* **Compilación y ejecución:**
  ```bash
  javac SumaSinSincronizacion.java
  Time java SumaSinSincronizacion
