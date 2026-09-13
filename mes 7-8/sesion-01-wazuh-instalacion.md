
---

## Secciones del dashboard

- **Security Alerts:** alertas de seguridad en tiempo real
- **Integrity Monitoring:** cambios en archivos (permisos, contenido)
- **Vulnerability Detection:** vulnerabilidades conocidas en los equipos
- **MITRE ATT&CK:** eventos mapeados al framework de ataques
- **Compliance:** GDPR, HIPAA, NIST, PCI DSS

---

## Instalación en Kali

```bash
# Descargar instalador
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh

# Dar permisos de ejecución
chmod u+x wazuh-install.sh

# Instalar todos los componentes ignorando chequeo de OS
sudo bash wazuh-install.sh -a -i

# Verificar estado
sudo systemctl status wazuh-manager

# Reiniciar si es necesario
sudo systemctl restart wazuh-manager
```

**Acceso al dashboard:** https://127.0.0.1
El certificado es self-signed: aceptar el riesgo en Firefox.

---

## Requisitos de hardware

Mínimo recomendado: 4 GB de RAM.
Con 2 GB el sistema arranca lento y puede fallar.
