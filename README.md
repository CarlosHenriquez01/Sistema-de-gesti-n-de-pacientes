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



CÓDIGO - SISTEMA DE GESTIÓN DE PACIENTES.
using System;
using System.Collections.Generic;
using System.Linq;

namespace SistemaGestionPacientes
{
    // Clase modelo que representa un paciente
    public class Paciente
    {
        public string Id { get; set; }          // Identificador único (cédula o ID)
        public string Nombre { get; set; }
        public int Edad { get; set; }
        public string Sexo { get; set; }
        public string Diagnostico { get; set; }
        public DateTime FechaIngreso { get; set; }

        public override string ToString()
        {
            return $"ID: {Id}, Nombre: {Nombre}, Edad: {Edad}, Sexo: {Sexo}, Diagnóstico: {Diagnostico}, Fecha Ingreso: {FechaIngreso.ToShortDateString()}";
        }
    }

    // Clase encargada de la lógica CRUD sobre la lista de pacientes
    public class GestorPacientes
    {
        private List<Paciente> pacientes = new List<Paciente>();

        // Crear (Alta)
        public bool AgregarPaciente(Paciente nuevoPaciente)
        {
            if (pacientes.Any(p => p.Id == nuevoPaciente.Id))
            {
                return false; // ID duplicado
            }
            pacientes.Add(nuevoPaciente);
            return true;
        }

        // Leer (Consulta) - Listar todos
        public List<Paciente> ListarPacientes()
        {
            return pacientes;
        }

        // Buscar por ID o nombre
        public Paciente BuscarPaciente(string criterio)
        {
            return pacientes.FirstOrDefault(p => p.Id.Equals(criterio, StringComparison.OrdinalIgnoreCase)
                                             || p.Nombre.Equals(criterio, StringComparison.OrdinalIgnoreCase));
        }

        // Actualizar datos
        public bool ActualizarPaciente(string id, Paciente pacienteActualizado)
        {
            var paciente = pacientes.FirstOrDefault(p => p.Id == id);
            if (paciente == null) return false;

            paciente.Nombre = pacienteActualizado.Nombre;
            paciente.Edad = pacienteActualizado.Edad;
            paciente.Sexo = pacienteActualizado.Sexo;
            paciente.Diagnostico = pacienteActualizado.Diagnostico;
            paciente.FechaIngreso = pacienteActualizado.FechaIngreso;
            return true;
        }

        // Eliminar paciente
        public bool EliminarPaciente(string id)
        {
            var paciente = pacientes.FirstOrDefault(p => p.Id == id);
            if (paciente == null) return false;

            pacientes.Remove(paciente);
            return true;
        }
    }

    class Program
    {
        static GestorPacientes gestor = new GestorPacientes();

        static void Main(string[] args)
        {
            bool salir = false;
            while (!salir)
            {
                MostrarMenu();
                string opcion = Console.ReadLine();

                switch (opcion)
                {
                    case "1":
                        RegistrarPaciente();
                        break;
                    case "2":
                        ListarPacientes();
                        break;
                    case "3":
                        BuscarPaciente();
                        break;
                    case "4":
                        ActualizarPaciente();
                        break;
                    case "5":
                        EliminarPaciente();
                        break;
                    case "6":
                        salir = true;
                        Console.WriteLine("Saliendo del sistema...");
                        break;
                    default:
                        Console.WriteLine("Opción inválida. Intente de nuevo.");
                        break;
                }
            }
        }

        static void MostrarMenu()
        {
            Console.WriteLine("\n--- Sistema de Gestión de Pacientes ---");
            Console.WriteLine("1. Registrar nuevo paciente");
            Console.WriteLine("2. Listar todos los pacientes");
            Console.WriteLine("3. Buscar paciente por ID o nombre");
            Console.WriteLine("4. Actualizar datos de un paciente");
            Console.WriteLine("5. Eliminar un paciente");
            Console.WriteLine("6. Salir");
            Console.Write("Seleccione una opción: ");
        }

        static void RegistrarPaciente()
        {
            do
            {
                Console.WriteLine("\n--- Registrar Nuevo Paciente ---");

                Console.Write("ID: ");
                string id = Console.ReadLine();

                if (string.IsNullOrWhiteSpace(id))
                {
                    Console.WriteLine("El ID no puede estar vacío.");
                    continue;
                }

                if (gestor.BuscarPaciente(id) != null)
                {
                    Console.WriteLine("Ya existe un paciente con ese ID.");
                    continue;
                }

                Console.Write("Nombre completo: ");
                string nombre = Console.ReadLine();

                Console.Write("Edad: ");
                if (!int.TryParse(Console.ReadLine(), out int edad) || edad <= 0)
                {
                    Console.WriteLine("Edad inválida.");
                    continue;
                }

                Console.Write("Sexo (M/F): ");
                string sexo = Console.ReadLine();

                Console.Write("Diagnóstico: ");
                string diagnostico = Console.ReadLine();

                DateTime fechaIngreso = DateTime.Now;

                Paciente nuevoPaciente = new Paciente
                {
                    Id = id,
                    Nombre = nombre,
                    Edad = edad,
                    Sexo = sexo,
                    Diagnostico = diagnostico,
                    FechaIngreso = fechaIngreso
                };

                if (gestor.AgregarPaciente(nuevoPaciente))
                {
                    Console.WriteLine("Paciente registrado correctamente.");
                }
                else
                {
                    Console.WriteLine("Error al registrar paciente.");
                }

                Console.Write("¿Desea registrar otro paciente? (S/N): ");
            } while (Console.ReadLine().Trim().ToUpper() == "S");
        }

