# Oh-My-Posh Segment Reference Guide

**Complete analysis of 124 themes** in the oh-my-posh repository
**56 unique segment types** identified

---

## Quick Reference: Top Segments for DevOps Workflows

### Currently in Your Themes ✓
- **git** - Repository status (100% usage)
- **path** - Current directory (100% usage)
- **kubectl** - Kubernetes context (10% usage)
- **docker** - Docker context (2% usage)
- **executiontime** - Command duration (47% usage)
- **status** - Exit code indicator (65% usage)
- **session** - User@host (82% usage)
- **os** - OS icon (31% usage)
- **text** - Custom content (100% usage)

### Top Recommendations to Add

#### 1. **terraform** - HIGHLY RECOMMENDED
Shows Terraform workspace to prevent applying to wrong environment
```json
{
  "type": "terraform",
  "template": " \uf1bb {{ .WorkspaceName }} "
}
```

#### 2. **aws** - HIGHLY RECOMMENDED
Shows AWS profile and region to prevent wrong account deployments
```json
{
  "type": "aws",
  "template": " \ue7ad {{.Profile}}{{if .Region}}@{{.Region}}{{end}} "
}
```

#### 3. **root** - RECOMMENDED FOR SAFETY
Visual warning when running as root/admin
```json
{
  "type": "root",
  "template": " \uf292 ",
  "foreground": "#ffff00",
  "background": "#ff0000"
}
```

#### 4. **node** - RECOMMENDED
Shows Node.js version and package manager
```json
{
  "type": "node",
  "template": " \ue718 {{ .Full }} ",
  "properties": {
    "fetch_package_manager": true
  }
}
```

#### 5. **python** - RECOMMENDED
Shows Python version and active virtualenv
```json
{
  "type": "python",
  "template": " \ue235 {{ if .Venv }}{{ .Venv }} {{ end }}{{ .Full }} "
}
```

---

## Complete Segment Catalog (by Priority)

### TIER 1: DevOps Essentials

#### git (100% - 124/124 themes)
**Purpose**: Git repository status, branch, changes, stash count

**Compact (for narrow terminals)**:
```json
"template": " {{ .HEAD }}{{ if .Working.Changed }} \uf044{{ end }}{{ if .Staging.Changed }} \uf046{{ end }}"
```

**Full detail**:
```json
"template": " {{ .UpstreamIcon }}{{ .HEAD }}{{ if .BranchStatus }} {{ .BranchStatus }}{{ end }}{{ if .Working.Changed }} \uf044 {{ .Working.String }}{{ end }}{{ if .Staging.Changed }} \uf046 {{ .Staging.String }}{{ end }}{{ if gt .StashCount 0 }} \ueb4b {{ .StashCount }}{{ end }}"
```

**Properties**:
- `fetch_status: true` - Show working/staging changes
- `fetch_upstream_icon: true` - Show GitHub/GitLab/Bitbucket icons
- `branch_ahead_icon: "\u2191"` - Customize ahead indicator
- `branch_behind_icon: "\u2193"` - Customize behind indicator

---

#### kubectl (10% - 12/124 themes)
**Purpose**: Kubernetes context and namespace

**Space-efficient**:
```json
"template": " \uf308 {{.Context}}{{ if ne .Namespace \"default\" }}:{{.Namespace}}{{ end }} "
```

**Properties**:
- `parse_kubeconfig: true` - Read from kubeconfig file
- `display_error: false` - Hide errors when kubectl unavailable

---

#### docker (2% - 2/124 themes)
**Purpose**: Current Docker context

**Template**:
```json
"template": " \uf308 {{ .Context }} "
```

**Properties**:
- `display_mode: "context"` - Show active Docker context

---

#### path (100% - 124/124 themes)
**Purpose**: Current working directory

**Folder name only (space-efficient)**:
```json
"template": " \uf07b {{ .Path }} ",
"properties": {
  "style": "folder"
}
```

**Properties**:
- `style`: "folder" | "full" | "agnoster" | "agnoster_short"
- `max_depth`: Limit directory depth shown
- `folder_separator_icon`: Custom path separator
- `folder_icon`: Custom folder icon
- `home_icon`: Custom home directory icon

---

#### status (65% - 81/124 themes)
**Purpose**: Last command exit code and status

