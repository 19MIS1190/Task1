# Task1
package com.example.demo;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.web.bind.annotation.*;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import java.util.List;

@SpringBootApplication
public class ServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ServerApplication.class, args);
    }
}

@Entity
class Server {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String language;
    private String framework;

    // Constructors, getters, and setters
}

interface ServerRepository extends JpaRepository<Server, Long> {
    List<Server> findByNameContaining(String name);
}

@RestController
@RequestMapping("/servers")
class ServerController {
    private final ServerRepository serverRepository;

    public ServerController(ServerRepository serverRepository) {
        this.serverRepository = serverRepository;
    }

    @GetMapping
    public List<Server> getAllServers() {
        return serverRepository.findAll();
    }

    @GetMapping("/{id}")
    public Server getServerById(@PathVariable Long id) {
        return serverRepository.findById(id).orElse(null);
    }

    @PostMapping
    public Server createServer(@RequestBody Server server) {
        return serverRepository.save(server);
    }

    @DeleteMapping("/{id}")
    public void deleteServer(@PathVariable Long id) {
        serverRepository.deleteById(id);
    }

    @GetMapping("/search")
    public List<Server> findServersByName(@RequestParam String name) {
        return serverRepository.findByNameContaining(name);
    }
}
