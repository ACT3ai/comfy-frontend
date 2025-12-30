/Users/bryan/BGit/act3/WebApp/comfy-frontend/Architecture.md

# ComfyUI Frontend - Architecture Documentation

## 1. Executive Summary

**ComfyUI Frontend** is the official web-based user interface implementation for ComfyUI, a powerful node-based generative AI pipeline system. It is an open-source project (GPL-3.0-only licensed) that provides a complete browser-based editor for creating and executing AI workflows. The frontend integrates with a ComfyUI backend server to manage node execution, model management, and result visualization.

**Key Capabilities:**
- Visual node graph editor for building AI workflows
- Workflow execution and real-time monitoring
- Asset management (models, images, videos)
- User-friendly interface for complex AI pipelines
- Integration with ACT3 AI's video generation pipeline

---

## 2. Purpose and Role in ACT3 AI WebApp

### Primary Purpose
ComfyUI Frontend serves as the node-based workflow editor for the ACT3 AI filmmaking platform. It enables users to:

- Build complex AI video generation pipelines visually
- Chain multiple AI models together (Veo3, Runway, FLUX, etc.)
- Create reusable workflow templates for cinematography tasks
- Execute and monitor rendering jobs in real-time

### Integration with ACT3 Ecosystem

The comfy-frontend integrates with the broader ACT3 AI ecosystem:

1. **AI Video Generation**: Connects to AI model wrappers for generating video shots
2. **Backend Integration**: Communicates with ComfyUI backend server via REST and WebSocket APIs
3. **Shared Architecture**: Part of modular webapp structure alongside:
   - `~/BGit/act3/WebApp/ai-wrapper` - AI model wrappers
   - `~/BGit/act3/WebApp/server-utils` - Backend utilities
   - `~/BGit/act3/WebApp/comfyui-app` - ComfyUI application server

4. **Workflow-Centric Approach**:
   - Aligns with ACT3's focus on cinematography and shot management
   - Node-based pipeline for AI video generation
   - Integration points for veo3, Runway, FLUX models

---

## 3. Technology Stack

### Core Framework
- **Vue 3** with TypeScript (Composition API)
- **Vite** as the build tool and dev server
- **TailwindCSS** for styling
- **PrimeVue 4.2.5** for UI components

### State Management & Data
- **Pinia 2.1.7** for reactive state management (28 store modules)
- **Zod 3.23.8** for runtime schema validation
- **axios** for HTTP requests

### Graph & Canvas
- **@comfyorg/litegraph** (v0.11.0-0) - Custom fork of litegraph.js for the node graph canvas
- Handles all node rendering, connections, and canvas interactions

### Internationalization
- **vue-i18n 9.14.3** with native translation support
- Supports 6 languages: English, Chinese (Simplified), Russian, Japanese, Korean, French

### Additional Libraries
- **@vueuse/core** - Vue composition utilities
- **fuse.js** - Fuzzy search for nodes
- **algoliasearch** - Search functionality
- **jsondiffpatch** - Workflow diffing and comparison
- **@tiptap** - Rich text editing for notes
- **xterm** - Terminal emulation for logs
- **three.js** - 3D visualization support
- **Playwright** - Browser automation testing
- **Vitest** - Unit and component testing

---

## 4. Directory Structure

