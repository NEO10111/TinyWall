# TinyWall MCP Server (Model Context Protocol)

Ce serveur MCP permet à un agent IA (Claude Desktop, Cursor, Antigravity, VS Code Copilot, etc.) de contrôler et configurer **TinyWall** via des outils déclaratifs standardisés.

---

## 1. Outils fournis à l'agent IA

| Outil MCP | Description | Paramètres |
|---|---|---|
| `tinywall_get_status` | Récupère le mode actuel, l'état de verrouillage et le profil actif | Aucun |
| `tinywall_set_mode` | Change le mode du pare-feu | `mode`: `"normal"`, `"blockall"`, `"allowoutgoing"`, `"disabled"`, `"autolearn"` |
| `tinywall_list_rules` | Liste l'ensemble des règles et applications configurées | Aucun |
| `tinywall_allow_program`| Autorise une application | `path` (obligatoire), `policy` (`"tcpudp"` ou `"unrestricted"`) |
| `tinywall_block_program`| Bloque formellement une application | `path` (obligatoire) |
| `tinywall_remove_rule`  | Supprime une règle | `path` ou `id` (GUID) |
| `tinywall_get_recent_logs` | Récupère les connexions et paquets bloqués récents | `limit` (optionnel, défaut: 25) |

---

## 2. Configuration MCP pour votre Agent

### Exemple pour `claude_desktop_config.json` ou `cursor / antigravity` :

```json
{
  "mcpServers": {
    "tinywall": {
      "command": "python",
      "args": [
        "C:\\Users\\Dell\\Desktop\\TinyWall\\mcp-server\\tinywall_mcp.py"
      ]
    }
  }
}
```

---

## 3. Mise en place du binaire TinyWall

Rappel : Pour que le serveur MCP et les commandes puissent dialoguer avec le service Windows actif :
1. Arrêtez le service Windows : `Stop-Service TinyWall` (ou via le gestionnaire de services).
2. Remplacez `C:\Program Files (x86)\TinyWall\TinyWall.exe` par `C:\Users\Dell\Desktop\TinyWall\TinyWall\bin\Release\TinyWall.exe`.
3. Relancez le service : `Start-Service TinyWall`.

Une fois le binaire mis en place, votre agent IA pourra autoriser, bloquer et surveiller votre pare-feu de façon autonome !
