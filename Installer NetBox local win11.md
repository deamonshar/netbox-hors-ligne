# Installer NetBox sur un PC Windows 11 sans Internet

Nov 24, 2026 · @Shara

## Avant de commencer

Ce tuto installe NetBox sur un PC Windows 11 sans Internet. Le PC connecté n'a besoin que d'un navigateur : il ne fera que télécharger un fichier. Comptez environ 1 heure, attente comprise.

**NetBox**, c'est un site web qui tourne sur votre PC et qui sert à documenter un réseau : équipements, baies, adresses IP, câbles. On l'utilise depuis un navigateur (Edge, Chrome…).

**Le principe :** NetBox a besoin de logiciels (Docker) et de fichiers lourds (les « images ») qu'un navigateur ne sait pas télécharger directement. On demande donc à **GitHub**, un service en ligne gratuit, de tout préparer à notre place dans un seul fichier ZIP. Le PC connecté n'a plus qu'à le télécharger.

&#91;embedded content: vue d'ensemble · 4 étapes\]

Sur le PC hors ligne, un script fait toute l'installation : vous double-cliquez, vous redémarrez une fois, vous choisissez un mot de passe.

**Les mots à connaître :**

- **GitHub** : un site (github.com) où l'on peut faire travailler des ordinateurs en ligne gratuitement.
- **Docker** : le logiciel qui fait tourner NetBox « en boîte » sur le PC hors ligne.
- **Image** : une application en boîte, prête à être lancée.
- **Terminal** : la fenêtre où l'on tape des commandes (clic droit sur **Démarrer** → **Terminal**). Vous n'en aurez besoin que pour l'entretien, pas pour l'installation.

## Ce qu'il vous faut

| Élément | Détail |
| --- | --- |
| PC connecté | N'importe quel PC avec un navigateur (Edge, Chrome, Firefox). Aucune installation, aucun droit administrateur. |
| Compte GitHub | Gratuit, créé en 2 minutes sur github.com avec une adresse e-mail. |
| Clé USB | 8 Go minimum, formatée en **exFAT** ou NTFS (le paquet pèse 1,5 à 2 Go). |
| PC hors ligne | Windows 11 (Famille, Pro ou Entreprise), processeur Intel ou AMD, 8 Go de RAM, 20 Go d'espace libre. |
| Droits sur le PC hors ligne | Pouvoir répondre **Oui** quand Windows demande une autorisation administrateur. |

## Partie A — Sur le PC connecté (navigateur seulement)

À la fin de cette partie, vous aurez un seul fichier **`netbox-hors-ligne.zip`** sur votre clé USB. Les étapes A1 à A3 ne se font qu'une fois ; pour les fois suivantes, A4 et A5 suffisent.

### A1. Créer un compte GitHub (une seule fois)

1. Allez sur **github.com** et cliquez **Sign up**.
2. Entrez votre e-mail, un mot de passe et un nom d'utilisateur, puis suivez les étapes.
3. GitHub envoie un code par e-mail : recopiez-le pour valider le compte.

### A2. Créer un dépôt (une seule fois)

Un « dépôt » est un dossier en ligne sur GitHub.

1. En haut à droite, cliquez sur **+** → **New repository**.
2. **Repository name** : `netbox-hors-ligne`.
3. Choisissez **Public**. Le dépôt ne contient aucune donnée personnelle, et les dépôts publics peuvent utiliser GitHub Actions gratuitement.
4. Cochez **Add a README file**, puis cliquez **Create repository**.

### A3. Ajouter le fichier de préparation (une seule fois)

