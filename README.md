# FamilyHub — Add-on Home Assistant

Application de gestion familiale complète intégrée à Home Assistant.

## Fonctionnalités

- 📅 Agenda, planning semaine, anniversaires, Google Calendar
- ✅ Tâches, ménage, points enfants
- 🛒 Courses, menus, stock
- 💰 Budget, dépenses, abonnements
- 📄 Documents, médicaments, vaccins, RDV
- 🎒 Enfants — emploi du temps, devoirs
- ✈️ Vacances, projets familiaux
- 👨‍👧‍👦 Coparentalité — suivi frais garde alternée
- 🔒 Multi-profils, PINs, permissions par onglet
- 📑 Onglets personnalisés
- 🏠 Intégration HA — présence, calendriers, services
- 📊 Export Excel

## Installation

1. HA → **Paramètres → Modules complémentaires → Dépôts**
2. Ajoute : `https://github.com/TON_PSEUDO/familyhub-addon`
3. Installe **FamilyHub**
4. Configure et démarre

## Configuration

```yaml
secret: "ton_mot_de_passe"   # Protège l'API
log_level: info
api_port: 3001               # Port exposé pour la sync
```

## Accès multi-appareils

Dans FamilyHub → **Paramètres → NAS** :

| Champ    | Valeur                        |
|----------|-------------------------------|
| Hôte     | `hatissot.duckdns.org`        |
| Port API | `3001`                        |
| Protocole| HTTPS                         |
| Secret   | même que dans la config addon |

> Le port 3001 doit être ouvert sur ta box (redirection vers le Raspberry Pi).

## Webhook HA → FamilyHub

```yaml
# Dans une automatisation HA
action:
  service: rest_command.familyhub_webhook
  data:
    type: "presence"
    person: "Jean"
    state: "home"

rest_command:
  familyhub_webhook:
    url: "http://localhost:3001/api/webhook"
    method: POST
    content_type: "application/json"
    headers:
      x-webhook-token: "ton_secret"
    payload: '{"type":"{{ type }}","person":"{{ person }}","state":"{{ state }}"}'
```

## Sauvegarde

Les données sont dans `/data/familyhub.db` — incluses dans les sauvegardes HA.