        static void ListarPacientes()
        {
            Console.WriteLine("\n--- Lista de Pacientes ---");
            var lista = gestor.ListarPacientes();
            if (lista.Count == 0)
            {
                Console.WriteLine("No hay pacientes registrados.");
            }
            else
            {
                foreach (var paciente in lista)
                {
                    Console.WriteLine(paciente);
                }
            }
            Console.WriteLine("Presione Enter para continuar...");
            Console.ReadLine();
        }

        static void BuscarPaciente()
        {
            do
            {
                Console.Write("\nIngrese ID o nombre del paciente a buscar: ");
                string criterio = Console.ReadLine();

                var paciente = gestor.BuscarPaciente(criterio);
                if (paciente == null)
                {
                    Console.WriteLine("Paciente no encontrado.");
                }
                else
                {
                    Console.WriteLine("Paciente encontrado:");
                    Console.WriteLine(paciente);
                }

                Console.Write("¿Desea buscar otro paciente? (S/N): ");
            } while (Console.ReadLine().Trim().ToUpper() == "S");
        }

        static void ActualizarPaciente()
        {
            do
            {
                Console.Write("\nIngrese el ID del paciente a actualizar: ");
                string id = Console.ReadLine();

                var paciente = gestor.BuscarPaciente(id);
                if (paciente == null)
                {
                    Console.WriteLine("Paciente no encontrado.");
                    continue;
                }

                Console.WriteLine("Ingrese los nuevos datos (deje vacío para no modificar):");

                Console.Write($"Nombre ({paciente.Nombre}): ");
                string nombre = Console.ReadLine();
                if (string.IsNullOrWhiteSpace(nombre)) nombre = paciente.Nombre;

                Console.Write($"Edad ({paciente.Edad}): ");
                string edadStr = Console.ReadLine();
                int edad = paciente.Edad;
                if (!string.IsNullOrWhiteSpace(edadStr))
                {
                    if (!int.TryParse(edadStr, out edad) || edad <= 0)
                    {
                        Console.WriteLine("Edad inválida. Se mantiene el valor anterior.");
                        edad = paciente.Edad;
                    }
                }

                Console.Write($"Sexo ({paciente.Sexo}): ");
                string sexo = Console.ReadLine();
                if (string.IsNullOrWhiteSpace(sexo)) sexo = paciente.Sexo;

                Console.Write($"Diagnóstico ({paciente.Diagnostico}): ");
                string diagnostico = Console.ReadLine();
                if (string.IsNullOrWhiteSpace(diagnostico)) diagnostico = paciente.Diagnostico;

                Paciente actualizado = new Paciente
                {
                    Id = paciente.Id,
                    Nombre = nombre,
                    Edad = edad,
                    Sexo = sexo,
                    Diagnostico = diagnostico,
                    FechaIngreso = paciente.FechaIngreso
                };

                if (gestor.ActualizarPaciente(id, actualizado))
                {
                    Console.WriteLine("Paciente actualizado correctamente.");
                }
                else
                {
                    Console.WriteLine("Error al actualizar paciente.");
                }

                Console.Write("¿Desea actualizar otro paciente? (S/N): ");
            } while (Console.ReadLine().Trim().ToUpper() == "S");
        }

        static void EliminarPaciente()
        {
            do
            {
                Console.Write("\nIngrese el ID del paciente a eliminar: ");
                string id = Console.ReadLine();

                var paciente = gestor.BuscarPaciente(id);
                if (paciente == null)
                {
                    Console.WriteLine("Paciente no encontrado.");
                    continue;
                }

                Console.WriteLine("Paciente encontrado:");
                Console.WriteLine(paciente);

                Console.Write("¿Confirma eliminar este paciente? (S/N): ");
                if (Console.ReadLine().Trim().ToUpper() == "S")
                {
                    if (gestor.EliminarPaciente(id))
                    {
                        Console.WriteLine("Paciente eliminado correctamente.");
                    }
                    else
                    {
                        Console.WriteLine("Error al eliminar paciente.");
                    }
                }
                else
                {
                    Console.WriteLine("Eliminación cancelada.");
                }

                Console.Write("¿Desea eliminar otro paciente? (S/N): ");
            } while (Console.ReadLine().Trim().ToUpper() == "S");
        }
    }
}
