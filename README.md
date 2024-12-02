
---

# Injection de Dépendances en Java

L'injection de dépendances (Dependency Injection, ou DI) est un **modèle de conception** qui permet de rendre une application plus modulaire et maintenable en séparant les responsabilités. Elle facilite également les tests en permettant d'injecter des dépendances simulées ou différentes configurations.

---

## Pourquoi utiliser l'injection de dépendances ?

Les principaux avantages de l'injection de dépendances sont les suivants :
- **Faible couplage** : Les classes dépendent des interfaces ou des abstractions, plutôt que des implémentations concrètes.
- **Testabilité améliorée** : Les dépendances peuvent être facilement remplacées par des objets factices (mocks) pour les tests.
- **Réutilisabilité** : Les composants peuvent être réutilisés dans différents contextes.
- **Maintenance simplifiée** : Les changements dans une dépendance n'exigent pas de modifier les classes qui l'utilisent.

---

## Les types d'injection de dépendances

En Java, l'injection de dépendances peut se faire de plusieurs manières :

### 1. **Injection par le constructeur**
Les dépendances sont passées comme arguments au constructeur d'une classe.

```java
public class Service {
    private final Repository repository;

    public Service(Repository repository) {
        this.repository = repository;
    }

    public void execute() {
        repository.save();
    }
}
```

### 2. **Injection par le setter**
Les dépendances sont fournies via des méthodes setter.

```java
public class Service {
    private Repository repository;

    public void setRepository(Repository repository) {
        this.repository = repository;
    }

    public void execute() {
        repository.save();
    }
}
```

### 3. **Injection par le champ**
Une dépendance est injectée directement dans un champ à l'aide de frameworks comme Spring (via des annotations).

```java
import org.springframework.beans.factory.annotation.Autowired;

public class Service {
    @Autowired
    private Repository repository;

    public void execute() {
        repository.save();
    }
}
```

---

## Références au cours

**S.O.L.I.D.** - L'injection de dépendances est un principe clé du Dependency Inversion Principle (DIP) de SOLID.
1. **Single Responsibility Principle (SRP)**
    - Les classes ont une seule responsabilité.
2. **Open/Closed Principle (OCP)**
    - Les classes sont ouvertes à l'extension mais fermées à la modification.
3. **Liskov Substitution Principle (LSP)**
    - Les objets d'une classe dérivée peuvent être utilisés comme objets de la classe de base.
4. **Interface Segregation Principle (ISP)**
    - Les interfaces sont spécifiques aux clients.
5. **Dependency Inversion Principle (DIP)**
    - Les classes dépendent d'abstractions, pas d'implémentations.

---

## Frameworks populaires pour l'injection de dépendances

1. **Spring Framework**
    - Spring est l'un des frameworks les plus populaires pour l'injection de dépendances en Java.
    - Il utilise des annotations comme `@Autowired`, `@Component`, `@Service`, et un conteneur IoC (Inversion of Control) pour gérer les dépendances.

2. **Google Guice**
    - Guice possède un système simple pour configurer et injecter des dépendances via des modules définis par l'utilisateur.

3. **Jakarta CDI** (Context and Dependency Injection)
    - CDI est une spécification standard pour l'injection de dépendances, incluse dans Jakarta EE.

---

## Exemple avec Spring

Voici un exemple d'injection de dépendances avec Spring :

```java
-Fichier1

import org.springframework.stereotype.Component;

@Component
public class RepositoryImpl implements Repository {
    @Override
    public void save() {
        System.out.println("Données enregistrées !");
    }
}

-Fichier2

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class Service {
    private final Repository repository;

    @Autowired
    public Service(Repository repository) {
        this.repository = repository;
    }

    public void execute() {
        repository.save();
    }
}

-Fichier3

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

@Configuration
@ComponentScan(basePackages = "com.example")
public class AppConfig {}

public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = 
                new AnnotationConfigApplicationContext(AppConfig.class);

        Service service = context.getBean(Service.class);
        service.execute();

        context.close();
    }
}
```

---

## Ressources supplémentaires

- [Documentation officielle de Spring DI](https://spring.io/projects/spring-framework)
- [Tutoriels Google Guice](https://github.com/google/guice/wiki)
- [Jakarta CDI](https://jakarta.ee/specifications/cdi/)

---