1. Dans votre dépôt, cliquez **Add file** → **Create new file**.
2. Dans la case du nom, tapez exactement : `.github/workflows/netbox-hors-ligne.yml` (chaque `/` crée un dossier, c'est normal).
3. Allez à l'**Annexe** en bas de ce tuto, cliquez sur le bouton de copie du grand bloc de texte, et collez-le dans la grande zone blanche de GitHub (Ctrl+V).
4. Cliquez **Commit changes…** puis encore **Commit changes**.

### A4. Lancer la préparation du paquet

1. Cliquez sur l'onglet **Actions** (en haut du dépôt).
2. Dans la colonne de gauche, cliquez **Preparer NetBox hors ligne**.
3. À droite, cliquez **Run workflow**, puis sur le bouton vert **Run workflow**.
4. Actualisez la page (F5) : une ligne apparaît avec un rond jaune qui tourne. GitHub travaille.
5. Attendez **10 à 15 minutes**, jusqu'à ce que le rond devienne une **coche verte**.

&#91;embedded content: écran simplifié · onglet Actions de GitHub\]

### A5. Télécharger le paquet

1. Cliquez sur la ligne avec la coche verte.
2. Descendez jusqu'à la section **Artifacts**.
3. Cliquez sur **netbox-hors-ligne** : le téléchargement de `netbox-hors-ligne.zip` démarre (1,5 à 2 Go).

&#91;embedded content: écran simplifié · page d'une exécution terminée\]

Le paquet reste disponible **7 jours** sur GitHub. Passé ce délai, refaites A4 : vous obtiendrez en plus la dernière version de NetBox.

### A6. Copier sur la clé USB

Copiez `netbox-hors-ligne.zip` sur la clé, puis retirez-la proprement (icône USB en bas à droite → **Éjecter**). **N'ouvrez pas** le ZIP sur ce PC : ce n'est pas nécessaire.

## Partie B — Sur le PC sans Internet

Ici, vous ne tapez aucune commande : un script fait l'installation. Votre rôle se limite à vérifier la virtualisation, double-cliquer, redémarrer et choisir un mot de passe.

### B1. Vérifier que la virtualisation est activée

1. Appuyez sur **Ctrl + Maj + Échap** pour ouvrir le **Gestionnaire des tâches**.
2. Cliquez sur **Performance** (à gauche), puis sur **Processeur**.
3. En bas à droite, regardez la ligne **Virtualisation**.

&#91;embedded content: écran simplifié · Gestionnaire des tâches, onglet Performance\]

Si elle indique **Activé**, passez à B2. Si elle indique **Désactivé**, activez-la dans le BIOS. C'est la seule chose que le script ne peut pas faire :

1. Redémarrez le PC et appuyez plusieurs fois sur la touche du BIOS dès l'allumage : souvent **F2**, **Suppr**, **F10** ou **Échap**.
2. Cherchez **Intel Virtualization Technology**, **VT-x**, **SVM Mode** (processeurs AMD) ou **Virtualization**, souvent dans **Advanced** ou **CPU Configuration**.
3. Passez-la sur **Enabled**, puis enregistrez et quittez (souvent **F10** → **Yes**).

### B2. Débloquer et extraire le paquet

1. Copiez `netbox-hors-ligne.zip` de la clé vers **`C:\`**.
2. Clic droit sur le ZIP → **Propriétés**. Si une case **Débloquer** apparaît en bas, cochez-la, puis cliquez **OK**. Sans ça, Windows bloquera le script.
3. Clic droit sur le ZIP → **Extraire tout…** → dans la case de destination, tapez `C:\NetBoxSetup` → **Extraire**.
4. Ouvrez `C:\NetBoxSetup`. Vous devez y voir ces 5 éléments :

- [ ] `installer-netbox.bat`
- [ ] `installer-netbox.ps1`
- [ ] `Docker Desktop Installer.exe`
- [ ] un fichier `wsl…x64.msi`
- [ ] le dossier `netbox-docker-release`

&#91;embedded content: ce que fait installer-netbox.bat · 8 étapes, 1 redémarrage\]

Si le script s'arrête sur une **ERREUR** en rouge, corrigez ce qu'il indique puis relancez-le : il saute les étapes déjà faites.

### B3. Lancer l'installation

1. Dans `C:\NetBoxSetup`, double-cliquez sur **`installer-netbox.bat`**.
2. Windows demande l'autorisation : cliquez **Oui**.
3. Si une fenêtre bleue **« Windows a protégé votre ordinateur »** apparaît : cliquez **Informations complémentaires** → **Exécuter quand même**.
4. Une fenêtre bleue (PowerShell) affiche chaque étape avec des lignes **OK** en vert. Laissez-la travailler 5 à 10 minutes, **sans la fermer**.

### B4. Redémarrer

1. Quand la fenêtre affiche **Redemarrage necessaire**, appuyez sur **Entrée** : le PC redémarre.
2. Reconnectez-vous avec **le même compte Windows**. La fenêtre bleue se rouvre toute seule et reprend là où elle s'était arrêtée.
3. Docker Desktop s'ouvre aussi. S'il propose de se connecter à un compte, cliquez **Skip** (passer). Ne le fermez pas.
4. Le script charge NetBox et le démarre : comptez **5 à 15 minutes**. La ligne `etat de netbox : starting` s'affiche plusieurs fois, c'est normal.

### B5. Choisir le mot de passe administrateur

Quand la fenêtre demande **Choisissez un mot de passe pour 'admin'** :

1. Tapez un mot de passe d'au moins 8 caractères, sans guillemets. **Rien ne s'affiche quand vous tapez**, c'est normal.
2. Appuyez sur **Entrée**, retapez-le, puis **Entrée**.
3. Notez-le : c'est lui qui ouvre NetBox.

### B6. Se connecter à NetBox

Le navigateur s'ouvre tout seul sur **`http://localhost:8000`**. Sinon, ouvrez Edge et tapez cette adresse.

1. **Username** : `admin`
2. **Password** : le mot de passe choisi en B5
3. Cliquez **Sign In**.

&#91;embedded content: écran simplifié · page de connexion NetBox\]

Vous êtes dans NetBox : l'installation est terminée. Appuyez sur Entrée dans la fenêtre bleue pour la fermer.

## Au quotidien

Vos données restent en place quand vous arrêtez NetBox ou éteignez le PC. Sur ce PC, NetBox fonctionne comme ceci :

&#91;embedded content: ce qui tourne sur le PC · 5 conteneurs\]

Pour les commandes ci-dessous, ouvrez **Terminal** (clic droit sur **Démarrer** → **Terminal**) et tapez d'abord :

```powershell
cd C:\NetBoxSetup\netbox-docker-release
```

| Je veux… | Ce que je fais |
| --- | --- |
| Utiliser NetBox après avoir allumé le PC | Ouvrez **Docker Desktop**, attendez la pastille verte **Engine running**, puis allez sur `http://localhost:8000`. Si la page ne s'affiche pas : `docker compose start` |
| Arrêter NetBox | `docker compose stop` |
| Voir si tout tourne | `docker compose ps` (la ligne netbox doit indiquer **healthy**) |
| Voir les messages de NetBox | `docker compose logs -f netbox` (Ctrl+C pour quitter) |
| Installer sur un autre PC hors ligne | Réutilisez le même `netbox-hors-ligne.zip` et refaites B1 à B6. |
| Passer à une version plus récente | Refaites A4 à A6 pour obtenir un nouveau ZIP, puis demandez-moi la procédure de mise à jour (elle préserve vos données). |

**Sauvegarder vos données** (par exemple chaque semaine) :

```powershell
docker compose exec postgres sh -c 'pg_dump -U $POSTGRES_USER $POSTGRES_DB > /tmp/sauvegarde.sql'
```

```powershell
docker compose cp postgres:/tmp/sauvegarde.sql .\sauvegarde-netbox.sql
```

Un fichier `sauvegarde-netbox.sql` apparaît dans `C:\NetBoxSetup\netbox-docker-release`. Copiez-le sur une clé USB ou un autre disque.

**Attention :** ne tapez jamais `docker compose down -v`. Le `-v` efface toutes les données NetBox.

## En cas de problème

Tout ce que fait le script est enregistré dans `C:\NetBoxSetup\install.log` : en cas d'erreur, envoyez-moi ce fichier ou le message en rouge.

| Ce que vous voyez | Ce qu'il faut faire |
| --- | --- |
| **GitHub :** pas de bouton **Run workflow** | Le fichier n'est pas au bon endroit. Vérifiez qu'il s'appelle exactement `.github/workflows/netbox-hors-ligne.yml` (avec le point au début). |
| **GitHub :** croix rouge au lieu de la coche verte | Cliquez sur la ligne, puis sur **paquet** : l'étape en rouge montre l'erreur. Souvent c'est passager : cliquez **Re-run all jobs**. |
| **GitHub :** pas de section Artifacts | L'exécution n'est pas finie, ou elle date de plus de 7 jours. Attendez la coche verte, ou refaites A4. |
| Windows refuse d'ouvrir le script, ou il se ferme aussitôt | Le ZIP n'a pas été débloqué. Supprimez `C:\NetBoxSetup`, refaites B2 en cochant bien **Débloquer**. |
| **ERREUR : La virtualisation est desactivee** | Activez-la dans le BIOS (B1), puis relancez `installer-netbox.bat`. |
| La fenêtre ne se rouvre pas après le redémarrage | Relancez simplement `installer-netbox.bat` dans `C:\NetBoxSetup` : il reprend où il en était. |
| **ERREUR : Docker ne demarre pas** | Ouvrez Docker Desktop pour lire son message. S'il parle de WSL ou de virtualisation, redémarrez le PC une fois de plus, puis relancez le script. |
| Docker Desktop demande `wsl --update` | Ignorez-le : cette commande a besoin d'Internet. WSL est déjà installé par le script. |
| **ERREUR : docker compose up a echoue (netbox is unhealthy), ou NetBox n'est pas pret apres 20 minutes** | Relancez le script : sur un PC lent, le premier démarrage peut prendre plus longtemps. |
| **port is already allocated** | Le port 8000 est déjà pris. Ouvrez `installer-netbox.ps1` avec le Bloc-notes, remplacez `$Port = 8000` par `$Port = 8081`, enregistrez, relancez. NetBox sera sur `http://localhost:8081`. |
| Mot de passe admin oublié | Dans le Terminal (voir Au quotidien) : `docker compose exec netbox /opt/netbox/netbox/manage.py changepassword admin` |

## Annexe — Le fichier à coller dans GitHub

C'est le fichier de l'étape A3, à copier **en entier** sans rien modifier, sous le nom `.github/workflows/netbox-hors-ligne.yml`. Il télécharge NetBox, ses images, les installeurs de Docker et de WSL, et il écrit les deux scripts d'installation du PC hors ligne. GitHub réunit le tout dans le ZIP.

```yaml
# Prepare le paquet NetBox hors ligne (images Docker + installeurs + scripts).
# Lancement : onglet Actions > "Preparer NetBox hors ligne" > Run workflow.
name: Preparer NetBox hors ligne

on:
  workflow_dispatch:

jobs:
  paquet:
    runs-on: ubuntu-latest
    steps:
      - name: Telecharger netbox-docker (branche release)
        run: |
          mkdir -p paquet
          git clone --depth 1 -b release https://github.com/netbox-community/netbox-docker.git paquet/netbox-docker-release
          rm -rf paquet/netbox-docker-release/.git
          cp paquet/netbox-docker-release/docker-compose.override.yml.example paquet/netbox-docker-release/docker-compose.override.yml

      - name: Telecharger les images NetBox et les mettre dans un fichier
        working-directory: paquet/netbox-docker-release
        run: |
          docker compose pull
          docker save -o netbox-images.tar $(docker compose config --images)
          ls -lh netbox-images.tar

      - name: Telecharger les installeurs Docker Desktop et WSL
        env:
          GH_TOKEN: ${{ github.token }}
        run: |
          curl -fL -o "paquet/Docker Desktop Installer.exe" "https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe"
          gh release download --repo microsoft/WSL --pattern '*.x64.msi' --dir paquet

      - name: Ecrire les scripts d'installation
        run: |
          cat > paquet/installer-netbox.bat <<'NBEOF'
          @echo off
          rem Double-cliquez sur ce fichier pour installer NetBox.
          rem Il se relance automatiquement en administrateur.

          net session >nul 2>&1
          if %errorlevel% neq 0 (
              powershell -NoProfile -Command "Start-Process -FilePath '%~f0' -Verb RunAs"
              exit /b
          )

          powershell -NoProfile -ExecutionPolicy Bypass -File "%~dp0installer-netbox.ps1"
          NBEOF
          cat > paquet/installer-netbox.ps1 <<'NBEOF'
          # =====================================================================
          #  Installation automatique de NetBox sur un PC Windows 11 SANS Internet
          #  A lancer via installer-netbox.bat (clic droit > Executer en tant qu'admin
          #  n'est pas necessaire : le .bat le demande tout seul).
          #
          #  Contenu attendu a cote de ce script :
          #    - Docker Desktop Installer.exe
          #    - wsl.X.X.X.X.x64.msi
          #    - netbox-docker-release\   (avec netbox-images.tar dedans)
          #
          #  Le script peut etre relance autant de fois que necessaire :
          #  il saute les etapes deja faites.
          # =====================================================================

          # 'Continue' : les erreurs des commandes docker sont verifiees une par une
          # (en mode 'Stop', PowerShell 5.1 plante sur leurs simples messages d'information)
          $ErrorActionPreference = 'Continue'

          # ---------- Reglages (modifiables) ----------
          $Dest        = 'C:\NetBoxSetup'          # dossier d'installation local
          $AdminUser   = 'admin'                   # nom du compte NetBox a creer
          $AdminEmail  = 'admin@example.com'
          $Port        = 8000                      # http://localhost:8000
          $TaskName    = 'InstallNetBox-Reprise'
          # --------------------------------------------

          $ProjectDir  = Join-Path $Dest 'netbox-docker-release'
          $ImagesTar   = Join-Path $ProjectDir 'netbox-images.tar'
          $DockerExe   = 'C:\Program Files\Docker\Docker\Docker Desktop.exe'
          $DockerBin   = 'C:\Program Files\Docker\Docker\resources\bin'
          $WslExe      = 'C:\Program Files\WSL\wsl.exe'

          New-Item -ItemType Directory -Force -Path $Dest | Out-Null
          Start-Transcript -Path (Join-Path $Dest 'install.log') -Append | Out-Null

          function Etape($texte) { Write-Host "`n==> $texte" -ForegroundColor Cyan }
          function Ok($texte)    { Write-Host "    OK : $texte" -ForegroundColor Green }
          function Stop-Erreur($texte) {
              Write-Host "`n    ERREUR : $texte" -ForegroundColor Red
              Stop-Transcript | Out-Null
              Read-Host "`nAppuyez sur Entree pour fermer"
              exit 1
          }

          # ---------- 0. Droits administrateur ----------
          $estAdmin = ([Security.Principal.WindowsPrincipal][Security.Principal.WindowsIdentity]::GetCurrent()
                      ).IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)
          if (-not $estAdmin) { Stop-Erreur "Lancez le script via installer-netbox.bat (droits administrateur requis)." }

          # ---------- 1. Copier les fichiers de la cle vers C:\NetBoxSetup ----------
          Etape "Copie des fichiers vers $Dest"
          $Src = $PSScriptRoot
          if ($Src.TrimEnd('\') -ne $Dest.TrimEnd('\')) {
              $deja = (Test-Path $ImagesTar) -and
                      ((Get-Item $ImagesTar).Length -eq (Get-Item (Join-Path $Src 'netbox-docker-release\netbox-images.tar') -ErrorAction SilentlyContinue).Length)
              if ($deja) {
                  Ok "fichiers deja copies"
              } else {
                  Write-Host "    Copie en cours (1 a 3 minutes)..."
                  try { Copy-Item -Path (Join-Path $Src '*') -Destination $Dest -Recurse -Force -ErrorAction Stop }
                  catch { Stop-Erreur "copie impossible : $($_.Exception.Message)" }
                  Ok "copie terminee"
              }
          }
          Get-ChildItem -Path $Dest -Recurse | Unblock-File

          $DockerSetup = Get-ChildItem -Path $Dest -Filter 'Docker Desktop Installer.exe' | Select-Object -First 1
          $WslMsi      = Get-ChildItem -Path $Dest -Filter 'wsl*x64.msi' | Select-Object -First 1
          if (-not $DockerSetup)          { Stop-Erreur "'Docker Desktop Installer.exe' introuvable a cote du script." }
          if (-not $WslMsi)               { Stop-Erreur "Fichier 'wsl...x64.msi' introuvable a cote du script." }
          if (-not (Test-Path $ImagesTar)) { Stop-Erreur "netbox-docker-release\netbox-images.tar introuvable." }

          # ---------- 2. Virtualisation ----------
          Etape "Verification de la virtualisation"
          $cpu = Get-CimInstance Win32_Processor | Select-Object -First 1
          $sys = Get-CimInstance Win32_ComputerSystem
          if (-not ($cpu.VirtualizationFirmwareEnabled -or $sys.HypervisorPresent)) {
              Stop-Erreur "La virtualisation est desactivee. Activez-la dans le BIOS (Intel VT-x / AMD SVM), puis relancez le script."
          }
          Ok "virtualisation activee"

          $redemarrageNecessaire = $false

          # ---------- 3. Fonctions Windows ----------
          Etape "Activation des fonctions Windows (WSL + plateforme de machine virtuelle)"
          foreach ($f in 'Microsoft-Windows-Subsystem-Linux', 'VirtualMachinePlatform') {
              $etat = (Get-WindowsOptionalFeature -Online -FeatureName $f).State
              if ($etat -ne 'Enabled') {
                  $r = Enable-WindowsOptionalFeature -Online -FeatureName $f -All -NoRestart
                  if ($r.RestartNeeded) { $redemarrageNecessaire = $true }
                  Ok "$f activee"
              } else { Ok "$f deja activee" }
          }

          # ---------- 4. WSL ----------
          Etape "Installation de WSL"
          if (Test-Path $WslExe) {
              Ok "WSL deja installe"
          } else {
              $p = Start-Process msiexec.exe -ArgumentList "/i `"$($WslMsi.FullName)`" /quiet /norestart" -Wait -PassThru
              if ($p.ExitCode -notin 0, 3010) { Stop-Erreur "installation de WSL echouee (code $($p.ExitCode))." }
              if ($p.ExitCode -eq 3010) { $redemarrageNecessaire = $true }
              Ok "WSL installe"
          }

          # ---------- 5. Docker Desktop ----------
          Etape "Installation de Docker Desktop"
          if (Test-Path $DockerExe) {
              Ok "Docker Desktop deja installe"
          } else {
              Write-Host "    Installation silencieuse (3 a 5 minutes)..."
              $p = Start-Process $DockerSetup.FullName -ArgumentList 'install', '--quiet', '--accept-license', '--backend=wsl-2' -Wait -PassThru
              if ($p.ExitCode -ne 0) { Stop-Erreur "installation de Docker echouee (code $($p.ExitCode))." }
              $redemarrageNecessaire = $true
              Ok "Docker Desktop installe"
          }
          # l'utilisateur doit etre dans le groupe docker-users
          $moi = "$env:USERDOMAIN\$env:USERNAME"
          $membres = net localgroup docker-users 2>$null
          if (-not ($membres -match [regex]::Escape($env:USERNAME))) {
              net localgroup docker-users $moi /add | Out-Null
              $redemarrageNecessaire = $true
              Ok "$moi ajoute au groupe docker-users"
          }

          # ---------- 6. Redemarrage puis reprise automatique ----------
          if ($redemarrageNecessaire) {
              Etape "Redemarrage necessaire"
              $action    = New-ScheduledTaskAction -Execute 'powershell.exe' `
                           -Argument "-NoProfile -ExecutionPolicy Bypass -File `"$Dest\installer-netbox.ps1`""
              $trigger   = New-ScheduledTaskTrigger -AtLogOn -User $moi
              $principal = New-ScheduledTaskPrincipal -UserId $moi -LogonType Interactive -RunLevel Highest
              Register-ScheduledTask -TaskName $TaskName -Action $action -Trigger $trigger -Principal $principal -Force | Out-Null
              Write-Host "    Le PC va redemarrer. Reconnectez-vous avec le MEME compte :" -ForegroundColor Yellow
              Write-Host "    l'installation reprendra toute seule." -ForegroundColor Yellow
              Stop-Transcript | Out-Null
              Read-Host "`nAppuyez sur Entree pour redemarrer maintenant"
              Restart-Computer -Force
              exit 0
          }
          Unregister-ScheduledTask -TaskName $TaskName -Confirm:$false -ErrorAction SilentlyContinue

          # ---------- 7. Demarrer Docker ----------
          Etape "Demarrage de Docker (jusqu'a 5 minutes)"
          $env:Path = "$DockerBin;$env:Path"
          # lance via l'explorateur pour que Docker Desktop tourne SANS droits admin (mode normal)
          if (-not (Get-Process 'Docker Desktop' -ErrorAction SilentlyContinue)) { Start-Process explorer.exe -ArgumentList "`"$DockerExe`"" }
          $limite = (Get-Date).AddMinutes(5)
          do {
              Start-Sleep -Seconds 5
              docker info *> $null
          } until ($LASTEXITCODE -eq 0 -or (Get-Date) -gt $limite)
          if ($LASTEXITCODE -ne 0) { Stop-Erreur "Docker ne demarre pas. Ouvrez Docker Desktop pour voir le message, puis relancez le script." }
          Ok "Docker est pret"

          Set-Location $ProjectDir

          # ---------- 8. Charger les images ----------
          Etape "Chargement des images NetBox"
          $override = Join-Path $ProjectDir 'docker-compose.override.yml'
          if (-not (Test-Path $override)) {
              Copy-Item (Join-Path $ProjectDir 'docker-compose.override.yml.example') $override
          }
          # port + delai de demarrage plus long : au 1er lancement NetBox prepare sa base,
          # ce qui depasse souvent les 2 minutes prevues par defaut
          $yaml = @(
              'services:',
              '  netbox:',
              '    ports:',
              "      - `"${Port}:8080`"",
              '    healthcheck:',
              '      start_period: 900s'
          )
          [IO.File]::WriteAllLines($override, $yaml)   # UTF-8 sans BOM, sinon docker compose peut refuser le fichier

          $manquantes = docker compose config --images | Where-Object { docker image inspect $_ *> $null; $LASTEXITCODE -ne 0 }
          if ($manquantes) {
              Write-Host "    Chargement de netbox-images.tar (1 a 3 minutes)..."
              docker load -i $ImagesTar
              if ($LASTEXITCODE -ne 0) { Stop-Erreur "docker load a echoue." }
              Ok "images chargees"
          } else { Ok "images deja presentes" }

          # ---------- 9. Lancer NetBox ----------
          Etape "Demarrage de NetBox (5 a 20 minutes au premier lancement)"
          docker compose up -d --pull never
          if ($LASTEXITCODE -ne 0) { Write-Host "    NetBox prepare encore sa base de donnees, on patiente..." -ForegroundColor Yellow }
          $limite = (Get-Date).AddMinutes(20)
          do {
              Start-Sleep -Seconds 10
              $id = docker compose ps -q netbox
              $sante = if ($id) { docker inspect -f '{{.State.Health.Status}}' $id } else { 'absent' }
              Write-Host "    etat de netbox : $sante"
          } until ($sante -eq 'healthy' -or (Get-Date) -gt $limite)
          if ($sante -ne 'healthy') { Stop-Erreur "NetBox n'est pas pret apres 20 minutes. Relancez le script, ou voir : docker compose logs netbox" }
          # 2e passage : demarre les conteneurs qui attendaient que NetBox soit pret (worker, etc.)
          docker compose up -d --pull never
          if ($LASTEXITCODE -ne 0) { Stop-Erreur "docker compose up a echoue (voir les messages ci-dessus)." }
          Ok "NetBox fonctionne"

          # ---------- 10. Compte administrateur ----------
          Etape "Creation du compte administrateur '$AdminUser'"
          $test = docker compose exec -T netbox /opt/netbox/netbox/manage.py shell -c "from django.contrib.auth import get_user_model; print(get_user_model().objects.filter(username='$AdminUser').exists())" 2>$null
          if ("$test" -match 'True') {
              Ok "le compte '$AdminUser' existe deja"
          } else {
              do {
                  $mdp1 = Read-Host "    Choisissez un mot de passe pour '$AdminUser'" -AsSecureString
                  $mdp2 = Read-Host "    Retapez-le" -AsSecureString
                  $p1 = [Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringToBSTR($mdp1))
                  $p2 = [Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringToBSTR($mdp2))
                  if ($p1 -ne $p2 -or $p1.Length -lt 8) { Write-Host "    Les mots de passe different ou font moins de 8 caracteres. Recommencez." -ForegroundColor Yellow }
              } until ($p1 -eq $p2 -and $p1.Length -ge 8)
              docker compose exec -T -e "DJANGO_SUPERUSER_PASSWORD=$p1" netbox /opt/netbox/netbox/manage.py createsuperuser --noinput --username $AdminUser --email $AdminEmail
              if ($LASTEXITCODE -ne 0) { Stop-Erreur "creation du compte echouee." }
              Ok "compte cree"
          }

          # ---------- Fin ----------
          Etape "Installation terminee"
          Write-Host "    Ouvrez http://localhost:$Port et connectez-vous avec '$AdminUser'." -ForegroundColor Green
          Start-Process "http://localhost:$Port"
          Stop-Transcript | Out-Null
          Read-Host "`nAppuyez sur Entree pour fermer"
          NBEOF
          sed -i 's/\r*$/\r/' paquet/installer-netbox.bat paquet/installer-netbox.ps1
          ls -la paquet

      - name: Mettre le paquet a disposition
        uses: actions/upload-artifact@v4
        with:
          name: netbox-hors-ligne
          path: paquet/
          retention-days: 7
```
