# Manual de Desarrollo: PsykoDeck 🃏✨
---

## Diseño de la Base de Datos 🗄️

### 👤 Área: Usuario y Progreso

#### **Tabla INSIGNIAS**
> Almacena los logros e insignias desbloqueables que los usuarios pueden equipar en su perfil.

| Campo | Tipo / Restricciones | Descripción |
| :--- | :--- | :--- |
| **`ID_INSIGNIA`** 🔑 | `INT` **(PK, AUTO_INCREMENT)** | Identificador único de la insignia. |
| **`NOMBRE`** | `VARCHAR(50)` **(NOT NULL)** | Nombre descriptivo de la insignia. |
| **`DESCRIPCION`** | `TEXT` | Requisitos de obtención o texto expositivo. |

<br>

#### **Tabla USUARIOS**
> Registra la información de cuenta, recursos, nivel de jugador y el tipo de ente (Humano o Bot IA).

| Campo | Tipo / Restricciones | Descripción |
| :--- | :--- | :--- |
| **`ID_USUARIO`** 🔑 | `INT` **(PK, AUTO_INCREMENT)** | Identificador único del usuario. |
| **`NOMBRE`** | `VARCHAR(25)` **(NOT NULL)** | Apodo o *nickname* de registro. |
| **`CORREO`** | `VARCHAR(100)` **(NOT NULL)** | Email de acceso y autenticación. |
| **`CONTR`** | `VARCHAR(255)` **(NOT NULL)** | Contraseña protegida mediante *hash*. |
| **`SOULD`** | `ENUM('AFTERLINE', 'REDLINE', 'BLACKLINE')` | Alma o afinidad seleccionada. |
| **`NIVEL`** | `INT` **(DEFAULT 1)** | Nivel global de avance. |
| **`EXPERIENCIA`** | `INT` **(DEFAULT 0)** | Puntos de experiencia (XP) acumulados. |
| **`TIPO`** | `ENUM('JUGADOR', 'BOT')` **(DEFAULT 'JUGADOR')** | Distingue entre cuentas humanas e IAs. |
| **`ID_INSIGNIA`** 🔗 | `INT` **(FK -> INSIGNIAS.ID_INSIGNIA, SET NULL)** | Insignia equipada actualmente. |

---

### 🎴 Área: Cartas y Catálogo

#### **Tabla CARTAS**
> Catálogo maestro. Contiene la ficha base y el coste de invocación de cada carta.

| Campo | Tipo / Restricciones | Descripción |
| :--- | :--- | :--- |
| **`ID_CARTA`** 🔑 | `INT` **(PK, AUTO_INCREMENT)** | Identificador único en el catálogo base. |
| **`NOMBRE`** | `VARCHAR(50)` **(NOT NULL)** | Nombre oficial de la carta. |
| **`COSTE`** | `INT` **(NOT NULL)** | Coste en *Psique* necesario para jugarla. |
| **`TIPO`** | `ENUM('CRIATURA', 'EFECTO')` | Clasificación general de la carta. |

<br>

#### **Tabla TIPOS**
> Define las facciones, escuelas o elementos a los que se filian las criaturas.

| Campo | Tipo / Restricciones | Descripción |
| :--- | :--- | :--- |
| **`ID_TIPO`** 🔑 | `INT` **(PK, AUTO_INCREMENT)** | Identificador del tipo o facción. |
| **`NOMBRE`** | `VARCHAR(25)` **(NOT NULL)** | Nombre de la facción (ej. `PAIN`, `FAULT`, `EMPTY`). |

<br>

#### **Tabla CRIATURAS**
> Estadísticas de combate y referencias visuales para las cartas de tipo Criatura.

| Campo | Tipo / Restricciones | Descripción |
| :--- | :--- | :--- |
| **`ID_CRIATURA`** 🔑 | `INT` **(PK, AUTO_INCREMENT)** | Identificador único de la entidad. |
| **`NOMBRE`** | `VARCHAR(50)` **(NOT NULL)** | Nombre específico de la criatura. |
| **`ATAQUE`** ⚔️ | `INT` **(NOT NULL)** | Potencia de ataque base. |
| **`VIDA`** 💚 | `INT` **(NOT NULL)** | Puntos de salud base. |
| **`VELOCIDAD`** ⚡ | `INT` **(NOT NULL)** | Prioridad e iniciativa en el turno. |
| **`IMAGEN`** 🖼️ | `VARCHAR(255)` **(NOT NULL)** | Ruta del arte (`512x512px`, `.webp`). |
| **`ID_TIPO`** 🔗 | `INT` **(FK -> TIPOS.ID_TIPO)** | Facción a la que pertenece. |
| **`ID_CARTA`** 🔗 | `INT` **(FK -> CARTAS.ID_CARTA, CASCADE)** | Enlace a su ficha general de catálogo. |

