
```bash
autonmap () {

RED='\033[1;31m'
GREEN='\033[1;32m'
PURPLE='\033[1;35m'
if [ $# -ne 1 ]; then
 echo -e "${RED}[-] Debes proporcionar una dirección IP como argumento."
 echo -e "${RED}[!] Ejemplo: autonmap <IP>"
 return 1
fi
    
echo -e "${GREEN}\n[+] Lanzando nmap al rango completo de puertos de la máquina con IP: ${PURPLE}$1"

sudo nmap -sS --min-rate 5000 -p- --open "$1" -Pn -oN nmap_inicial > /dev/null

ports=$(cat nmap_inicial | grep '^[0-9]' | cut -d '/' -f1 | xargs | tr ' ' ',')

echo -e "${GREEN}[+] Se han encontrado los siguientes puertos abiertos: ${PURPLE}$ports"
echo -e "${GREEN}[+] Analizando servicios y lanzando scripts básicos de reconocimiento..."

nmap -p"$ports" -sC -sV "$1" -Pn -oN Scan.txt > /dev/null
rm -rf nmap_inicial
echo -e "${GREEN}[+] Escaneo completado | Se ha creado el documento ${PURPLE}Scan.txt"
}
```