```
comfy-frontend/
├── src/                              # Main source code
│   ├── components/                   # Vue components
│   │   ├── graph/                   # Graph canvas and visualization
│   │   ├── sidebar/                 # Left sidebar panels
│   │   ├── bottomPanel/             # Bottom terminal/logs panel
│   │   ├── topbar/                  # Top navigation bar
│   │   ├── actionbar/               # Action buttons bar
│   │   ├── dialog/                  # Modal dialogs
│   │   ├── common/                  # Shared UI components (25 files)
│   │   ├── searchbox/               # Node search interface
│   │   ├── node/                    # Node-specific components
│   │   ├── load3d/                  # 3D model loader
│   │   ├── toast/                   # Toast notifications
│   │   └── templates/               # Template components
│   ├── composables/                 # Vue composition functions (20+)
│   │   ├── node/                    # Node-related composables
│   │   ├── bottomPanelTabs/         # Bottom panel tab logic
│   │   ├── sidebarTabs/             # Sidebar tab logic
│   │   └── widgets/                 # Widget management
│   ├── stores/                       # Pinia state stores (28 files)
│   │   ├── workspace/               # Workspace-specific stores
│   │   ├── workflowStore.ts         # Workflow management
│   │   ├── executionStore.ts        # Execution tracking
│   │   ├── nodeDefStore.ts          # Node definitions
│   │   ├── queueStore.ts            # Task queue management
│   │   ├── settingStore.ts          # User settings
│   │   └── keybindingStore.ts       # Keyboard shortcuts (200+)
│   ├── services/                     # Business logic services
│   │   ├── litegraphService.ts      # Litegraph integration
│   │   ├── workflowService.ts       # Workflow operations
│   │   ├── extensionService.ts      # Extension management
│   │   └── keybindingService.ts     # Keybinding management
│   ├── scripts/                      # Core application logic
│   │   ├── app.ts                   # Main ComfyApp class (1500+ lines)
│   │   ├── api.ts                   # API client (700+ lines)
│   │   ├── ui.ts                    # UI utilities
│   │   ├── widgets.ts               # Widget factory
│   │   └── pnginfo.ts               # PNG metadata handling
│   ├── schemas/                      # Zod validation schemas
│   │   ├── comfyWorkflowSchema.ts   # Workflow JSON schema
│   │   ├── apiSchema.ts             # API response schemas
│   │   └── nodeDefSchema.ts         # Node definition schemas (V1 & V2)
│   ├── types/                        # TypeScript type definitions (12+ files)
│   ├── utils/                        # Utility functions (12+ files)
│   ├── constants/                    # Constants and config
│   ├── locales/                      # i18n translations (6 languages)
│   ├── extensions/core/              # Core extensions
│   │   ├── maskeditor.ts            # Mask editing
│   │   ├── noteNode.ts              # Note nodes
│   │   └── load3d.ts                # 3D model loading (1000+ lines)
│   ├── views/                        # Page-level components
│   ├── main.ts                       # Vue app entry point
│   └── App.vue                       # Root Vue component
├── public/                            # Static assets
├── tests-ui/                          # Unit tests
├── browser_tests/                     # Playwright E2E tests
├── comfyui_frontend_package/         # Python package distribution
└── package.json                       # Dependencies (version 1.14.5)
```

---

## 5. Key Architectural Layers

### 5.1 Presentation Layer (Components)

#### Graph Canvas Components (`src/components/graph/`)
- **GraphCanvas.vue** - Main canvas rendering container
- **NodeBadge.vue** - Node status indicators
- **NodeTooltip.vue** - Node help tooltips
- **GraphCanvasMenu.vue** - Right-click context menu
- **SelectionToolbox.vue** - Selection tools for manipulating nodes
- **TitleEditor.vue** - Inline node/group title editing
- **DomWidgets.vue** - DOM-based widget rendering

#### Sidebar Components (`src/components/sidebar/`)
- **WorkflowsSidebarTab.vue** - Workflow file browser
- **NodeLibrarySidebarTab.vue** - Node palette with search
- **QueueSidebarTab.vue** - Queue and history viewer
- **ModelLibrarySidebarTab.vue** - Model management

#### Bottom Panel (`src/components/bottomPanel/`)
- **BottomPanel.vue** - Terminal and logs viewer
- **BaseTerminal.vue**, **CommandTerminal.vue** - Terminal interfaces
- **LogsTerminal.vue** - Server log streaming

### 5.2 State Management Layer (Pinia Stores)

#### Core Stores:

**1. workflowStore.ts** - Manages workflow data
- `ComfyWorkflow` class wraps workflow files with change tracking
- `ChangeTracker` monitors modifications
- Methods: load, save, unload, modify

**2. executionStore.ts** - Tracks execution state
- Active execution status
- Progress tracking per node
- Queue management
- WebSocket event handlers for execution updates

**3. nodeDefStore.ts** - Node definition registry
- `ComfyNodeDefImpl` class wraps node metadata
- Manages V1 and V2 node definition schemas
- Fuzzy search functionality via `NodeSearchService`

**4. queueStore.ts** - Task queue and history
- Pending and running tasks
- Result items and media
- Task filtering and sorting

**5. settingStore.ts** - User settings persistence
- Keybindings
- Color palettes
- Workflow preferences
- Server configuration

**6. keybindingStore.ts** - Keyboard shortcuts
- 200+ built-in keybindings
- Customizable shortcuts
- Conflict detection

**7. Workspace Stores** (`src/stores/workspace/`):
- **sidebarTabStore.ts** - Sidebar panel state
- **bottomPanelStore.ts** - Bottom panel state
- **colorPaletteStore.ts** - UI theme colors

### 5.3 Service Layer

#### Key Services (`src/services/`):

**1. litegraphService.ts** (450+ lines)
- Registers node definitions with litegraph
- Handles widget construction
- Input/output slot management
- Node event listeners
- Canvas rendering extensions

