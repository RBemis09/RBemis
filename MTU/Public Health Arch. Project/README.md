
# Public Health Disease Surveillance Architecture Development Project

This project presents an end-to-end system for developing a disease surveillance architecture using synthetic health data, FHIR-based data exchange, and secure EMR hosting via virtual machines. The system is built to simulate real-world hospital data flow using modern healthcare IT standards and open-source tools.

---

## 🧩 Project Overview

The **goal** of this project is to simulate a regional public health infrastructure capable of receiving, storing, securing, and analyzing syndromic surveillance data. The architecture includes:

- Synthetic COVID-19 patient data generation with **Synthea**
- Data integration into **OpenEMR**
- Data exchange with **HAPI FHIR server**
- REST API interaction and query with **Postman**
- Virtualized hosting in MTU’s computing cluster

---

## 🏗️ System Architecture

- **Synthea** generates synthetic patient FHIR data by geographic region.
- **OpenEMR** is installed on virtual machines to simulate healthcare provider EMR systems.
- A central **HAPI FHIR Server** aggregates FHIR data from regional nodes.
- **Postman** and Python REST clients interact with the HAPI server for CRUD operations and surveillance queries.
- **Ubuntu Server VMs** host each hospital's stack, secured with proper firewall and Apache configurations.

---

## 🧪 Modules

### 1. Synthetic Data Generation (Synthea)

- Generate synthetic COVID-19 patient data for:
  - Aspirus Hospital (Calumet, MI)
  - Portage Health (Houghton, MI)
  - BCMH (Baraga, MI)
  - MGH (Marquette, MI)
- Example command:
  ```bash
  java -jar synthea-with-dependencies.jar -p 20 Michigan "Calumet" --config covid19
  ```

### 2. EMR Installation and Security (OpenEMR)

- Followed manual installation for version 6.0.0 of OpenEMR on Ubuntu.
- Set up LAMP stack, secured MySQL, and configured Apache.
- Applied security best practices:
  - Enabled UFW firewall
  - Set secure Apache headers
  - Installed automatic security updates
- Backups and password hardening suggested for production.

### 3. HAPI FHIR Server Deployment

- Deployed using Docker with custom `application.yaml`
- Bound to port 8090 for local RESTful access
- Key commands:
  ```bash
  sudo docker pull hapiproject/hapi:latest
  sudo docker run --name hapifhir -p 8090:8080 -itd -v ~/fhirConfig:/configs \
    -e "-spring.config.location=file:///configs/application.yaml" hapiproject/hapi:latest
  ```

### 4. FHIR REST API Interaction (Postman)

- Tested resource creation (e.g., `Practitioner`) via Swagger UI and Postman
- Used GET/POST/PUT/DELETE methods on endpoints like:
  ```
  http://localhost:8090/fhir/Practitioner
  ```
- Implemented conditional search:
  ```
  /Practitioner?_family=William&_lastUpdated=gt2011-01-01
  ```

---

## 💻 Virtual Machine Deployment

VMs deployed on the **MTU College of Computing Cluster** using provided templates:

- VM Names:
  - `aspirus`
  - `portage`
  - `bcmh`
  - `mgh`
  - `uphie`
- Each VM configured with static IPs, user permissions, and hostname labels
- OpenEMR and HAPI installed per node with security hardening
- Optional: VMware Tools for better VM responsiveness

---

## 🛠️ Tools & Technologies

| Tool       | Purpose                                    |
|------------|--------------------------------------------|
| Synthea    | Generate synthetic healthcare data         |
| OpenEMR    | EMR system for patient record management   |
| HAPI FHIR  | Central FHIR data aggregation              |
| Postman    | API testing and resource validation        |
| Docker     | Containerized deployment of FHIR server    |
| Ubuntu     | OS for VM infrastructure                   |

---

## 📂 Directory Structure

```
├── synthea/                 # Generated synthetic data
├── openemr/                 # Installed OpenEMR files
├── hapi-fhir-config/        # Custom Docker YAML configuration
├── postman-collections/     # Sample API requests for Postman
├── vm-setup/                # VM config, IPs, networking info
└── docs/                    # Guides, screenshots, documentation
```

---

## ✅ Setup Checklist

| Task                                     | Status |
|------------------------------------------|--------|
| Generate Synthea COVID data              | ✅     |
| Install & Secure OpenEMR                 | ✅     |
| Deploy Docker-based HAPI FHIR Server     | ✅     |
| Postman CRUD operations tested           | ✅     |
| Secure each VM with firewall and headers | ✅     |
| Connect EMRs to HAPI server              | ✅     |

---

## 🔒 Security Considerations

- UFW configured to limit exposure
- Apache hardened with response headers
- Strong passwords enforced
- Optional: SSL configuration for EMR portals

---

## 📚 References

- [HAPI FHIR Documentation](https://hapifhir.io)
- [OpenEMR Official Site](https://www.open-emr.org/)
- [Synthea GitHub](https://github.com/synthetichealth/synthea)
- [Postman API Tool](https://www.postman.com/)

