# Instalar Docker usando WSL2 sem a necessidade de Docker Desktop

Habilitar funcionalidades no Windows (Um ou mais reboots serão necessários)

Powershell (Administrador)

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```
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
mkdir c:\temp
mkdir d:\wsl
Invoke-WebRequest -Uri https://dl.rockylinux.org/pub/rocky/9/images/x86_64/Rocky-9-Container-Base.latest.x86_64.tar.xz -OutFile C:\temp\rockylinux.tar.xz
wsl --import rockylinux d:\wsl\rockylinux C:\temp\rockylinux.tar.xz
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
dnf -y install systemd
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
sed -i 's@mirrorlist?arch@mirrorlist?cc=br\&arch@g' /etc/yum.repos.d/rocky*repo
echo 'fastestmirror=True' >> /etc/dnf/dnf.conf
echo 'max_parallel_downloads=10' >> /etc/dnf/dnf.conf
echo 'defaultyes=True' >> /etc/dnf/dnf.conf
echo 'keepcache=True' >> /etc/dnf/dnf.conf
dnf -y clean all
dnf -y update && dnf -y upgrade
dnf -y install 'dnf-command(config-manager)'
dnf -y install sudo git iproute net-tools procps-ng mlocate cockpit passwd
```
Instalação do docker no RockyLinux
```
dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
dnf -y install docker-ce docker-ce-cli containerd.io
```
Iniciar o serviço Docker
```
systemctl enable docker
systemctl start docker
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

Contratulations ! Docker is installed using WSL !!!