**2. workflowService.ts** (300+ lines)
- Save/load/export workflows
- File picker integration
- Import from PNG metadata
- Workflow persistence

**3. extensionService.ts** (150+ lines)
- Extension loading and registration
- Sidebar/bottom panel registration
- Setting registration
- Command registration

**4. nodeSearchService.ts**
- Fuzzy search through node library
- Scoring and filtering
- Algolia integration

**5. dialogService.ts**
- Modal dialogs (confirm, prompt, alert)
- Compatible with Electron

### 5.4 Core Application Logic

#### app.ts (1500+ lines) - The `ComfyApp` class

Central orchestrator for the entire application:

```typescript
class ComfyApp {
  canvas: LGraphCanvas                  // The litegraph canvas
  graph: LGraph                         // The node graph
  extensionManager: ExtensionManager    // Extension lifecycle

  // Core methods
  graphToPrompt(): Promise<PromptData>  // Convert graph to execution payload
  queuePrompt(): Promise<any>           // Send workflow to backend
  loadGraphData(data): void             // Load workflow JSON
  configure(config): Promise<void>      // Initialize with server config
  setupNodeDefs(defs): void             // Register node definitions
}
```

**Responsibilities:**
- Manages the litegraph canvas and node graph
- Handles workflow conversion to execution prompts
- Manages extension lifecycle
- Processes user input and commands

#### api.ts (700+ lines) - The API client

```typescript
class API {
  // WebSocket connection management
  // REST API calls to backend
  // Type-safe event handling

  fetchApi(route, options): Promise<Response>
  queuePrompt(number, workflow): Promise<any>
  getExtensions(): Promise<string[]>
  getEmbeddings(): Promise<string[]>
}
```

**Event Types** (discriminated unions):
- **Backend Events**: progress, executing, executed, status, execution_start, logs
- **Frontend Events**: graphChanged, promptQueued, graphCleared

**Features:**
- WebSocket reconnection logic with exponential backoff
- Type-safe event handling
- Error recovery

#### widgets.ts - Widget factory
- Creates input widgets from node definitions
- Connects widgets to node execution parameters

#### pnginfo.ts - Metadata extraction
- Extracts workflow from PNG files
- Handles A1111 format conversion
- Supports FLAC, WebP, GLTF metadata

---

## 6. Key Classes and Their Responsibilities

### ComfyWorkflow (workflowStore.ts)
```typescript
class ComfyWorkflow extends UserFile {
  changeTracker: ChangeTracker          // Non-reactive change tracker
  activeState: ComfyWorkflowJSON        // Current workflow state
  initialState: ComfyWorkflowJSON       // Original workflow state

  async load(force?: boolean)           // Load from remote storage
  async save()                          // Save to remote storage
  unload()                              // Cleanup
}
```

**Purpose**: Wraps workflow files with change tracking and persistence

### ComfyNodeDefImpl (nodeDefStore.ts)
```typescript
class ComfyNodeDefImpl implements ComfyNodeDefV1, ComfyNodeDefV2 {
  name: string                          // Node name/type
  display_name: string                  // User-friendly name
  category: string                      // Node category
  inputs: Record<string, InputSpecV2>   // Input specifications
  outputs: OutputSpecV2[]               // Output specifications
  nodeSource: NodeSource                // Custom or built-in

  // Backwards compatible with V1 schema
  input: ComfyInputSpecV1
  output: ComfyOutputSpecV1
}
```

**Purpose**: Provides unified interface for node definitions across schema versions

### ChangeTracker (changeTracker.ts)
```typescript
class ChangeTracker {
  // Tracks differences between initial and active states
  // Provides undo/redo capabilities
  // Detects unsaved changes
}
```

**Purpose**: Non-reactive change tracking for workflow modifications

---

## 7. Data Flow and Integration Patterns

### Workflow Execution Flow

```
1. User modifies graph on canvas
   → GraphCanvas.vue updates

2. litegraphService updates node definitions

3. User clicks "Queue Prompt"
   → useCoreCommands composable

4. ComfyApp.graphToPrompt()
   → Converts graph to JSON execution payload

5. api.queuePrompt()
   → Sends to backend via REST/WebSocket

6. Backend executes nodes
   → Sends progress updates via WebSocket

7. WebSocket events (executing, progress, executed) arrive
   → executionStore updates via event handlers

8. UI re-renders with progress updates
   → Node badges show execution state

9. Results stored in queueStore
   → Displayed in sidebar queue tab
```

### Workflow Persistence Flow

