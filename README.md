import java.util.*; // Import necessary utilities (ArrayList, Scanner, etc.)

// Class representing an Employee
class Employe {
    // Private attributes (Encapsulation)
    private int id;
    private String nom;
    private String poste;
    private double salaire;

    // Constructor to initialize employee details
    public Employe(int id, String nom, String poste, double salaire) {
        this.id = id;
        this.nom = nom;
        this.poste = poste;
        this.salaire = salaire;
    }

    // Getter methods to retrieve employee details
    public int getId() { return id; }
    public String getNom() { return nom; }
    public String getPoste() { return poste; }
    public double getSalaire() { return salaire; }

    // Setter methods to update employee details
    public void setNom(String nom) { this.nom = nom; }
    public void setPoste(String poste) { this.poste = poste; }
    public void setSalaire(double salaire) { this.salaire = salaire; }

    // Method to return employee details as a string
    @Override
    public String toString() {
        return "ID: " + id + ", Nom: " + nom + ", Poste: " + poste + ", Salaire: " + salaire;
    }
}

// Class to manage a list of employees
class GestionEmployes {
    private List<Employe> employes = new ArrayList<>(); // List to store employees

    // Method to add a new employee
    public void ajouterEmploye(Employe employe) {
        employes.add(employe);
    }

    // Method to modify an existing employee's details
    public boolean modifierEmploye(int id, String nom, String poste, double salaire) {
        for (Employe e : employes) {
            if (e.getId() == id) {
                e.setNom(nom);
                e.setPoste(poste);
                e.setSalaire(salaire);
                return true; // Return true if employee is found and updated
            }
        }
        return false; // Return false if employee is not found
    }

    // Method to remove an employee using their ID
    public boolean supprimerEmploye(int id) {
        return employes.removeIf(e -> e.getId() == id);
    }

    // Method to display all employees in a formatted table
    public void afficherEmployes() {
        if (employes.isEmpty()) {
            System.out.println("Aucun employé à afficher.");
            return;
        }

        // Print table header
        System.out.println("+----+-----------------+-----------------+------------+");
        System.out.println("| ID | Nom             | Poste           | Salaire    |");
        System.out.println("+----+-----------------+-----------------+------------+");

        // Print employee data
        for (Employe e : employes) {
            System.out.printf("| %-2d | %-15s | %-15s | %-10.2f |\n",
                    e.getId(), e.getNom(), e.getPoste(), e.getSalaire());
        }

        // Print table footer
        System.out.println("+----+-----------------+-----------------+------------+");
    }

    // Method to search for employees by name or job title
    public List<Employe> rechercherEmploye(String critere) {
        List<Employe> resultats = new ArrayList<>();
        for (Employe e : employes) {
            if (e.getNom().equalsIgnoreCase(critere) || e.getPoste().equalsIgnoreCase(critere)) {
                resultats.add(e);
            }
        }
        return resultats; // Return the list of matching employees
    }

    // Method to calculate the total payroll (sum of all salaries)
    public double calculerMasseSalariale() {
        double total = 0;
        for (Employe e : employes) {
            total += e.getSalaire();
        }
        return total; // Return the total salary sum
    }

    // Method to sort employees by salary (ascending or descending)
    public void trierEmployesParSalaire(boolean croissant) {
        employes.sort(Comparator.comparingDouble(Employe::getSalaire)); // Sort by salary
        if (!croissant) Collections.reverse(employes); // Reverse if descending
    }
}

// Main class to run the application
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in); // Scanner for user input
        GestionEmployes gestion = new GestionEmployes(); // Create employee manager

        // Infinite loop to show menu until the user exits
        while (true) {
            System.out.println("\nMenu :");
            System.out.println("1. Ajouter un employé");
            System.out.println("2. Modifier un employé");
            System.out.println("3. Supprimer un employé");
            System.out.println("4. Afficher tous les employés");
            System.out.println("5. Rechercher un employé");
            System.out.println("6. Calculer la masse salariale");
            System.out.println("7. Trier les employés par salaire");
            System.out.println("8. Quitter");
            System.out.print("Votre choix : ");
            int choix = scanner.nextInt(); // Get user's menu choice

            switch (choix) {
                case 1:
                    // Add a new employee
                    System.out.print("ID: ");
                    int id = scanner.nextInt();
                    scanner.nextLine(); // Consume newline character
                    System.out.print("Nom: ");
                    String nom = scanner.nextLine();
                    System.out.print("Poste: ");
                    String poste = scanner.nextLine();
                    System.out.print("Salaire: ");
                    double salaire = scanner.nextDouble();
                    gestion.ajouterEmploye(new Employe(id, nom, poste, salaire));
                    break;

                case 2:
                    // Modify an existing employee
                    System.out.print("ID de l'employé à modifier: ");
                    int idModif = scanner.nextInt();
                    scanner.nextLine(); // Consume newline
                    System.out.print("Nouveau nom: ");
                    String newNom = scanner.nextLine();
                    System.out.print("Nouveau poste: ");
                    String newPoste = scanner.nextLine();
                    System.out.print("Nouveau salaire: ");
                    double newSalaire = scanner.nextDouble();
                    if (!gestion.modifierEmploye(idModif, newNom, newPoste, newSalaire))
                        System.out.println("Employé introuvable.");
                    break;

                case 3:
                    // Delete an employee
                    System.out.print("ID de l'employé à supprimer: ");
                    int idSuppr = scanner.nextInt();
                    if (!gestion.supprimerEmploye(idSuppr))
                        System.out.println("Employé introuvable.");
                    break;

                case 4:
                    // Display all employees
                    gestion.afficherEmployes();
                    break;

                case 5:
                    // Search for an employee
                    System.out.print("Nom ou poste à rechercher: ");
                    scanner.nextLine(); // Consume newline
                    String critere = scanner.nextLine();
                    List<Employe> resultats = gestion.rechercherEmploye(critere);
                    if (resultats.isEmpty()) System.out.println("Aucun employé trouvé.");
                    else resultats.forEach(System.out::println);
                    break;

                case 6:
                    // Calculate total salary
                    System.out.println("Masse salariale totale: " + gestion.calculerMasseSalariale());
                    break;

                case 7:
                    // Sort employees by salary
                    System.out.print("Trier par ordre croissant ? (true/false): ");
                    boolean croissant = scanner.nextBoolean();
                    gestion.trierEmployesParSalaire(croissant);
                    gestion.afficherEmployes();
                    break;

                case 8:
                    // Exit the program
                    System.out.println("Fin du programme.");
                    scanner.close(); // Close scanner
                    return; // Exit loop

                default:
                    System.out.println("Choix invalide. Veuillez entrer un numéro valide.");
            }
        }
    }
}

