# Terraform IaC – GCP VM Deployment 

## Vad projektet gör
Det här projektet använder Terraform för att automatiskt sätta upp en virtuell maskin (VM) i Google Cloud Platform (GCP). 
Infrastrukturen definieras som kod (IaC) och driftsätts via en GitHub Actions CI/CD-pipeline som automatiskt 
kör format-kontroll, säkerhetsskanning och validering innan deploy sker. VM:en är konfigurerad med dagliga 
backups och härdade säkerhetsinställningar för att följa DevSecOps-principer.

## Hur man kör

### Förkrav
- [Terraform](https://developer.hashicorp.com/terraform/downloads) installerat
- GCP Service Account med rätt behörigheter
- `gcloud` CLI installerat och autentiserat

### Steg

**1. Initiera Terraform**
```bash
terraform init
```

**2. Granska ändringar**
```bash
terraform plan \
  -var="project_id=chas-devsecops-2026" \
  -var="region=europe-north1" \
  -var="student_id=albin-mansson"
```

**3. Driftsätt infrastrukturen**
```bash
terraform apply \
  -var="project_id=chas-devsecops-2026" \
  -var="region=europe-north1" \
  -var="student_id=albin-mansson"
```


## Säkerhetsbeslut

### ufw (Uncomplicated Firewall)
ufw används för att begränsa nätverkstrafik till VM:en. Endast nödvändiga portar 
(t.ex. SSH på port 22) är öppna, vilket minskar attackytan avsevärt.

### fail2ban
fail2ban övervakar inloggningsförsök och blockerar automatiskt IP-adresser som 
gör för många misslyckade inloggningsförsök. Detta skyddar mot brute-force-attacker mot SSH.

### Trivy (IaC-skanning)
Trivy skannar Terraform-koden i CI-pipelinen efter kända säkerhetsproblem och 
felkonfigurationer innan infrastrukturen driftsätts. Endast CRITICAL och HIGH-sårbarheter 
stoppar pipelinen.

### Principen om minsta privilegium
Service accounten som används av GitHub Actions har endast de behörigheter som krävs 
för att skapa och hantera de specifika resurserna i projektet.
