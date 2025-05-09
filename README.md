# 🪪 Generador de Credenciales de Evento

### Estudiante(s):  
- Benjamin Vivero – Patrones de Diseño (Sección 2)

---

## 🎯 Objetivo del Proyecto

Este sistema permite emitir credenciales personalizadas para un evento, a partir de una plantilla clonable. Se aplican los patrones de diseño **Prototype** (para clonar credenciales) y **Singleton** (para configuración global del evento).
---
```java
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

class ConfiguracionEvento {
    private static ConfiguracionEvento instancia;
    private String nombreEvento;
    private int duracionHoras;
    private String idioma;

    private ConfiguracionEvento() {
        // Valores predeterminados
        this.nombreEvento = "Conferencia Tech 2025";
        this.duracionHoras = 8;
        this.idioma = "Español";
    }

    public static synchronized ConfiguracionEvento obtenerInstancia() {
        if (instancia == null) {
            instancia = new ConfiguracionEvento();
        }
        return instancia;
    }

    public String obtenerNombreEvento() { return nombreEvento; }
    public void establecerNombreEvento(String nombre) { this.nombreEvento = nombre; }
    public int obtenerDuracionHoras() { return duracionHoras; }
    public void establecerDuracionHoras(int horas) { this.duracionHoras = horas; }
    public String obtenerIdioma() { return idioma; }
    public void establecerIdioma(String idioma) { this.idioma = idioma; }
}

class CredencialEvento implements Cloneable {
    private String nombre;
    private String cargo;
    private String rut;
    
    public CredencialEvento() {
        // Plantilla base
        this.nombre = "[NOMBRE]";
        this.cargo = "[CARGO]";
        this.rut = "[RUT]";
    }

    @Override
    public CredencialEvento clonar() {
        try {
            return (CredencialEvento) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new Error("Error en la clonación");
        }
    }

    public void establecerNombre(String nombre) { this.nombre = nombre; }
    public void establecerCargo(String cargo) { this.cargo = cargo; }
    public void establecerRut(String rut) { this.rut = rut; }

    @Override
    public String toString() {
        return "Credencial del Evento:\n" +
               "Nombre: " + nombre + "\n" +
               "Cargo: " + cargo + "\n" +
               "RUT: " + rut + "\n" +
               "Evento: " + ConfiguracionEvento.obtenerInstancia().obtenerNombreEvento() + "\n" +
               "Duración: " + ConfiguracionEvento.obtenerInstancia().obtenerDuracionHoras() + " horas";
    }
}

public class SistemaPrincipal {
    private static List<CredencialEvento> credenciales = new ArrayList<>();
    private static CredencialEvento plantilla = new CredencialEvento();
    private static Scanner lector = new Scanner(System.in);

    public static void main(String[] args) {
        ConfiguracionEvento configuracion = ConfiguracionEvento.obtenerInstancia();
        
        while (true) {
            mostrarMenuPrincipal();
            int opcion = leerOpcion();
            
            switch (opcion) {
                case 1:
                    crearCredencial();
                    break;
                case 2:
                    listarCredenciales();
                    break;
                case 3:
                    salirSistema();
                default:
                    System.out.println("Opción no válida");
            }
        }
    }

    private static void mostrarMenuPrincipal() {
        System.out.println("\n--- Menú de Control ---");
        System.out.println("1. Generar nueva credencial");
        System.out.println("2. Mostrar todas las credenciales");
        System.out.println("3. Salir del sistema");
        System.out.print("Ingrese su elección: ");
    }

    private static int leerOpcion() {
        int opcion = lector.nextInt();
        lector.nextLine(); // Limpiar buffer
        return opcion;
    }

    private static void crearCredencial() {
        CredencialEvento nueva = plantilla.clonar();
        
        System.out.print("Nombre completo del participante: ");
        nueva.establecerNombre(lector.nextLine());
        
        System.out.print("Rol en el evento: ");
        nueva.establecerCargo(lector.nextLine());
        
        System.out.print("Documento de identidad (RUT): ");
        nueva.establecerRut(lector.nextLine());
        
        credenciales.add(nueva);
        System.out.println("¡Credencial registrada con éxito!");
    }

    private static void listarCredenciales() {
        System.out.println("\n=== Registro de Credenciales ===");
        for (CredencialEvento credencial : credenciales) {
            System.out.println(credencial);
            System.out.println("----------------------------------");
        }
    }

    private static void salirSistema() {
        System.out.println("Saliendo del sistema...");
        lector.close();
        System.exit(0);
    }
}
```
---
## Implementacion de Singelton

```java
class ConfiguracionEvento {

    private static ConfiguracionEvento instancia;


    private ConfiguracionEvento() {
        this.nombreEvento = "Conferencia Tech 2025";
        this.duracionHoras = 8;
        this.idioma = "Español";
    }


    public static synchronized ConfiguracionEvento obtenerInstancia() {
        if (instancia == null) {
            instancia = new ConfiguracionEvento(); // ← Creación única
        }
        return instancia;
    }
```
---
## Implementacion de Prototype

```java
class CredencialEvento implements Cloneable {
    

    @Override
    public CredencialEvento clonar() {
        try {
            return (CredencialEvento) super.clone(); // ← Clonación nativa
        } catch (CloneNotSupportedException e) {
            throw new Error("Error en la clonación");
        }
    }


    public CredencialEvento() {
        this.nombre = "[NOMBRE]";
        this.cargo = "[CARGO]";
        this.rut = "[RUT]";
    }
```
---
Diagrama UML
---
![image](https://github.com/user-attachments/assets/a85c97f4-c4ce-451a-adb0-687d108c4a3a)
