# Manual de Desarrollo: PsykoDeck 🃏✨

## 1. Diseño de la Base de Datos 🗄️

### 👤 Área: Usuario

#### **Tabla Insignias**
| Campo | Tipo / Restricción | Descripción |
| :--- | :--- | :--- |
| **`Id_Insignia`** | `INT` **(PK, AUTO_INCREMENT)** | Identificador único del logro o insignia. |
| **`Nombre`** | `VARCHAR(50)` **(NOT NULL)** | Nombre descriptivo de la insignia. |
| **`Descripción`** | `TEXT` | Requisitos para desbloquearla o detalle expositivo. |

#### **Tabla Usuarios**
| Campo | Tipo / Restricción | Descripción |
| :--- | :--- | :--- |
| **`Id_Usuario`** | `INT` **(PK, AUTO_INCREMENT)** | Identificador único del jugador. |
| **`Nombre`** | `VARCHAR(50)` **(NOT NULL, UNIQUE)** | Apodo o *nickname* dentro del juego. |
| **`Correo_Electronico`** | `VARCHAR(100)` **(NOT NULL, UNIQUE)** | Email de registro y acceso. |
| **`Contr`** | `VARCHAR(255)` **(NOT NULL)** | Contraseña cifrada mediante *hash*. |
| **`Psique`** | `INT` **(DEFAULT 0)** | Recurso o divisa acumulada. |
| **`Nivel`** | `INT` **(DEFAULT 1)** | Nivel global de progreso. |
| **`Experiencia`** | `INT` **(DEFAULT 0)** | Puntos de experiencia (XP) obtenidos. |
| **`Id_Insignia`** | `INT` **(FK -> Insignias.Id_Insignia)** | Insignia equipada actualmente en el perfil. |

---

### 🎴 Área: Cartas y Catálogo

#### **Tabla Cartas**
| Campo | Tipo / Restricción | Descripción |
| :--- | :--- | :--- |
| **`Id_Carta`** | `INT` **(PK, AUTO_INCREMENT)** | Identificador base en el catálogo general. |
| **`Nombre`** | `VARCHAR(50)` **(NOT NULL)** | Nombre oficial de la carta. |
| **`Coste`** | `INT` **(NOT NULL)** | Coste en energía/Psique necesario para jugarla. |
| **`TipoCarta`** | `ENUM('CRIATURA', 'EFECTO', 'ESCENARIO')` | Categoría principal a la que pertenece. |

#### **Tabla Tipo**
| Campo | Tipo / Restricción | Descripción |
| :--- | :--- | :--- |
| **`Id_Tipo`** | `INT` **(PK, AUTO_INCREMENT)** | Identificador de la facción o elemento. |
| **`Nombre`** | `VARCHAR(50)` **(NOT NULL, UNIQUE)** | Nombre del tipo (ej. `PAIN`, `FAULT`, `EMPTY`). |

#### **Tabla Criaturas**
| Campo | Tipo / Restricción | Descripción |
| :--- | :--- | :--- |
| **`Id_Criatura`** | `INT` **(PK, AUTO_INCREMENT)** | Identificador único de la entidad. |
| **`Nombre`** | `VARCHAR(50)` **(NOT NULL)** | Nombre específico de la criatura. |
| **`Ataque`** | `INT` **(NOT NULL)** | Valor base de potencia ofensiva (ATQ ⚔️). |
| **`Vida`** | `INT` **(NOT NULL)** | Valor base de puntos de salud (VIDA 💚). |
| **`Velocidad`** | `INT` **(NOT NULL)** | Prioridad e iniciativa en el turno (VEL ⚡). |
| **`Imagen`** | `VARCHAR(255)` | Ruta del arte (512x512px, `.webp`). |
| **`Id_Tipo`** | `INT` **(FK -> Tipo.Id_Tipo)** | Facción a la que está afiliada. |
| **`Id_Carta`** | `INT` **(FK -> Cartas.Id_Carta, UNIQUE)** | Enlace directo con su ficha general de catálogo. |

#### **Tabla Efectos**
| Campo | Tipo / Restricción | Descripción |
| :--- | :--- | :--- |
| **`Id_Efecto`** | `INT` **(PK, AUTO_INCREMENT)** | Identificador único del hechizo o habilidad. |
| **`Nombre`** | `VARCHAR(50)` **(NOT NULL)** | Nombre del efecto. |
| **`Descripcion`** | `TEXT` **(NOT NULL)** | Regla o mecánica que ejecuta la carta. |
| **`Imagen`** | `VARCHAR(255)` | Ruta del arte (512x512px, `.webp`). |
| **`Id_Carta`** | `INT` **(FK -> Cartas.Id_Carta, UNIQUE)** | Enlace directo con su ficha general de catálogo. |

---

### 🏙️ Área: Escenarios

#### **Tabla Escenarios**
| Campo | Tipo / Restricción | Descripción |
| :--- | :--- | :--- |
| **`Id_Escenario`** | `INT` **(PK, AUTO_INCREMENT)** | Identificador del terreno de combate. |
| **`Nombre`** | `VARCHAR(50)` **(NOT NULL)** | Nombre del escenario activo. |
| **`Concepto`** | `TEXT` | *Lore* o modificadores ambientales del campo. |
| **`Imagen_Fondo`** | `VARCHAR(255)` | Ruta del fondo *responsive* (1920x1080px, `.webp`). |

---

### ⚔️ Área: Partida

#### **Tabla Partida**
| Campo | Tipo / Restricción | Descripción |
| :--- | :--- | :--- |
| **`Id_Partida`** | `INT` **(PK, AUTO_INCREMENT)** | Identificador de la sesión de enfrentamiento en curso. |
| **`Turno`** | `INT` **(DEFAULT 1)** | Contador incremental del turno actual. |
| **`Usuario1`** | `INT` **(FK -> Usuarios.Id_Usuario)** | Jugador humano (propietario de la sesión). |
| **`Usuario2`** | `INT` **(FK -> Usuarios.Id_Usuario)** | Oponente predeterminado (Bot / IA). |
| **`Id_Escenario`** | `INT` **(FK -> Escenarios.Id_Escenario)** | Terreno de juego activo en la partida. |

#### **Tabla Instancia**
| Campo | Tipo / Restricción | Descripción |
| :--- | :--- | :--- |
| **`Id_Instancia`** | `INT` **(PK, AUTO_INCREMENT)** | Identificador único de la carta física en combate. |
| **`Id_Partida`** | `INT` **(FK -> Partida.Id_Partida)** | Partida a la que está vinculada la copia. |
| **`Id_Usuario`** | `INT` **(FK -> Usuarios.Id_Usuario)** | Propietario en turno (Jugador 1 o Bot). |
| **`Id_Carta`** | `INT` **(FK -> Cartas.Id_Carta)** | Referencia al catálogo para obtener estadísticas base. |
| **`Estado`** | `ENUM('MAZO', 'MANO', 'TABLERO', 'CEMENTERIO')` | Ubicación y zona dinámica en tiempo real. |
