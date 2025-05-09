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

// Singleton Configuration
class ConfiguracionEvento {
    private static ConfiguracionEvento instance;
    private String nombreEvento;
    private int duracionHoras;
    private String idioma;

    private ConfiguracionEvento() {
        
        this.nombreEvento = "Conferencia Tech 2025";
        this.duracionHoras = 8;
        this.idioma = "Español";
    }

    public static synchronized ConfiguracionEvento getInstance() {
        if (instance == null) {
            instance = new ConfiguracionEvento();
        }
        return instance;
    }

    // Getters y Setters
    public String getNombreEvento() { return nombreEvento; }
    public void setNombreEvento(String nombre) { this.nombreEvento = nombre; }
    public int getDuracionHoras() { return duracionHoras; }
    public void setDuracionHoras(int horas) { this.duracionHoras = horas; }
    public String getIdioma() { return idioma; }
    public void setIdioma(String idioma) { this.idioma = idioma; }
}


class CredencialEvento implements Cloneable {
    private String nombre;
    private String cargo;
    private String rut;
    
    public CredencialEvento() {
        
        this.nombre = "[NOMBRE]";
        this.cargo = "[CARGO]";
        this.rut = "[RUT]";
    }

    @Override
    public CredencialEvento clone() {
        try {
            return (CredencialEvento) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }

    
    public void setNombre(String nombre) { this.nombre = nombre; }
    public void setCargo(String cargo) { this.cargo = cargo; }
    public void setRut(String rut) { this.rut = rut; }

    @Override
    public String toString() {
        return "Credencial:\n" +
               "Nombre: " + nombre + "\n" +
               "Cargo: " + cargo + "\n" +
               "RUT: " + rut + "\n" +
               "Evento: " + ConfiguracionEvento.getInstance().getNombreEvento() + "\n" +
               "Duración: " + ConfiguracionEvento.getInstance().getDuracionHoras() + " horas";
    }
}

public class MainSystem {
    private static List<CredencialEvento> credenciales = new ArrayList<>();
    private static CredencialEvento plantilla = new CredencialEvento();
    private static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        ConfiguracionEvento config = ConfiguracionEvento.getInstance();
        
        while (true) {
            System.out.println("\n--- Menú Principal ---");
            System.out.println("1. Crear nueva credencial");
            System.out.println("2. Ver todas las credenciales");
            System.out.println("3. Salir");
            System.out.print("Selección: ");

            int opcion = scanner.nextInt();
            scanner.nextLine(); // Clear buffer

            switch (opcion) {
                case 1:
                    crearCredencial();
                    break;
                case 2:
                    listarCredenciales();
                    break;
                case 3:
                    System.exit(0);
                default:
                    System.out.println("Opción inválida");
            }
        }
    }

    private static void crearCredencial() {
        CredencialEvento nueva = plantilla.clone();
        
        System.out.print("Nombre del asistente: ");
        nueva.setNombre(scanner.nextLine());
        
        System.out.print("Cargo: ");
        nueva.setCargo(scanner.nextLine());
        
        System.out.print("RUT: ");
        nueva.setRut(scanner.nextLine());
        
        credenciales.add(nueva);
        System.out.println("Credencial creada exitosamente!");
    }

    private static void listarCredenciales() {
        System.out.println("\n--- Credenciales Registradas ---");
        for (CredencialEvento c : credenciales) {
            System.out.println(c);
            System.out.println("----------------------");
        }
    }
}
```
---
## Implementacion de Singelton

```java
class ConfiguracionEvento {
    private static ConfiguracionEvento instance;
    private String nombreEvento;
    private int duracionHoras;
    private String idioma;

    private ConfiguracionEvento() {
        
        this.nombreEvento = "Conferencia Tech 2025";
        this.duracionHoras = 8;
        this.idioma = "Español";
    }

    public static synchronized ConfiguracionEvento getInstance() {
        if (instance == null) {
            instance = new ConfiguracionEvento();
        }
        return instance;
    }
```
---
## Implementacion de Prototype

```java
class CredencialEvento implements Cloneable {
    private String nombre;
    private String cargo;
    private String rut;
    
    public CredencialEvento() {
        
        this.nombre = "[NOMBRE]";
        this.cargo = "[CARGO]";
        this.rut = "[RUT]";
    }

    @Override
    public CredencialEvento clone() {
        try {
            return (CredencialEvento) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
```
---
## 🖥️ Menú por consola
---
![Image](https://github.com/user-attachments/assets/8f1eded3-8638-4c0d-9aeb-e2083e598cae)
---
Diagrama UML
---
![Image](https://github.com/user-attachments/assets/7cbc5c46-ed8c-4ca6-83cc-cbd543afe6ac)
---
## 📸 Captura del sistema funcionando
---
## 📸 Ingresando una crendial
---
![Image](https://github.com/user-attachments/assets/d08a5bf2-345d-4a48-907e-e8600c9388e3)
---
## 📸 Ingresando y mostrando mas de una credencial
---
![Image](https://github.com/user-attachments/assets/1012170e-b696-4347-91ba-5f745ca33dac)
---
## 📸 Opcion de salir del programa
---
![Image](https://github.com/user-attachments/assets/ef6f4215-2fb8-4c3a-b981-bd3782013b68)
---
