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


package mundo;

import java.util.ArrayList;
import java.util.List;
import java.util.Random;

public class Sistema {
    private static Sistema instancia; // Singleton
    private List<String> horariosDisponibles;
    private List<String> materiasTelecomunicaciones;
    private List<String> materiasSistemas;

    private Sistema() {
        // Inicialización de horarios
        horariosDisponibles = new ArrayList<>();
        horariosDisponibles.add("Lunes 8-10");
        horariosDisponibles.add("Lunes 10-12");
        horariosDisponibles.add("Martes 8-10");
        horariosDisponibles.add("Martes 10-12");
        horariosDisponibles.add("Miércoles 8-10");
        horariosDisponibles.add("Jueves 8-10");
        horariosDisponibles.add("Viernes 8-10");

        // Inicialización de materias
        materiasTelecomunicaciones = List.of("Electrónica", "Estructuras de Datos", "Historia de las Culturas", "Ondas y Campos Electromagnéticos");
        materiasSistemas = List.of("Patrones GRASP", "Ondas y Campos Electromagnéticos", "Matemáticas Especiales TIC", "Matemáticas Aplicadas TIC");
    }

    public static Sistema getInstancia() {
        if (instancia == null) {
            instancia = new Sistema();
        }
        return instancia;
    }

    public List<String> getMaterias(String carrera) {
        return carrera.equals("Telecomunicaciones") ? materiasTelecomunicaciones : materiasSistemas;
    }

    public String asignarHorario() {
        if (horariosDisponibles.isEmpty()) {
            return "Sin horario disponible";
        }
        Random random = new Random();
        return horariosDisponibles.remove(random.nextInt(horariosDisponibles.size()));
    }
}

package controlador;

import mundo.Sistema;

import java.util.ArrayList;
import java.util.List;

public class Controlador {
    private Sistema sistema;

    public Controlador() {
        sistema = Sistema.getInstancia();
    }

    public List<String> listarMaterias(String carrera) {
        return sistema.getMaterias(carrera);
    }

    public String asignarHorario() {
        return sistema.asignarHorario();
    }

    public boolean validarInscripcion(List<String> materiasSeleccionadas) {
        return !materiasSeleccionadas.isEmpty();
    }
}

