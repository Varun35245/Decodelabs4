# Decodelabs4
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}

}
 package com.example.demo.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import com.example.demo.model.User;
import com.example.demo.service.UserService;

@RestController
@CrossOrigin
public class UserController {

    @Autowired
    private UserService service;

    @GetMapping("/")
    public String home() {
        return "Welcome to project";
    }

    @GetMapping("/api/users")
    public List<User> getUsers() {
        return service.getAllUsers();
    }

    @GetMapping("/api/users/{id}")
    public User getUserById(@PathVariable int id) {
        return service.getUserById(id);
    }

    @PostMapping("/api/users")
    public String addUser(@RequestBody User user) {
        service.saveUser(user);
        return "User Added Successfully";
    }

    @PutMapping("/users/{id}")
    public User updateUser(@PathVariable int id, @RequestBody User user) {
        return service.updateUser(id, user);
    }

    @DeleteMapping("/api/users/{id}")
    public String deleteUser(@PathVariable int id) {
        service.deleteUser(id);
        return "User Deleted Successfully";
    }
}
package com.example.demo.model;

import jakarta.persistence.*;

@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private String name;
    private String email;

    // Constructors
    public User() {
    }

    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }

    // Getter & Setter
    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
package com.example.demo.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import com.example.demo.model.User;

public interface UserRepository extends JpaRepository<User, Integer> {
}
 package com.example.demo.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.example.demo.model.User;
import com.example.demo.repository.UserRepository;

@Service
public class UserService {

    @Autowired
    private UserRepository repo;

    public List<User> getAllUsers() {
        return repo.findAll();
    }

    public void saveUser(User user) {
        repo.save(user);
    }

    public User getUserById(int id) {
        return repo.findById(id).orElse(null);
    }

    public void deleteUser(int id) {
        repo.deleteById(id);
    }

    public User updateUser(int id, User updatedUser) {
        User existingUser = repo.findById(id).orElse(null);

        if (existingUser != null) {
            existingUser.setName(updatedUser.getName());
            existingUser.setEmail(updatedUser.getEmail());
            return repo.save(existingUser);
        }
        return null;
    }
}
