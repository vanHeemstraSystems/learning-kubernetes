## K9s Installation and Usage Guide

Excellent! K9s is a fantastic terminal UI for managing Kubernetes. Let me guide you through installation and usage.

### Installation

**Option 1: Via winget (recommended)**

Open PowerShell:

```powershell
winget install derailed.k9s
```

**Option 2: Via Chocolatey**

If you have Chocolatey installed:

```powershell
choco install k9s
```

**Option 3: Manual download**

1. Go to <https://github.com/derailed/k9s/releases>
1. Download the Windows zip file (k9s_Windows_amd64.tar.gz)
1. Extract to a folder (e.g., `C:\tools\k9s`)
1. Add that folder to your PATH environment variable

**Verify installation:**

```powershell
k9s version
```

### Starting K9s

Simply type:

```powershell
k9s
```

K9s will automatically connect to your minikube cluster (it reads your kubectl config).

### Essential K9s Navigation

#### Basic Navigation

- **`:` (colon)** - Enter command mode
- **`/` (slash)** - Filter/search current view
- **`Esc`** - Exit command/filter mode or go back
- **`Ctrl+A`** - Show all available resources
- **`?`** - Show help/keyboard shortcuts
- **`:q`** or `Ctrl+C` - Quit k9s

#### Viewing Resources

Type `:` followed by resource shortcut:

```
:pods       or :po    - View pods
:deploy     or :dp    - View deployments
:svc        or :svc   - View services
:ns         or :ns    - View namespaces
:nodes      or :no    - View nodes
:events     or :ev    - View events
:configmaps or :cm    - View configmaps
:secrets    or :sec   - View secrets
:ing        or :ing   - View ingresses
:pv                   - View persistent volumes
:pvc                  - View persistent volume claims
:sa                   - View service accounts
```

#### Common Actions on Resources

When viewing a resource (e.g., pods):

- **`Enter`** - Describe selected resource
- **`d`** - Describe (same as Enter)
- **`l`** - View logs (for pods)
- **`y`** - View YAML manifest
- **`e`** - Edit resource
- **`s`** - Shell into pod (exec)
- **`Ctrl+D`** - Delete resource
- **`Ctrl+K`** - Kill pod (force delete)
- **`n`** - Switch namespace

#### Log Viewing (when in logs view)

- **`0-9`** - Toggle showing previous N logs (0=all, 1=100, 2=200, etc.)
- **`f`** - Toggle auto-scroll/follow
- **`w`** - Toggle line wrapping
- **`s`** - Toggle timestamp display
- **`/`** - Filter logs
- **`c`** - Clear logs

#### Filtering and Searching

When in any view, press **`/`** to filter:

```
/nginx          - Show only items containing "nginx"
/!running       - Exclude items containing "running"
/running|pending - Show items with "running" OR "pending"
```

### Practical Workflow Examples

#### Example 1: Deploy and Monitor nginx

In a separate PowerShell window:

```powershell
kubectl create deployment nginx --image=nginx --replicas=3
kubectl expose deployment nginx --type=NodePort --port=80
```

In k9s:

1. Type `:pods` - see your nginx pods appearing
1. Select a pod with arrow keys
1. Press `l` to view logs
1. Press `s` to shell into the pod
1. Press `:svc` to see the service
1. Press `:deploy` to see the deployment

#### Example 2: Troubleshoot a failing pod

1. `:pods` - view all pods
1. `/` then type status to filter
1. Select the failing pod
1. Press `d` - describe to see events
1. Press `l` - view logs
1. Press `y` - check YAML configuration

#### Example 3: Scale a deployment

1. `:deploy` - view deployments
1. Select deployment
1. Press `s` - scale
1. Enter new replica count
1. Watch pods scale in `:pods` view

#### Example 4: Monitor cluster resources

1. `:nodes` - view node resources
1. `:pods` - press `Shift+P` to sort by CPU/memory
1. `:xray pods` - see pod dependencies

### Useful K9s Commands

```
:ctx          - Switch Kubernetes context
:ns           - View/switch namespaces  
:xray pods    - Show pod relationships
:popeye       - Run cluster sanitizer (finds issues)
:pulse        - View cluster metrics
:rbac         - Check RBAC permissions
:screendump   - Take screenshot
:benchmarks   - Run benchmarks
```

### Namespace Operations

- Press **`0`** - View all namespaces
- Press **`1-9`** - Quick switch to numbered namespace
- Type `:ns` then select namespace and press `Enter`

### Customizing K9s

K9s stores config in: `%APPDATA%\k9s\`

Create/edit `%APPDATA%\k9s\config.yaml`:

```yaml
k9s:
  refreshRate: 2        # Refresh every 2 seconds
  maxConnRetry: 5
  readOnly: false
  noExitOnCtrlC: false
  ui:
    enableMouse: true
    headless: false
    logoless: false
    crumbsless: false
    skin: "default"     # or "dracula", "monokai", "nord", etc.
```

### K9s Skins (Themes)

View available skins:

```powershell
k9s info
```

Popular skins:

- default
- dracula
- monokai
- nord
- one-dark
- solarized-dark
- transparent

Change skin in config.yaml or press **`:skin`** then select.

### Helpful Tips

1. **Auto-refresh**: K9s refreshes automatically every 2 seconds
1. **Mouse support**: You can click on items (though keyboard is faster)
1. **Copy text**: Use your terminal’s copy function (select text)
1. **Multiple views**: Open multiple terminal windows with k9s in different views
1. **Pulses view**: `:pulses` shows real-time cluster activity

### Common Keyboard Shortcuts Summary

```
Navigation:
  Arrow keys  - Move selection
  Enter       - Describe resource
  Esc         - Go back
  
Commands:
  :           - Command mode
  /           - Filter current view
  ?           - Help
  
Actions:
  l           - Logs
  d           - Describe  
  e           - Edit
  y           - YAML
  s           - Shell/Scale
  Ctrl+D      - Delete
  
Views:
  0           - All namespaces
  Ctrl+A      - All resources
  :pods       - Pods view
  :svc        - Services view
```

### Quick Practice Exercise

Try this workflow:

1. Start k9s: `k9s`
1. View pods: `:pods`
1. Create test deployment (new PowerShell): `kubectl create deployment test --image=nginx`
1. Watch it appear in k9s (auto-refreshes)
1. Select the pod and press `l` for logs
1. Press `s` to shell into it
1. Type `exit` to leave shell
1. Press `:deploy` to see deployment
1. Delete it: select and press `Ctrl+D`, confirm with `y`