**Show only on error**:
```json
"template": "{{ if gt .Code 0 }} \uf00d {{ .Code }}{{ end }}"
```

**Always show**:
```json
"template": "{{ if eq .Code 0 }}\u2713{{ else }}\uf00d {{ .Code }}{{ end }}",
"properties": {
  "always_enabled": true
}
```

---

### TIER 2: Cloud & Container Context

#### terraform (4/124 themes)
**Purpose**: Current Terraform workspace
**Template**: ` \uf1bb {{ .WorkspaceName }} `
**Use case**: Verify workspace before apply/destroy operations

---

#### aws (11/124 themes)
**Purpose**: AWS profile and region
**Template**: ` \ue7ad {{.Profile}}{{if .Region}}@{{.Region}}{{end}} `
**Use case**: Multi-account AWS operations safety

---

#### az (9/124 themes)
**Purpose**: Azure subscription name
**Template**: ` \uebd8 {{ .Name }} `
**Properties**: `source: "cli"` or `"pwsh"`

---

#### gcp (5/124 themes)
**Purpose**: Google Cloud project
**Template**: ` \ue7b2 {{ .Project }} `

---

#### firebase (2/124 themes)
**Purpose**: Firebase project name
**Template**: ` \udb82\udd67 {{ .Project }} `

---

#### talosctl (1/124 themes)
**Purpose**: Talos Linux context
**Template**: ` \udb84\udcfe {{.Context}} `

---

### TIER 3: Productivity & System Info

#### executiontime (47/124 themes)
**Purpose**: Command execution duration

**With threshold (show only slow commands)**:
```json
"template": " \uf252 {{ .FormattedMs }} ",
"properties": {
  "threshold": 500,
  "style": "roundrock"
}
```

**Styles**: "austin" | "roundrock" | "dallas" | "galveston"

---

#### time (65/124 themes)
**Purpose**: Current time/date
**Template**: ` \uebaa {{ .CurrentDate | date "15:04:05" }} `

---

#### session (82/124 themes)
**Purpose**: Username@hostname

**With SSH indicator**:
```json
"template": " {{ if .SSHSession }}\ueba9 {{ end }}{{ .UserName }}@{{ .HostName }} "
```

---

#### os (31/124 themes)
**Purpose**: Operating system icon

**Template**:
```json
"template": " {{ if .WSL }}WSL at {{ end }}{{.Icon}} ",
"properties": {
  "linux": "\ue712",
  "macos": "\ue711",
  "windows": "\ue70f"
}
```

---

#### shell (17/124 themes)
**Purpose**: Current shell name
**Template**: ` \uf120 {{ .Name }} `

---

#### root (66/124 themes)
**Purpose**: Root/admin privilege indicator
**Template**: ` \uf292 ` (often with warning colors like yellow on red)

---

#### battery (23/124 themes)
**Purpose**: Battery level and charging status

**Template**:
```json
"template": " {{ .Icon }}{{ .Percentage }} ",
"properties": {
  "charged_icon": "\uf240 ",
  "charging_icon": "\uf1e6 ",
  "discharging_icon": "\ue234 "
}
```

---

#### sysinfo (12/124 themes)
**Purpose**: System memory/CPU usage
**Template**: ` \ue266 {{ round .PhysicalPercentUsed .Precision }}% `

---

### TIER 4: Language & Runtime Versions

#### node (37/124 themes)
**Purpose**: Node.js version and package manager

**Template**:
```json
"template": " \ue718 {{ if .PackageManagerIcon }}{{ .PackageManagerIcon }} {{ end }}{{ .Full }} ",
"properties": {
  "fetch_package_manager": true,
  "npm_icon": " \ue5fa ",
  "yarn_icon": " \ue6a7 ",
  "pnpm_icon": " \ue73a "
}
```

---

#### python (40/124 themes)
**Purpose**: Python version and virtualenv
**Template**: ` \ue235 {{ if .Venv }}{{ .Venv }} {{ end }}{{ .Full }} `

---

#### go (18/124 themes)
**Purpose**: Go version
**Template**: ` \ue626 {{ .Full }} `

---

#### rust (6/124 themes)
**Purpose**: Rust version
**Template**: ` \ue7a8 {{ .Full }} `

---

