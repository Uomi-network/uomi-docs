# Introduction

### WASP

WASP (WebAssembly Agent System Platform) is a comprehensive development environment created by the UOMI team for building, testing, and deploying WebAssembly agents. It provides a simulation environment that mirrors the UOMI blockchain's behavior, allowing developers to create and test agents in a controlled environment before deployment.

#### Key Features

* WebAssembly-based agent execution
* Blockchain node simulation
* Built-in debugging capabilities
* Frontend integration tools
* Hot-reloading development environment

### Architecture Overview

WASP implements a three-layer architecture:

1. **Host Layer** - Simulates blockchain node behavior
2. **Agent Layer** - Contains the WebAssembly agent code
3. **Frontend Layer** - Provides user interface and interaction

#### Communication Flow

```
Frontend (main.js) <-> Host Simulation <-> WebAssembly Agent
```

### Project Structure

```
Copywasp-project/
├── host/                 # Blockchain node simulation
├── agent-template/       # Agent development environment
|   └── src/                
│      ├── lib/             # Core agent functionality
│      ├── utils/           # Utility functions
│                
└── main.js              # User interface 
```