<br>

#### **Tabla EFECTO**
> Reglas y habilidades especiales ejecutadas por las cartas de tipo Efecto / Hechizo.

| Campo | Tipo / Restricciones | Descripción |
| :--- | :--- | :--- |
| **`ID_EFECTO`** 🔑 | `INT` **(PK, AUTO_INCREMENT)** | Identificador único del efecto. |
| **`NOMBRE`** | `VARCHAR(50)` **(NOT NULL)** | Nombre del efecto o hechizo. |
| **`DESCRIPCION`** | `TEXT` **(NOT NULL)** | Lógica o regla que aplica al jugarse. |
| **`IMAGEN`** 🖼️ | `VARCHAR(255)` | Ruta del arte (`512x512px`, `.webp`). |
| **`ID_CARTA`** 🔗 | `INT` **(FK -> CARTAS.ID_CARTA, CASCADE)** | Enlace a su ficha general de catálogo. |

---

### 🏙️ Área: Escenarios

#### **Tabla ESCENARIOS**
> Campos de batalla donde se desarrollan los duelos, incluyendo fondos e historia.

| Campo | Tipo / Restricciones | Descripción |
| :--- | :--- | :--- |
| **`ID_ESCENARIO`** 🔑 | `INT` **(PK, AUTO_INCREMENT)** | Identificador único del escenario. |
| **`NOMBRE`** | `VARCHAR(50)` **(NOT NULL)** | Nombre del terreno de combate. |
| **`CONCEPTO`** | `TEXT` **(NOT NULL)** | Contexto ambiental o *lore* de la zona. |
| **`IMAGEN`** 🖼️ | `VARCHAR(255)` **(NOT NULL)** | Ruta del fondo web (`1920x1080px`, `.webp`). |

---

### ⚔️ Área: Partida y Estado Dinámico

#### **Tabla PARTIDA**
> Controla la sesión activa entre un jugador humano y la IA o un rival.

| Campo | Tipo / Restricciones | Descripción |
| :--- | :--- | :--- |
| **`ID_PARTIDA`** 🔑 | `INT` **(PK, AUTO_INCREMENT)** | Identificador de la sesión activa. |
| **`TURNO`** ⏱️ | `INT` **(DEFAULT 1)** | Contador incremental de turnos transcurridos. |
| **`USUARIO1`** 🔗 | `INT` **(FK -> USUARIOS.ID_USUARIO)** | Jugador humano (Host de la sesión). |
| **`USUARIO2`** 🔗 | `INT` **(FK -> USUARIOS.ID_USUARIO)** | Oponente (Bot IA o Rival). |
| **`ID_ESCENARIO`** 🔗 | `INT` **(FK -> ESCENARIOS.ID_ESCENARIO)** | Campo de batalla activo en el enfrentamiento. |

<br>

#### **Tabla INSTANCIA**
> Mapea la ubicación exacta y en tiempo real de cada carta dentro del flujo de la partida.

| Campo | Tipo / Restricciones | Descripción |
| :--- | :--- | :--- |
| **`ID_INSTANCIA`** 🔑 | `INT` **(PK, AUTO_INCREMENT)** | Identificador único de la carta física en combate. |
| **`ID_PARTIDA`** 🔗 | `INT` **(FK -> PARTIDA.ID_PARTIDA, CASCADE)** | Partida activa a la que está vinculada. |
| **`ID_USUARIO`** 🔗 | `INT` **(FK -> USUARIOS.ID_USUARIO)** | Propietario actual de la copia (P1 o P2). |
| **`ID_CARTA`** 🔗 | `INT` **(FK -> CARTAS.ID_CARTA)** | Referencia al catálogo maestro. |
| **`ESTADO`** 📍 | `ENUM('MAZO', 'MANO', 'TABLERO', 'CEMENTERIO')` | Ubicación actual de la carta en mesa. |

---

> **Leyenda de Claves:**  
> 🔑 **PK**: Clave Primaria (*Primary Key*) | 🔗 **FK**: Clave Foránea (*Foreign Key*)
___

## ¿Quién hace qué? 🏗️