```
1. User saves workflow
   → workflowService.saveWorkflow()

2. ComfyWorkflow.changeTracker serializes state

3. Sent to backend file API via HTTP POST

4. workflowStore tracks in activeWorkflows collection

5. On reload:
   → workflowService.loadWorkflow()
   → ComfyWorkflow.load() fetches from backend
   → Graph rehydrated from JSON
```

### Extension Loading Flow

```
1. Backend provides list of extensions
   → api.getExtensions()

2. extensionService.loadExtensions()
   → Fetches scripts dynamically

3. Extensions call app.registerExtension()
   → Register with ExtensionManager

4. Extensions can register:
   - Sidebar tabs
   - Bottom panel tabs
   - Commands and keybindings
   - Settings
   - Custom widgets
   - Node lifecycle hooks
```

---

## 8. Key Features and Capabilities

### Workflow Editing
- Visual node graph canvas with litegraph.js
- Node creation, connection, deletion
- Group nodes (multi-level nesting)
- Reroute nodes for wire management
- Node title/group title editing
- Copy/paste nodes
- Undo/redo via change tracker
- Search and filter nodes

### Execution Management
- Queue workflows for execution
- Real-time progress tracking
- Per-node execution status
- Cached/skipped node detection
- Batch execution
- Queue history and results

### UI Components
- Left sidebar with tabs: Workflows, Node Library, Models, Queue
- Right-click context menus
- Bottom terminal for server logs
- Integrated xterm terminal
- Toast notifications
- Modal dialogs
- Settings panel (50+ customizable options)

### File Management
- Load/save workflows (.json)
- Import from PNG metadata
- Export workflow as JSON
- Download results (images, videos, audio)
- Project file browser

### Internationalization
- 6 languages with built-in translations
- Per-component i18n support
- Language switching in settings

### 3D Support
- Three.js 3D model viewer (load3d extension)
- GLTF/WebP metadata extraction
- 3D preview integration

### Customization
- Color palette system with presets
- Keybinding customization (200+ shortcuts)
- Widget appearance settings
- Canvas rendering options

### Advanced Features
- Mask editor for image manipulation
- Note nodes for workflow documentation
- Pragmatic drag-and-drop support
- Server log streaming
- Marketplace integration (algoliasearch)
- Desktop app support (Electron)

---

## 9. Extension System and Extensibility

### Extension API

```typescript
app.registerExtension({
  name: 'MyExtension',

  // Lifecycle hooks
  init?(app): void | Promise<void>
  setup?(): void
  addedExtension?(): void

  // Registration capabilities
  commands?: ComfyCommand[]
  keybindings?: Keybinding[]
  settings?: SettingParams[]
  menuCommands?: MenuCommandGroup[]
  bottomPanelTabs?: BottomPanelExtension[]
  sidebarTabs?: SidebarTabExtension[]

  // Node customization hooks
  nodeCreated?(node): void
  beforeRegisterNodeDef?(nodeType, nodeData): void
  registerCustomNodes?(): void

  // Workflow handling hooks
  beforeRegisterCustom?(app): Promise<void>
  async loadedGraphNode?(node, app): Promise<void>
})
```

### Core Extensions (`src/extensions/core/`)
- **maskeditor.ts** - Mask editing UI
- **noteNode.ts** - Documentation nodes
- **load3d.ts** - 3D model loading (1000+ lines)
- **groupNodeManage.ts** - Group node features
- **editAttention.ts** - Attention weight editing

---

## 10. Data Validation and Type Safety

### Zod Schemas (`src/schemas/`)

**1. comfyWorkflowSchema.ts** (~400 lines)
- Validates ComfyUI workflow JSON structure
- Node definitions with inputs/outputs
- Link validation
- Group and reroute nodes

**2. apiSchema.ts**
- WebSocket message types
- Task/history response types
- Status and progress updates

**3. nodeDefSchema.ts** (V1 & V2)
- Input specification validation
- Output specification validation
- Migration functions between versions

**4. Additional Schemas**
- Color palette validation
- Keybinding validation
- Settings validation

---

## 11. Build, Deployment, and Testing

### Build Process
```bash
npm run build              # Production build with type checking
npm run build:types       # Generate TypeScript declarations
npm run zipdist           # Package for distribution
```

### Development
```bash
npm run dev               # Dev server at localhost:5173
npm run dev:electron      # Dev with Electron mock API
```

### Testing
```bash
npm run test:unit         # Vitest unit tests
npm run test:component    # Vue component tests
npm run test:browser      # Playwright E2E tests
```

### Deployment Options
- Built as Python package: `comfyui-frontend-package` on PyPI
- Distributable as compiled assets
- Supports:
  - Embedded in ComfyUI server
  - Standalone web server
  - Electron desktop application

