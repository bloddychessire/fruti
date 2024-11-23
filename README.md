package interfaz;

import controlador.Controlador;

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Controlador controlador = new Controlador();

        System.out.println("Ingrese su nombre:");
        String nombre = scanner.nextLine();

        System.out.println("Ingrese su carnet:");
        String carnet = scanner.nextLine();

        System.out.println("Seleccione su carrera:\n1. Ingeniería en Telecomunicaciones\n2. Ingeniería en Sistemas");
        int opcion = scanner.nextInt();
        scanner.nextLine(); // Limpiar buffer

        String carrera = opcion == 1 ? "Telecomunicaciones" : "Sistemas";
        List<String> materiasDisponibles = controlador.listarMaterias(carrera);
        List<String> materiasSeleccionadas = new ArrayList<>();

        System.out.println("Materias disponibles para " + carrera + ":");
        for (int i = 0; i < materiasDisponibles.size(); i++) {
            System.out.println((i + 1) + ". " + materiasDisponibles.get(i));
        }

        boolean inscribir = true;
        while (inscribir) {
            System.out.println("Ingrese el número de la materia que desea inscribir (0 para finalizar):");
            int seleccion = scanner.nextInt();
            scanner.nextLine(); // Limpiar buffer

            if (seleccion == 0) {
                if (!controlador.validarInscripcion(materiasSeleccionadas)) {
                    System.out.println("Debes inscribir al menos una materia antes de salir.");
                } else {
                    inscribir = false;
                }
            } else if (seleccion > 0 && seleccion <= materiasDisponibles.size()) {
                String materiaSeleccionada = materiasDisponibles.get(seleccion - 1);
                if (materiasSeleccionadas.contains(materiaSeleccionada)) {
                    System.out.println("Ya has inscrito esta materia.");
                } else {
                    String horario = controlador.asignarHorario();
                    materiasSeleccionadas.add(materiaSeleccionada + " | Horario: " + horario);
                    System.out.println("Materia inscrita: " + materiaSeleccionada + " | Horario: " + horario);
                }
            } else {
                System.out.println("Opción inválida. Intente de nuevo.");
            }
        }

        System.out.println("\nFelicidades, has inscrito las siguientes materias:");
        for (String materia : materiasSeleccionadas) {
            System.out.println("- " + materia);
        }
    }
}
