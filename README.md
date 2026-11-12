# CyberHealth

**Diplomarbeit (2025/26) · in Zusammenarbeit mit [Cyber Security Austria](https://www.cybersecurityaustria.at) als Projektpartner und Auftraggeber**

CyberHealth ist eine Webapplikation, die Gesundheitseinrichtungen dabei unterstützt, die Anforderungen der EU-Richtlinie **NIS2** und des österreichischen **NISG** zu erfüllen. IT-Systeme und medizinische Geräte werden zentral erfasst, mit bekannten Schwachstellen abgeglichen und für Audits dokumentiert.

> ⚠️ **Hinweis:** Da CyberHealth in Kooperation mit Cyber Security Austria entstanden ist, wird der Quellcode hier bewusst nicht veröffentlicht.
---

## Ausgangslage

Gerade bei medizinischen Geräten, die sich oft nicht einfach updaten lassen, fehlt in Gesundheitseinrichtungen häufig ein zentraler Überblick über den Sicherheitsstatus. Gleichzeitig verlangen NIS2 und NISG, dass Risiken bekannt, bewertet und nachweisbar dokumentiert sind.

Das Gesamtsystem wurde auf **zwei Diplomarbeiten** aufgeteilt:

| | Team | Schwerpunkt |
|---|---|---|
| **Teil 1 – CyberHealth** | Alice Hristov, Sudenur Yildiz, Fatma Yilmaz, Klara Leal Bukowska | Asset-Erfassung (CMDB), EUVD-Abgleich, Sicherheitsarchitektur, Containerisierung, Berichte |
| **Teil 2** | zweites Diplomarbeitsteam | Vertiefte Schwachstellenanalyse und GAP-Bericht mit Handlungsempfehlungen |

---

## Mein Teil – Alice Hristov

**Thema:** *Modellierung der zentralen CMDB zur Erfassung von IT-Assets inkl. sicherer Speicherung und Zugriffsbeschränkung durch Verschlüsselung*

Umgesetzt nach den drei Schutzzielen der CIA-Triade (Vertraulichkeit, Integrität, Verfügbarkeit):

- **CMDB-Modellierung** – Datenmodell zur zentralen Erfassung von IT-Assets und medizinischen Geräten als Datenbasis für die Schwachstellenbewertung
- **Feldgranulare Verschlüsselung** – sensible Felder (z. B. Seriennummern, Schwachstellenbeschreibungen) werden mit **AES-256-GCM** über **Jasypt** und einen **JPA-Converter** verschlüsselt, bevor sie gespeichert werden. Die Technologie wurde per Nutzwertanalyse ausgewählt.
- **Verschlüsselte Übertragung** – **NGINX** als Reverse Proxy mit TLS-Terminierung und automatischer Weiterleitung von HTTP auf HTTPS
- **Identity & Access Management** – **Keycloak** als Identity Provider mit OAuth2 / OpenID Connect, JWT-basierter Authentifizierung, rollenbasierter Zugriffskontrolle (RBAC) und Zwei-Faktor-Authentifizierung

---

## Architektur

![Systemarchitektur von CyberHealth](docs/architektur.png)

Alle Komponenten laufen als Docker-Container in einem Docker Swarm. Der Browser kommuniziert ausschließlich per HTTPS mit dem NGINX Reverse Proxy, der die TLS-Verbindung terminiert und Anfragen an das Spring-Boot-Backend oder den Client weiterleitet. Keycloak übernimmt die Anmeldung, das Backend gleicht Assets über die EUVD-API der ENISA ab und speichert sensible Felder verschlüsselt in PostgreSQL.

## Tech-Stack

| Bereich | Technologien |
|---|---|
| Frontend | Angular |
| Backend | Java, Spring Boot, JPA |
| Datenbank | PostgreSQL |
| Sicherheit | Keycloak, OAuth2 / OIDC, JWT, Jasypt, AES-256-GCM, TLS |
| Infrastruktur | Docker, Docker Compose, NGINX |
| Externe Daten | EUVD-API der ENISA (European Union Vulnerability Database), CVSS |

---