#### java (17/124 themes)
**Purpose**: Java version
**Template**: ` \ue738 {{ .Full }} `

---

#### dotnet (10/124 themes)
**Purpose**: .NET version
**Template**: ` \ue77f {{ .Full }} `

---

#### ruby (9/124 themes)
**Purpose**: Ruby version
**Template**: ` \ue791 {{ .Full }} `

---

#### php (4/124 themes)
**Purpose**: PHP version
**Template**: ` \ue73d {{ .Full }} `

---

#### lua (4/124 themes)
**Purpose**: Lua version
**Template**: ` \ue620 {{ .Full }} `

---

#### dart (9/124 themes)
**Purpose**: Dart/Flutter version
**Template**: ` \ue798 {{ .Full }} `

---

#### julia (6/124 themes)
**Purpose**: Julia version
**Template**: ` \ue624 {{ .Full }} `

---

#### kotlin (2/124 themes)
**Purpose**: Kotlin version
**Template**: `K {{ .Full }}`

---

#### haskell (2/124 themes)
**Purpose**: Haskell version
**Template**: ` \ue61f {{ .Full }} `

---

#### swift (2/124 themes)
**Purpose**: Swift version
**Template**: ` \ue755 {{ .Full }} `

---

#### crystal (3/124 themes)
**Purpose**: Crystal version
**Template**: ` \ue62f {{ .Full }} `

---

#### perl (2/124 themes)
**Purpose**: Perl version
**Template**: ` \ue769 {{ .Full }} `

---

#### r (2/124 themes)
**Purpose**: R version
**Template**: `R {{ .Full }}`

---

### TIER 5: Framework & Build Tools

#### angular (6/124 themes)
**Purpose**: Angular CLI version
**Template**: ` \ue753 {{ .Full }} `

---

#### nx (3/124 themes)
**Purpose**: Nx monorepo version
**Template**: `Nx {{ .Full }}`

---

#### aurelia (3/124 themes)
**Purpose**: Aurelia framework version
**Template**: ` \u03b1 {{ .Full }} `

---

#### flutter (2/124 themes)
**Purpose**: Flutter version
**Template**: ` \ue28e {{ .Full }} `

---

#### cmake (2/124 themes)
**Purpose**: CMake version
**Template**: `cmake {{ .Full }}`

---

#### npm (2/124 themes)
**Purpose**: npm package version from package.json
**Template**: Auto-detected from package.json

---

### TIER 6: Specialized & Niche Segments

#### project (2/124 themes)
**Purpose**: Project name from package.json, Cargo.toml, etc.
**Template**: ` \uf487 {{ .Name }} `

---

#### wakatime (1/124 themes)
**Purpose**: Coding time tracking

**Template**:
```json
"template": " \uFA19 {{ secondsRound .CumulativeTotal.Seconds }} ",
"properties": {
  "url": "https://wakatime.com/api/v1/users/current/summaries?start=today&end=today&api_key={{ .Env.WAKATIME_API_KEY }}"
}
```

**Requires**: WAKATIME_API_KEY environment variable

---

#### spotify (4/124 themes)
**Purpose**: Currently playing Spotify track
**Template**: ` \uf1bc {{ .Artist }} ~ {{ .Track }} `

---

#### strava (1/124 themes)
**Purpose**: Strava fitness data
**Requires**: API integration

---

#### ytm (1/124 themes)
**Purpose**: YouTube Music playback
**Requires**: API integration

---

#### azfunc (3/124 themes)
**Purpose**: Azure Functions core tools version
**Template**: ` \uf104\uf0e7\uf105 {{ .Full }} `

---

#### azd (1/124 themes)
**Purpose**: Azure Developer CLI environment
**Template**: ` \uebd8 {{ .DefaultEnvironment }} `

---

#### cf (2/124 themes)
**Purpose**: Cloud Foundry version
**Template**: ` \uf40a cf {{ .Full }} `

---

#### cftarget (2/124 themes)
**Purpose**: Cloud Foundry org/space
**Template**: ` \uf40a {{ .Org }}/{{ .Space }} `

---

#### cds (2/124 themes)
**Purpose**: SAP Cloud Application Programming version
**Template**: ` \ue311 cds {{ .Full }} `

---

### TIER 7: Utility Segments

#### text (100% - 124/124 themes)
**Purpose**: Static text, icons, separators, conditional content

