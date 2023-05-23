# My-proyect
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class User {
    private int id;
    private String user;
    private String pwd;
    private String names;

    public User(int id, String user, String pwd, String names) {
        this.id = id;
        this.user = user;
        this.pwd = pwd;
        this.names = names;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUser() {
        return user;
    }

    public void setUser(String user) {
        this.user = user;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    public String getNames() {
        return names;
    }

    public void setNames(String names) {
        this.names = names;
    }
}

class BD {
    private static List<User> users = new ArrayList<>();

    public static List<User> getUsers() {
        return users;
    }

    public static void setUsers(List<User> users) {
        BD.users = users;
    }
}

class Controladora {
    public static void procesarRequest(String tipo, String cabecera, String body) {
        switch (tipo) {
            case "Get":
                if (body == null || body.isEmpty()) {
                    System.out.println(getUsers());
                } else {
                    System.out.println(getUser(Integer.parseInt(body)));
                }
                break;
            case "Post":
                postUser(body);
                break;
            case "Put":
                putUser(body);
                break;
            case "Delete":
                deleteUser(Integer.parseInt(body));
                break;
            default:
                System.out.println("Tipo de solicitud no válido");
                break;
        }
    }

    private static List<User> getUsers() {
        return BD.getUsers();
    }

    private static User getUser(int id) {
        for (User user : BD.getUsers()) {
            if (user.getId() == id) {
                return user;
            }
        }
        return null;
    }

    private static void postUser(String body) {
        List<String> data = Arrays.asList(body.split(","));
        int id = BD.getUsers().size() + 1;
        User newUser = new User(id, data.get(0), data.get(1), data.get(2));
        BD.getUsers().add(newUser);
        System.out.println("Usuario creado con éxito");
    }

    private static void putUser(String body) {
        List<String> data = Arrays.asList(body.split(","));
        int id = Integer.parseInt(data.get(0));
        User userToUpdate = getUser(id);
        if (userToUpdate != null) {
            userToUpdate.setUser(data.get(1));
            userToUpdate.setPwd(data.get(2));
            userToUpdate.setNames(data.get(3));
            System.out.println("Usuario actualizado con éxito");
        } else {
            System.out.println("Usuario no encontrado");
        }
    }

    private static void deleteUser(int id) {
        User userToDelete = getUser(id);
        if (userToDelete != null) {
            BD.getUsers().remove(userToDelete);
            System.out.println("Usuario eliminado con éxito");
        } else {
            System.out.println("Usuario no encontrado");
        }
    }
}

public class Main {

  public static void main(String[] args) {

      // Crear usuarios de prueba
      List<User> users = new ArrayList<>(Arrays.asList(
        new User(1, "Melo64", "@n4_124%", "Camilo Molina"),
        new User(2, "Roca2020", "asd@_44", "Santiago Restrepo")
));
BD.setUsers(users);

      // Leer usuarios existentes
      Controladora.procesarRequest("Get", "", "");

      // Encontrar un usuario
      Controladora.procesarRequest("Get", "", "1");

      // Crear un usuario
      Controladora.procesarRequest("Post", "", "Pepito2022,p3p1t0%,Pepito Perez");

      // Editar la información de un usuario existente
      Controladora.procesarRequest("Put", "", "2,Juanito2022,j4n1t0%,Juanito Perez");

      // Eliminar un usuario
      Controladora.procesarRequest("Delete", "", "2");

      // Leer usuarios existentes después de eliminar uno
      Controladora.procesarRequest("Get", "", "");
  }
}
