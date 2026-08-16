Integrantes: Brandon Jesús Valdez Abreu 25-SISN-2-043 -- Carlos Dariel Henriquez Ramírez 25-SISN-2-040.


Descripción breve: El programa es un Sistema de Gestión de Pacientes en C# que permite administrar una lista de pacientes mediante un menú de consola. Su propósito es realizar operaciones CRUD: registrar, consultar, actualizar y eliminar pacientes.


Datos de entrada: El usuario ingresa los siguientes datos * ID * Nombre * Edad * Sexo * Diagnóstico.


Datos que procesa: El programa procesa los siguientes datos:
- El ID no puede estar vacío.
- No se permiten ID repetidos.
- La edad debe ser un número mayor que cero.
- Al actualizar, los campos vacíos mantienen la información anterior.
- El usuario puede buscar pacientes utilizando su ID o nombre. Si existe, se muestran sus datos; si no, aparece “Paciente no encontrado”
- Permite modificar nombre, edad, sexo y diagnóstico de un paciente existente. La fecha de ingreso se mantiene.
- El usuario busca al paciente por ID y debe confirmar la eliminación. Si responde S, se elimina; si responde N, se cancela.


Datos de salida: El programa muestra mensajes como:
* “Paciente registrado correctamente.”
* “Paciente encontrado.”
* “Paciente actualizado correctamente.”
* “Paciente eliminado correctamente.”
* “Paciente no encontrado.”
