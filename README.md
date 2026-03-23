Lab 1 – Terraform VM i Google Cloud
Beskrivning

I det här projektet har jag använt Terraform för att skapa en virtuell maskin (VM) i Google Cloud. Tanken är att jobba med Infrastructure as Code, alltså att definiera infrastrukturen i kod istället för att klicka runt i konsolen. Jag har också lagt till en enkel CI-pipeline och några grundläggande säkerhetsinställningar.

Hur man kör projektet

Börja med att initiera Terraform:

terraform init

Kolla att allt ser rätt ut:

terraform validate

Se vad som kommer skapas:

terraform plan

Applicera ändringarna:

terraform apply

Skriv yes när du blir tillfrågad.

CI/CD Pipeline

Jag har satt upp en GitHub Actions pipeline som körs vid push och pull requests. Den gör:

Formatteringskontroll (terraform fmt)
Validering av Terraform-koden
Säkerhetsscanning med Trivy

Screenshot på pipeline som går igenom:

![Pipeline](./images/pipeline.png)
VM i GCP

När man kör terraform apply skapas en VM i Google Cloud.

Screenshot från GCP Console:

![VM](./images/vm.png)
Säkerhet

Jag har lagt till några enkla säkerhetsåtgärder i startup-scriptet:

UFW (brandvägg)

Blockerar all inkommande trafik som standard
Tillåter bara SSH

Fail2Ban

Skyddar mot brute force-attacker
Blockerar IP-adresser efter flera misslyckade login

Unattended Upgrades

Installerar säkerhetsuppdateringar automatiskt

Tanken är att få en grundläggande “secure by default”-setup utan att göra det för avancerat.

Backup

Jag har också lagt till en backup policy:

Dagliga snapshots
Sparas i 7 dagar

Det gör att man kan återställa om något går fel.

Övrigt
terraform.tfvars är inte med i Git (innehåller känslig info)

Inloggning till GCP görs via:

gcloud auth application-default login