Para garantizar que el juego sea **rápido, fluido y 100% seguro contra trampas**, **PsykoDeck** se divide en tres capas bien diferenciadas:

* **Frontend (Angular - El Escenario Visual) 💻:** Es la pantalla con la que interactúa el usuario. Gestiona la interfaz gráfica, el arrastre de cartas (*drag & drop*), las animaciones de ataque, los efectos de sonido y la renderización en tiempo real del tablero.
* **Backend (Servidor REST / WebSockets - El Árbitro) 🧠:** Es el cerebro de la partida. Recibe las intenciones del jugador, valida que las jugadas sean legales (comprueba si hay suficiente energía, si es el turno correcto, etc.) y ejecuta la Inteligencia Artificial del Bot oponente.
* **Base de Datos (SQL - El Registro Oficial) 🗄️:** Guarda la **verdad absoluta del duelo**. Registra de forma inmutable la posición y estado de cada carta (`INSTANCIA`). Si el jugador pierde la conexión o refresca el navegador (`F5`), el estado de la mesa se reconstruye al instante sin perder el progreso.

---

## Flujo de Estados de las Cartas (`INSTANCIA`) 🃏

Durante el transcurso de un combate, cada carta no es solo un elemento visual; representa un registro dinámico en la base de datos que evoluciona a través de **cuatro zonas estratégicas**:

* **`[ MAZO ]` 🎴**  
  * **Transición inicial:** *Creación de Partida.*
  * **Detalle:** La carta pertenece a la baraja oculta de un jugador. En este estado no se muestran sus detalles al oponente para mantener la estrategia en secreto.
  ↓
* **`[ MANO ]` 🖐️**  
  * **Transición de entrada:** *Robar Carta.*
  * **Detalle:** Al comienzo del turno, la carta pasa a la mano del jugador activo. Desde aquí puede ser inspeccionada y queda lista para jugarse en cuanto se disponga del coste de *Psique* necesario.
  ↓
* **`[ TABLERO ]` ⚔️**  
  * **Transición de entrada:** *Pagar Psique e Invocación.*
  * **Detalle:** La carta entra activamente al campo de batalla. Si es de tipo **Criatura**, se posiciona para atacar o defender; si es de tipo **Efecto**, ejecuta su habilidad mágica o técnica en ese instante.
  ↓
* **`[ CEMENTERIO ]` 🪦**  
  * **Transición de entrada:** *Puntos de Vida <= 0 / Efecto Consumido.*
  * **Detalle:** Es la zona de descarte. Una criatura se envía al cementerio cuando su salud cae a 0 tras un combate, mientras que los hechizos o efectos van aquí una vez resuelta su acción.

---

## Ciclo de Fase del Turno ⏱️

Un turno completo en **PsykoDeck** se procesa automáticamente siguiendo una secuencia rigurosa de **cuatro fases interconectadas**:

1. **Fase 1: Recarga y Robo (Inicio de Turno) 🔄**
   * El servidor incrementa el contador general de `TURNO` en la tabla `PARTIDA`.
   * Se recarga y asigna el límite de recurso *Psique* 💎 disponible para el jugador activo.
   * El sistema selecciona la primera carta disponible con estado `'MAZO'` en la tabla `INSTANCIA` y la actualiza a `'MANO'`.

2. **Fase 2: Despliegue (Fase Principal) 🚀**
   * El usuario selecciona y arrastra una carta de su mano al tablero en Angular.
   * Angular envía la petición al Backend.
   * El Backend valida el coste en *Psique*: si la jugada es válida, resta la energía y actualiza el registro en `INSTANCIA` de `'MANO'` a `'TABLERO'`.

3. **Fase 3: Resolución y Combate 💥**
   * El sistema calcula el orden de iniciativa de las criaturas en el `'TABLERO'` basándose en su estadística de **`VELOCIDAD`** ⚡.
   * Se resuelven los ataques restando el valor de **`ATAQUE`** ⚔️ del agresor a los puntos de **`VIDA`** 💚 de la criatura oponente.
   * Si la salud de una criatura llega a 0, la base de datos cambia inmediatamente el estado de esa instancia a `'CEMENTERIO'`.

4. **Fase 4: Final del Turno 🏁**
   * Se comprueba si la vida total de alguno de los dos jugadores ha llegado a 0 para declarar un ganador 🏆.
   * Si ambos continúan en pie, el servidor cede el control del turno al oponente (o activa el turno de la IA) y el ciclo se reinicia.
