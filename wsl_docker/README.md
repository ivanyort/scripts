# Instalar Docker usando WSL2 sem a necessidade de Docker Desktop

Habilitar funcionalidades no Windows (Um ou mais reboots serão necessários)

Powershell (Administrador)

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName 'Containers' -All
```
```powershell
Enable-WindowsOptionalFeature -Online -FeatureName 'Microsoft-Hyper-V' -All
```
```powershell
Enable-WindowsOptionalFeature -Online -FeatureName 'VirtualMachinePlatform' -All
```
```powershell
Enable-WindowsOptionalFeature -Online -FeatureName 'Microsoft-Windows-Subsystem-Linux' -All
```


Vou utilizar RockyLinux ao inves do Ubuntu
```
Invoke-WebRequest -Uri https://dl.rockylinux.org/pub/rocky/8/images/x86_64/Rocky-8-Container-Base.latest.x86_64.tar.xz -OutFile D:\temp\rockylinux.tar.xz
wsl --import RockyLinux D:\wsl\rockylinux D:\temp\rockylinux.tar.xz
wsl --list
```

Iniciar o RockyLinux (Faz o login como root automaticamente)

```
wsl
```
A partir deste momento os comandos serão no terminal RockyLinux

Habilitar o systemd
```
echo '[boot]' > /etc/wsl.conf
echo 'systemd=true' >> /etc/wsl.conf
```
Desabilitar o ipv6
```
echo 'net.ipv6.conf.all.disable_ipv6 = 1' > /etc/sysctl.d/01_dis_ipv6.conf
echo 'net.ipv6.conf.default.disable_ipv6 = 1' >> /etc/sysctl.d/01_dis_ipv6.conf
```

Será necessário reiniciar o RockyLinux, para isso saia do terminal e reinicie o RockyLinux
```
exit
wsl --shutdown
wsl
```
Pacotes necessários

```
dnf install -y glibc-all-langpacks langpacks-en
localectl set-locale LANG=en_US.UTF-8
dnf -y update && dnf -y upgrade
dnf -y install 'dnf-command(config-manager)'
dnf -y install sudo git iproute net-tools procps-ng mlocate cockpit passwd
```
Instalação do docker no RockyLinux
```
dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
dnf -y install docker-ce docker-ce-cli containerd.io
```
Para possibilitar conexões remotas
```
systemctl enable docker
systemctl start docker
mkdir -p /etc/systemd/system/docker.service.d
echo '[Service]' > /etc/systemd/system/docker.service.d/options.conf
echo 'ExecStart=' >> /etc/systemd/system/docker.service.d/options.conf
echo 'ExecStart=/usr/bin/dockerd -H unix:// -H tcp://0.0.0.0:2375' >> /etc/systemd/system/docker.service.d/options.conf
systemctl daemon-reload
systemctl restart docker
```

Verificação da instalação do Docker
```
systemctl status docker
```
```
docker run --name hello-world hello-world
```

O container hello-world e sua imagem pode ser apagadas
```
docker container rm -f hello-world
docker system prune -af --volumes
```
Compilar o docker (client) para windows. Isso é feito dentro do RockyLinux
```
cd /root
git clone https://github.com/docker/cli.git
cd cli
docker buildx bake
docker buildx bake --set binary.platform=windows/amd64
```
Se tudo funcionou até aqui, podemos copiar o cliente docker windows para o diretorio c:\tools (este diretorio deve ser incluido no path do windows)
```
mkdir -p /mnt/c/tools
cp build/docker-windows-amd64.exe /mnt/c/tools/docker.exe
```
Voltando ao Powershell para erminar a configuração do cliente docker
```
C:
cd c:\tools
.\docker
```

Não usar comandos abaixo


```
docker login
docker context create wsl --docker "host=tcp://<YOUR_WSL_IP>:2375"
docker context use wsl

wsl -- ip -o -4 -json addr list eth0 `
| ConvertFrom-Json `
| %{ $_.addr_info.local } `
| ?{ $_ }

$wslip = wsl -- ip -o -4 -json addr list eth0 | ConvertFrom-Json | %{ $_.addr_info.local } ` | ?{ $_ }
Write-Host "Setting Docker context 'wsl' to host=tcp://$($wslip):2375"
docker context update wsl --docker "host=tcp://$($wslip):2375"
docker context use wsl

```