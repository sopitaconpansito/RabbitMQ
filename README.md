# RabbitMQ + Scapy MITM Playground

Repositorio para pruebas de interceptación y modificación de tráfico AMQP con Scapy, NetfilterQueue y Docker.

-----------------------------------------------------------
📂 Carpetas y Archivos

RabbitMQ/
├── docker-compose.yml
├── sender/
│   ├── Dockerfile
│   └── sender_cli.py           # CLI: auto-spam o modo interactivo
├── receiver/
│   ├── Dockerfile
│   └── receiver.py             # imprime cada msg al instante
├── interceptor_nfqueue.py      # modifica tráfico en vivo: M-1 y M-2
└── heartbeat_bad.py            # inyecta manualmente un heartbeat inválido (M-5)

-----------------------------------------------------------
🧪 ¿Qué hace cada cosa?

rabbitmq                → Broker AMQP + panel web (puertos 5672 y 15672)
sender                  → Envía mensajes: modo interactivo (-i) o automático (--freq, -m)
receiver                → Escucha la cola y muestra cada mensaje
interceptor_nfqueue.py  → Intercepta tráfico con iptables + NFQUEUE, aplica M-1 y M-2
heartbeat_bad.py        → Inyecta manualmente un heartbeat inválido (M-5) para cortar conexión

-----------------------------------------------------------
⚙️ Instalación rápida

1) Construir imágenes:
   docker compose build

2) Levantar broker + receiver:
   docker compose up -d rabbit receiver

3) Ver logs del receiver:
   docker compose logs -f receiver

4) Enviar mensajes:
   a) Modo interactivo:
      docker compose run --rm sender -i

   b) Auto spam cada 0.3 s:
      docker compose run --rm sender --freq 0.3 -m "Hola"

Accede al dashboard: http://localhost:15672 (usuario: gabriel / pass: insaid33)

-----------------------------------------------------------
🐍 Scripts MITM (Scapy)

Interceptor en vivo (M-1, M-2):
   sudo python3 interceptor_nfqueue.py

Inyección heartbeat inválido (M-5):
   python3 heartbeat_bad.py

-----------------------------------------------------------
🧹 Limpiar todo

Bajar contenedores, borrar volúmenes e imágenes del compose:
   docker compose down -v --rmi all

(opcional) botón nuclear global (⚠️ borra TODO Docker):
   docker system prune -a --volumes --force

-----------------------------------------------------------
📦 Requisitos

- Docker ≥ 20.x + Docker Compose v2
- Python ≥ 3.x con scapy y netfilterqueue
- Acceso sudo para iptables

-----------------------------------------------------------
🔮 Roadmap del laboratorio

[x] Sniffer básico (captura PCAP)
[x] Modificación en vivo con NFQUEUE (M-1, M-2)
[x] Inyección manual heartbeat inválido (M-5)
[ ] Fuzzing de payloads AMQP (opcional)
[x] Documentación de resultados e imágenes
[x] Video demostrativo
[x] README y entrega final

-----------------------------------------------------------
⚠️ Disclaimer

Proyecto para fines educativos y pruebas en entorno controlado.
No usar en producción ni contra sistemas que no sean de tu propiedad.
