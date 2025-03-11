    import java.io.*;
import java.util.*;

abstract class Animal {
    protected String nombre;
    protected int edad;
    protected String dieta;
    protected double consumoDiario; // Libras de alimento por día

    public Animal(String nombre, int edad, String dieta, double consumoDiario) {
        this.nombre = nombre;
        this.edad = edad;
        this.dieta = dieta;
        this.consumoDiario = consumoDiario;
    }

    public abstract void mostrarInfo();

    public double calcularConsumo(int dias) {
        if (dias == 0) {
            return 0;
        }
        return consumoDiario + calcularConsumo(dias - 1);
    }

    public String toCSV() {
        return nombre + "," + edad + "," + dieta + "," + consumoDiario;
    }
}

class Mamifero extends Animal {
    public Mamifero(String nombre, int edad, double consumoDiario) {
        super(nombre, edad, "Carnívoro", consumoDiario);
    }

    @Override
    public void mostrarInfo() {
        System.out.println("Mamífero - Nombre: " + nombre + ", Edad: " + edad + ", Dieta: " + dieta + ", Consumo Diario: " + consumoDiario + " lbs");
    }
}

class Ave extends Animal {
    public Ave(String nombre, int edad, double consumoDiario) {
        super(nombre, edad, "Herbívoro", consumoDiario);
    }

    @Override
    public void mostrarInfo() {
        System.out.println("Ave - Nombre: " + nombre + ", Edad: " + edad + ", Dieta: " + dieta + ", Consumo Diario: " + consumoDiario + " lbs");
    }
}

class Reptil extends Animal {
    public Reptil(String nombre, int edad, double consumoDiario) {
        super(nombre, edad, "Omnívoro", consumoDiario);
    }

    @Override
    public void mostrarInfo() {
        System.out.println("Reptil - Nombre: " + nombre + ", Edad: " + edad + ", Dieta: " + dieta + ", Consumo Diario: " + consumoDiario + " lbs");
    }
}

public class ZooManager {
    private static Scanner scanner = new Scanner(System.in);
    private static List<Animal> animales = new ArrayList<>();

    public static void main(String[] args) {
        int opcion;
        do {
            System.out.println("\n--- Menú Principal ---");
            System.out.println("1. Zoo");
            System.out.println("2. Fase 2");
            System.out.println("3. Fase 3");
            System.out.println("4. Salir");
            System.out.print("Seleccione una opción: ");

            try {
                opcion = scanner.nextInt();
                scanner.nextLine(); // Consumir el salto de línea
                switch (opcion) {
                    case 1 -> menuZoo();
                    case 4 -> System.out.println("Gracias por visitar el zoológico. ¡Hasta pronto!");
                    default -> System.out.println("Opción no válida.");
                }
            } catch (InputMismatchException e) {
                System.out.println("Error: Debe ingresar un número válido.");
                scanner.nextLine(); // Limpiar buffer
                opcion = 0;
            }
        } while (opcion != 4);
    }

    private static void menuZoo() {
        int opcion;
        do {
            System.out.println("\n--- Menú del Zoológico ---");
            System.out.println("1. Agregar nuevo animal");
            System.out.println("2. Ver todos los animales");
            System.out.println("3. Exportar datos a CSV");
            System.out.println("4. Volver al menú principal");
            System.out.print("Seleccione una opción: ");

            try {
                opcion = scanner.nextInt();
                scanner.nextLine();
                switch (opcion) {
                    case 1 -> agregarAnimal();
                    case 2 -> listarAnimales();
                    case 3 -> exportarCSV();
                    case 4 -> System.out.println("Volviendo al menú principal...");
                    default -> System.out.println("Opción no válida.");
                }
            } catch (InputMismatchException e) {
                System.out.println("Error: Debe ingresar un número válido.");
                scanner.nextLine();
                opcion = 0;
            }
        } while (opcion != 4);
    }

    private static void agregarAnimal() {
        if (animales.size() >= 3) {
            System.out.println("Ya ha ingresado un mamífero, un ave y un reptil.");
            return;
        }

        System.out.print("Ingrese el nombre del animal: ");
        String nombre = scanner.nextLine();
        System.out.print("Ingrese la edad del animal: ");
        int edad 2
        = scanner.nextInt();
        System.out.print("Ingrese el consumo diario de alimento (en libras): ");
        double consumoDiario = scanner.nextDouble();
        scanner.ne3
                
        System.out.println("Seleccione el tipo de animal:");
        System.out.println("1. Mamífero");
        System.out.println("2. Ave");
        System.out.println("3. Reptil");
        int tipo = scanner.nextInt();
        scanner.nextLine();

        Animal animal = switch (tipo) {
            case 1 -> new Mamifero(nombre, edad, consumoDiario);
            case 2 -> new Ave(nombre, edad, consumoDiario);
            case 3 -> new Reptil(nombre, edad, consumoDiario);
            default -> null;
        };

        if (animal != null) {
            animales.add(animal);
            System.out.println("Animal agregado con éxito.");
        } else {
            System.out.println("Opción no válida.");
        }
    }

    private static void listarAnimales() {
        if (animales.isEmpty()) {
            System.out.println("No hay animales registrados.");
        } else {
            System.out.println("\n--- Animales en el Zoológico ---");
            for (Animal animal : animales) {
                animal.mostrarInfo();
            }
        }
    }

    private static void exportarCSV() {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("zoo_data.csv"))) {
            writer.write("Nombre,Edad,Dieta,Consumo Diario\n");
            for (Animal animal : animales) {
                writer.write(animal.toCSV() + "\n");
            }
            System.out.println("Datos exportados a zoo_data.csv correctamente.");
        } catch (IOException e) {
            System.out.println("Error al exportar el archivo CSV.");
        }
    }
}
