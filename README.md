# Manual de Desarrollo De PsykoDeck 🃏✨

## 1º Diseño de la Base de Datos 🗄️

__Usuario__

### Tabla Insignias 
- Id_Insignia 
- Nombre
- Descripción


### Tabla Usuarios
- Id_Usuario
- Nombre
- Correo Electronico 
- Contr 
- Psique 
- Nivel
- Experiencia 
- Insignia **(FG)**

__Cartas__

### Tabla Cartas
- Id_Carta 
- Nombre
- Coste 
- TipoCarta

### Tabla Tipo
- Id_Tipo
- Nombre

### Tabla Criaturas
- Id_Criatura
- Nombre
- Tipo
- Ataque
- Vida 
- Velocidad 
- Imagen 
- Id_Tipo **(FK)**
- Id_Carta **(FK)**

### Tabla Efectos
- Nombre
- Descripcion
- Imagen 
- Id_Carta **(FG)**

__Escenarios__

### Tabla Escenarios 
- Id_Escenario 
- Nombre 
- Concepto 

__Partida__

### Tabla Partida 
- Id_Partida 
- Turno 
- Usuario1
- Usuario2
- Id_Escenario **(FK)**

### Tabla Instancia
- Id_Instancia 
- Id_Partida **(FK)**
- Id_Usuario **(FK)**
- Id_Carta  **(FK)**
- Estado 
