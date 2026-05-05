# Spring Boot E-Commerce - Code Analysis & Refactoring

Welcome to the project! This is a sample Spring Boot e-commerce app that we're using as a playground for static code analysis. The main goal here isn't just to build an app, but to figure out what's wrong with it under the hood—specifically looking at technical debt and where it breaks SOLID design principles.

## Getting Things Running

**1. Grab the code**
First things first, clone the repo and take a look around to get a feel for how the project is laid out.
![Git Clone](images/git-clone.png)

**2. Set up your environment**
You'll need Java, Maven, and MySQL to get this running locally. Once your database is up, just import `basedata.sql` to seed it with some initial data so you're not staring at an empty app.
![GitHub Repository](images/Github%20Repository.png)

**3. Fire up SonarQube**
We're using SonarQube to do the heavy lifting for our code inspection. Go ahead and install the package to get your analysis environment ready.
![SonarQube Installation](images/1-%20Installation%20pakcage%20sonarqube.png)

---

## The Analysis Workflow

We've set up automated quality gates to keep track of our technical debt and catch code smells before they become a nightmare. 

**1. Running the scan**
Kick off the SonarQube scanner against the codebase to see what we're dealing with.
![Running SonarQube](images/2-%20Running%20Sonarqube.png)

**2. Checking the damage**
Hop into the dashboard. This is where you'll see the raw truth about vulnerabilities, bugs, and messy code.
![SonarQube Results](images/3-%20Sonarqube%20Results.png)

**3. Taking it to the cloud**
We push our results to SonarQube Cloud so the whole team can see the reports in one centralized place. 
![Cloud Integration](images/4-%20Sonarqube%20Cloud%20Integration.png)
![Payload Download](images/5-%20Downloading%20the%20payload.png)

---

## The SOLID Damage Report

Based on the SonarQube audit (you can find the raw data in `code_smells.csv`), here's a breakdown of exactly where the code went off the rails and violated SOLID principles:

| Principle | Rules Triggered | What went wrong in the code? |
| :--- | :--- | :--- |
| **SRP** (Single Responsibility) | S120, S101, S117, S7930 | **Naming Chaos & God Classes**: The DAOs, Services, and Controllers are a bit of a mess structurally. We're also seeing mixed UI responsibilities, like duplicate DOM IDs scattered across the JSPs. |
| **OCP** (Open/Closed) | S1192, S7926 | **Hardcoded Everywhere**: Auth routes (like `"/login"`) and UI viewports are hardcoded directly into the files. If we want to change behavior, we're forced to rewrite existing code instead of just extending it. |
| **LSP** (Liskov Substitution) | S6819 | **Broken Semantics**: We're improperly using ARIA roles instead of standard HTML5 elements (e.g., throwing in `role="dialog"` instead of just using a `<dialog>` tag). It breaks how browsers expect the document to behave. |
| **ISP** (Interface Segregation) | S1186, S5254, ImgAlt | **Dead Code & Forced Implementations**: We found empty methods hanging around in `Cart.java` and some test files. On top of that, there are missing mandatory HTML attributes (`alt`, `lang`), meaning classes and views are being forced to implement things they clearly don't use. |
| **DIP** (Dependency Inversion) | S116 | **Leaky Abstractions**: `HibernateConfiguration.java` has public fields that directly expose internal database credentials to higher-level modules. Not great for security or modularity. |

