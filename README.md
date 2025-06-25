````markdown
#  Simple WebApp CI mit Jenkins

##  Projektbeschreibung

In diesem Projekt wird eine einfache CI-Pipeline mit Jenkins realisiert. Ziel ist es, den Quellcode einer React-Vite-Webanwendung über Jenkins zu clonen und eine (symbolische) Build-Stufe durchzuführen. Aufgrund von Einschränkungen im Jenkins-Docker-Setup wurde der eigentliche Build nicht im Jenkins-Container, sondern lokal durchgeführt. Jenkins dient hier der CI-Automatisierung, Codeprüfung und Integration mit dem Git-Repository.

---

##  Verwendete Technologien

- Node.js + npm
- Vite + React
- Jenkins (Docker, lokal)
- Git + GitHub

---

##  Setup-Anleitung

###  Voraussetzungen

- [x] Docker installiert
- [x] Git installiert
- [x] Node.js + npm lokal installiert

###  Webanwendung erstellen

```bash
npm create vite@latest simple-webapp -- --template react
cd simple-webapp
npm install
npm run build
````

###  Git-Repo einrichten

```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/jonascicek/jenkins-ha.git
git push -u origin main
```

---

##  Jenkins Setup

###  Jenkins starten (Docker)

```bash
docker run --name my-simple-jenkins -p 8080:8080 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
```

###  Jenkins UI ([http://localhost:8080](http://localhost:8080))

1. Unlock Jenkins mit Passwort aus `docker logs my-simple-jenkins`
2. Plugins installieren (empfohlen)
3. Admin-Benutzer anlegen
4. Neues „Pipeline“-Projekt anlegen

###  Job-Konfiguration

* **Definition**: `Pipeline script from SCM`
* **SCM**: Git
* **Repo-URL**: `https://github.com/jonascicek/jenkins-ha.git`
* **Branch**: `*/main`
* **Script Path**: `Jenkinsfile`

---

##  Jenkinsfile

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build (übersprungen)') {
            steps {
                echo '⚠️ Build wird in dieser Jenkins-Umgebung nicht ausgeführt.'
                echo '📦 Du kannst npm run build lokal ausführen.'
            }
        }
        stage('Fertig') {
            steps {
                echo '✅ Jenkins hat den Code erfolgreich aus dem Repository geladen.'
            }
        }
    }
}
```

---

##  Screenshots für die Abgabe

* [x] Jenkins läuft (Dashboard)
* [x] Pipeline-Konfiguration mit Git-Repo
* [x] Jenkinsfile-Inhalt
* [x] Erfolgreicher Pipeline-Lauf + Konsolenausgabe

---

## 💬 Reflexion: Fragen & Antworten

### 1. Welche Schritte waren notwendig, um Jenkins lokal mit Docker zum Laufen zu bringen?

* Docker Container mit `jenkins/jenkins:lts` gestartet
* Jenkins im Browser geöffnet (`localhost:8080`)
* Initialpasswort aus Docker-Log geholt
* Plugins installiert
* Admin-Benutzer erstellt

---

### 2. Was ist der Zweck der Datei Jenkinsfile, und wo muss sie im Verhältnis zu deinem Anwendungscode liegen?

* Sie beschreibt deklarativ, wie die CI-Pipeline aussehen soll (Stages wie „Checkout“ oder „Build“)
* Sie liegt im **Wurzelverzeichnis** des Git-Repositories – also im gleichen Ordner wie z. B. `package.json`

---

### 3. Beschreibe die zwei Hauptstages, die du in deiner Pipeline definiert hast, und was der Hauptzweck der Steps in jeder Stage ist.

* **Checkout**: Holt den aktuellen Code-Stand aus dem Git-Repository mit `checkout scm`
* **Build (übersprungen)**: Gibt einen Hinweis aus, dass der Build manuell oder lokal ausgeführt wird

---

### 4. Wie hast du in Jenkins konfiguriert, von welchem Git-Repository und welchem Branch der Code für die Pipeline geholt werden soll?

* Unter „Pipeline“ → „Pipeline-Skript aus SCM“ gewählt
* Git als SCM ausgewählt
* Repository-URL (`https://github.com/jonascicek/jenkins-ha.git`) angegeben
* Branch: `*/main`
* Script Path: `Jenkinsfile`

---

### 5. Was ist der Unterschied zwischen dem `checkout scm`-Step in deiner Pipeline und dem `git clone`-Befehl, den du manuell im Terminal ausführen würdest?

* `git clone` ist ein manueller Befehl in der Kommandozeile
* `checkout scm` ist ein Jenkins-interner Step, der automatisch basierend auf der Job-Konfiguration das Repo klont und den richtigen Branch auswählt – ohne dass man im Skript die URL erneut angeben muss

---
