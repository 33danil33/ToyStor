��#   T o y S t o r 
 import java.io.*;
import java.util.ArrayList;
import java.util.Random;
import java.util.Scanner;

class Toy {
    private String name;
    private String category;

    public Toy(String name, String category) {
        this.name = name;
        this.category = category;
    }

    public String getName() {
        return name;
    }

    public String getCategory() {
        return category;
    }

    @Override
    public String toString() {
        return name + " (" + category + ")";
    }
}

class ToyStore {
    private ArrayList<Toy> toys;
    private String storeName;

    public ToyStore(String storeName) {
        this.storeName = storeName;
        this.toys = new ArrayList<>();
    }

    public void addToy(Toy toy) {
        toys.add(toy);
    }

    public boolean removeToy(String toyName) {
        for (Toy toy : toys) {
            if (toy.getName().equalsIgnoreCase(toyName)) {
                toys.remove(toy);
                return true;
            }
        }
        return false;
    }

    public void listToys() {
        System.out.println("Игрушки в магазине " + storeName + ":");
        for (int i = 0; i < toys.size(); i++) {
            System.out.println((i + 1) + ". " + toys.get(i));
        }
    }

    public void saveToFile(String filename) {
        try (PrintWriter writer = new PrintWriter(new FileWriter(filename))) {
            for (Toy toy : toys) {
                writer.println(toy.getName() + "," + toy.getCategory());
            }
        } catch (IOException e) {
            System.err.println("Ошибка при записи в файл: " + e.getMessage());
        }
    }

    public void loadFromFile(String filename) {
        try (Scanner scanner = new Scanner(new File(filename))) {
            while (scanner.hasNextLine()) {
                String line = scanner.nextLine();
                String[] parts = line.split(",");
                if (parts.length == 2) {
                    Toy toy = new Toy(parts[0], parts[1]);
                    toys.add(toy);
                }
            }
        } catch (FileNotFoundException e) {
            System.err.println("Файл не найден: " + e.getMessage());
        }
    }

    public Toy drawRandomToy() {
        if (toys.isEmpty()) {
            System.out.println("Магазин пуст. Розыгрыш невозможен.");
            return null;
        }

        Random random = new Random();
        int randomIndex = random.nextInt(toys.size());
        return toys.remove(randomIndex);
    }
}

public class Main {
    public static void main(String[] args) {
        ToyStore store = new ToyStore("Детский магазин");
        store.loadFromFile("toys.txt");

        Scanner 3 = new Scanner(System.in);

        while (true) {
            System.out.println("\nМеню:");
            System.out.println("1. Добавить игрушку");
            System.out.println("2. Удалить игрушку");
            System.out.println("3. Список игрушек");
            System.out.println("4. Сохранить в файл");
            System.out.println("5. Розыгрыш игрушки");
            System.out.println("6. Выход");

            System.out.print("Выберите действие: ");
            int choice = scanner.nextInt();
            scanner.nextLine(); 

            switch (choice) {
                case 1:
                    System.out.print("Введите название игрушки: ");
                    String name = scanner.nextLine();
                    System.out.print("Введите категорию игрушки: ");
                    String category = scanner.nextLine();
                    Toy toy = new Toy(name, category);
                    store.addToy(toy);
                    System.out.println(toy.getName() + " добавлена в магазин.");
                    break;
                case 2:
                    System.out.print("Введите название игрушки для удаления: ");
                    String toyName = scanner.nextLine();
                    if (store.removeToy(toyName)) {
                        System.out.println(toyName + " удалена из магазина.");
                    } else {
                        System.out.println("Игрушка " + toyName + " не найдена в магазине.");
                    }
                    break;
                case 3:
                    store.listToys();
                    break;
                case 4:
                    store.saveToFile("toys.txt");
                    System.out.println("Игрушки сохранены в файл.");
                    break;
                case 5:
                    Toy winner = store.drawRandomToy();
                    if (winner != null) {
                        System.out.println("Поздравляем! Вы выиграли " + winner);
                    }
                    break;
                case 6:
                    System.out.println("Программа завершена.");
                    System.exit(0);
                default:
                    System.out.println("Неверный выбор. Пожалуйста, выберите действие из меню.");
            }
        }
    }
}
 