---

## 12. Configuration Files

### vite.config.mts
- Custom comfyAPI plugin for API shims
- Proxy configuration for dev server
- Icon and component auto-loading

### tsconfig.json
- Target: ES2022
- Strict mode enabled
- Path aliases (@/... → src/)

### tailwind.config.js
- Custom theme configuration
- Component-specific overrides

### eslint.config.js
- Code quality rules
- TypeScript linting

---

## 13. Performance Considerations

### Optimization Strategies
- Lazy loading of components
- Virtual scrolling for large lists
- Memoized computations in stores
- Non-reactive change trackers (using `markRaw`)
- Efficient diff algorithms for workflows
- Tree-shaking in production builds
- Code splitting and dynamic imports

### WebSocket Optimization
- Event-driven architecture
- Discriminated unions for type safety
- Batch updates to reduce re-renders
- Debounced canvas updates

---

## 14. Security and Error Handling

### Security Measures
- Zod runtime schema validation on all API responses
- XSS prevention through Vue's built-in escaping
- CORS proxy configuration for remote assets
- Optional Sentry integration for error tracking
- GPU rendering sandboxed to canvas element

### Error Management
- `useErrorHandling()` composable for error boundaries
- Toast notifications for user-facing errors
- Console logging with log levels
- Server log streaming to bottom panel terminal
- WebSocket reconnection with exponential backoff

---

## 15. File-by-File Summary

### Core Application Files

**src/main.ts**
- Vue app initialization
- Pinia store setup
- Router configuration
- i18n setup
- App mount point

**src/App.vue**
- Root Vue component
- Top-level layout structure
- Global styles

**src/router.ts**
- Vue Router setup
- Route definitions
- Navigation guards

### Scripts Directory

**src/scripts/app.ts** (1500+ lines)
- `ComfyApp` class - main orchestrator
- Extension management
- Graph-to-prompt conversion
- Queue management

**src/scripts/api.ts** (700+ lines)
- REST API client
- WebSocket connection management
- Event type definitions
- Reconnection logic

**src/scripts/ui.ts** (500+ lines)
- DOM element helpers
- Canvas configuration
- Menu bar management
- Context menus

**src/scripts/widgets.ts**
- Widget factory functions
- Input widget creation
- Widget-to-node binding

**src/scripts/pnginfo.ts**
- PNG metadata extraction
- A1111 format conversion
- Multi-format support (FLAC, WebP, GLTF)

### Store Files

**src/stores/workflowStore.ts**
- `ComfyWorkflow` class
- Workflow loading/saving
- Change tracking

**src/stores/executionStore.ts**
- Execution state tracking
- Progress monitoring
- WebSocket event handling

**src/stores/nodeDefStore.ts**
- Node definition registry
- `ComfyNodeDefImpl` class
- Search functionality

**src/stores/queueStore.ts**
- Task queue management
- History tracking
- Result storage

**src/stores/settingStore.ts**
- User preferences
- Setting persistence
- Configuration management

**src/stores/keybindingStore.ts**
- 200+ keybindings
- Customization
- Conflict resolution

### Service Files

**src/services/litegraphService.ts** (450+ lines)
- Litegraph integration
- Node registration
- Widget construction
- Canvas rendering

**src/services/workflowService.ts** (300+ lines)
- Workflow CRUD operations
- File picker integration
- PNG import/export

**src/services/extensionService.ts** (150+ lines)
- Extension loading
- Registration management
- Lifecycle coordination

**src/services/nodeSearchService.ts**
- Fuzzy search
- Node filtering
- Algolia integration

---

## 16. Summary

The ComfyUI Frontend is a production-ready, enterprise-grade web application that provides:

- **Visual Workflow Editor** using litegraph.js canvas
- **Complete State Management** with 28 Pinia stores
- **Extensible Architecture** for custom nodes and extensions
- **Real-time Execution Monitoring** via WebSocket
- **Rich UI Components** with sidebars, dialogs, terminals
- **Multi-language Support** (6 languages)
- **Type-safe Data Validation** using Zod schemas
- **Comprehensive Testing** (unit, component, E2E)
- **Desktop Support** through Electron

**Integration with ACT3 AI:**
- Serves as the node-based workflow editor for AI video generation
- Connects to AI model wrappers (Veo3, Runway, FLUX)
- Provides visual pipeline building for cinematography
- Enables reusable workflow templates for shot generation

**Version**: 1.14.5
**License**: GPL-3.0-only
**Platform**: Web (Vue 3 + TypeScript)
