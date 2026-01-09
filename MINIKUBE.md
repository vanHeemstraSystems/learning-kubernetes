## Minikube installeren met Docker Desktop - Volledige handleiding

### Stap 1: Docker Desktop installeren

1. **Download Docker Desktop**

- Ga naar <https://www.docker.com/products/docker-desktop/>
- Klik op “Download for Windows”

1. **Installeer Docker Desktop** (vereist admin rechten)

- Dubbelklik op het gedownloade bestand `Docker Desktop Installer.exe`
- Vink aan: “Use WSL 2 instead of Hyper-V” (aanbevolen)
- Klik op “OK” en wacht tot installatie voltooid is
- **Herstart je laptop** wanneer daarom gevraagd wordt

1. **Start Docker Desktop**

- Open Docker Desktop vanuit het Start menu
- Accepteer de servicevoorwaarden
- Je kunt de tutorial overslaan
- Wacht tot Docker volledig is opgestart (groen icoontje rechtsonder in systray)

1. **Verifieer Docker**
   Open PowerShell en typ:
   
   ```powershell
   docker --version
   docker run hello-world
   ```
   
   Je zou een welkomstbericht moeten zien.

### Stap 2: Minikube installeren

**Optie A: Via winget (makkelijkst)**

Open PowerShell als administrator:

```powershell
winget install Kubernetes.minikube
```

**Optie B: Handmatige download**

1. Ga naar <https://minikube.sigs.k8s.io/docs/start/>
1. Download de Windows installer (.exe)
1. Voer de installer uit (als admin)

### Stap 3: kubectl installeren

kubectl is de commandline tool voor Kubernetes. Minikube installeert deze vaak automatisch, maar je kunt het ook apart installeren:

```powershell
# Via winget
winget install Kubernetes.kubectl

# Of via minikube
minikube kubectl -- version
```

### Stap 4: Minikube cluster starten

1. **Sluit en heropen PowerShell** (om PATH te refreshen)
1. **Start minikube**
   
   ```powershell
   minikube start --driver=docker
   ```
   
   Dit doet het volgende:

- Download de Kubernetes image (~1GB, eerste keer)
- Start een Docker container met Kubernetes
- Configureert kubectl automatisch
- Duurt 3-5 minuten de eerste keer

1. **Verifieer de installatie**
   
   ```powershell
   # Check minikube status
   minikube status
   
   # Check Kubernetes nodes
   kubectl get nodes
   
   # Check alle system pods
   kubectl get pods -A
   ```

### Stap 5: Dashboard (optioneel)

Start het Kubernetes dashboard voor een visuele interface:

```powershell
minikube dashboard
```

Dit opent automatisch je browser met het dashboard.

### Handige minikube commando’s

```powershell
# Cluster stoppen (behoudt configuratie)
minikube stop

# Cluster starten (na stop)
minikube start

# Cluster verwijderen
minikube delete

# SSH in de minikube VM
minikube ssh

# IP adres van cluster
minikube ip

# Addons bekijken/activeren
minikube addons list
minikube addons enable metrics-server
```

### Configuratie aanpassen (optioneel)

Als je meer resources wilt toewijzen:

```powershell
# Verwijder eerst bestaande cluster
minikube delete

# Start met meer resources
minikube start --driver=docker --cpus=4 --memory=8192 --disk-size=40g
```

### Troubleshooting

**Als Docker niet start:**

- Controleer of virtualisatie is ingeschakeld in BIOS
- Check Windows Updates
- Herstart Docker Desktop service

**Als minikube niet start:**

```powershell
# Check Docker draait
docker ps

# Start met verbose output
minikube start --driver=docker -v=7

# Reset minikube config
minikube delete --all --purge
```

**Path problemen:**
Log uit en weer in, of herstart PowerShell na installatie.

### Test deployment

Test of alles werkt met een simpele nginx deployment:

```powershell
# Deploy nginx
kubectl create deployment nginx --image=nginx

# Expose als service
kubectl expose deployment nginx --type=NodePort --port=80

# Get service URL
minikube service nginx --url

# Open in browser
minikube service nginx
```

Veel succes! 