**Examples**:
- Prompt symbol: `\u276f` (❯)
- Decorative box: `\u256d\u2500` (┌─)
- Environment variable: `{{ .Env.VARIABLE_NAME }}`
- Conditional: `{{ if .Env.DEBUG }}DEBUG {{ end }}`

---

## Optimization for Narrow Tmux Panes (60-80 columns)

### Strategy 1: Multi-Line Layout (Recommended)

**Line 1** - Context (cloud, container):
```
kubectl | docker | aws/az | terraform
```

**Line 2** - Working directory:
```
path (folder style) | git status
```

**Line 3** - Input prompt:
```
status indicator | prompt symbol
```

**Width**: ~40-50 chars per line

---

### Strategy 2: Conditional Display

Only show segments when relevant:
```json
{
  "type": "kubectl",
  "template": "{{ if .Context }} \uf308 {{.Context}}{{ end }}"
}
```

---

### Strategy 3: Right-Aligned Segments (rprompt)

Move less critical info to the right side:
```json
{
  "type": "rprompt",
  "alignment": "right",
  "segments": [
    { "type": "time" },
    { "type": "executiontime" },
    { "type": "battery" }
  ]
}
```

---

### Strategy 4: Compact Templates

- **Path**: Use `style: "folder"` (basename only, not full path)
- **Git**: Skip `fetch_upstream_icon` to save space
- **Kubectl**: Omit "default" namespace with conditional
- **All segments**: Use single-character icons, not text labels

---

### Strategy 5: Threshold-Based Display

Only show when needed:
```json
{
  "type": "executiontime",
  "template": " {{ .FormattedMs }} ",
  "properties": {
    "threshold": 1000
  }
}
```
Shows only if command took >1 second

---

## Complete Segment Type List (56 types)

### By Category

**Version Control (1)**:
git

**Container/Orchestration (2)**:
docker, kubectl

**Cloud Platforms (9)**:
aws, az, azd, azfunc, cds, cf, cftarget, firebase, gcp

**Infrastructure (2)**:
talosctl, terraform

**Languages (21)**:
angular, aurelia, crystal, dart, dotnet, flutter, go, haskell, java, julia, kotlin, lua, node, nx, perl, php, python, r, ruby, rust, swift

**Build/Package (2)**:
cmake, npm

**System Info (7)**:
battery, os, root, session, shell, sysinfo, time

**Productivity (3)**:
executiontime, project, status

**Entertainment (4)**:
spotify, strava, wakatime, ytm

**Utility (2)**:
text, prompt/rprompt (structural)

---

## Adding Segments to Your Themes

### Example: Adding Terraform to blueish-flow

Insert after kubectl segment in themes/blueish-flow.omp.json:

```json
{
  "type": "terraform",
  "style": "powerline",
  "powerline_symbol": "\ue0b0",
  "foreground": "#ffffff",
  "background": "#5F43E9",
  "template": " \uf1bb {{ .WorkspaceName }} "
}
```

### Example: Adding Python to purple-container

Insert after kubectl segment in themes/purple-container.omp.json:

```json
{
  "type": "python",
  "style": "powerline",
  "powerline_symbol": "\ue0b0",
  "foreground": "#240046",
  "background": "#E0AAFF",
  "template": " \ue235 {{ if .Venv }}{{ .Venv }} {{ end }}{{ .Full }} "
}
```

### Example: Adding Root Indicator

Insert after session segment in both themes:

**blueish-flow**:
```json
{
  "type": "root",
  "style": "powerline",
  "powerline_symbol": "\ue0b0",
  "foreground": "#193549",
  "background": "#ffff66",
  "template": " \uf292 "
}
```

**purple-container**:
```json
{
  "type": "root",
  "style": "powerline",
  "powerline_symbol": "\ue0b0",
  "foreground": "#240046",
  "background": "#FFD60A",
  "template": " \uf292 "
}
```

---

## Summary

- **Total segments**: 56 unique types across 124 themes
- **Your current setup**: 9 segment types covering essentials
- **Top additions**: terraform, aws, root, node/python (if applicable)
- **Space budget**: Room for 1-2 more segments in 70-column pane

For more details on any segment, check the official documentation at:
https://ohmyposh.dev/docs/segments
