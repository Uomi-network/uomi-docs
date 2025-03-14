# Installing WASP

WASP (WebAssembly Agent System Platform) is available on GitHub on [GitHub official WASP repo](https://github.com/Uomi-network/WASP). This guide will walk you through the installation process and get you started with WASP development.

### Prerequisites

Before installing WASP, ensure your system meets the following requirements:

1. **Rust**
   * Latest stable version of Rust
   * Install from [https://rustup.rs/](https://rustup.rs/)
2. **Node.js**
   * Version 14 or higher
   * Download from [https://nodejs.org/](https://nodejs.org/)
3.  **WebAssembly Target**

    * Required for compiling Rust to WebAssembly
    * Install using rustup:

    ```bash
    rustup target add wasm32-unknown-unknown
    ```

### Installation Methods

You have two options for installing and setting up WASP:

#### Option 1: Quick Start with NPX (Recommended)

This is the fastest way to get started with a new WASP project:

```bash
# Create a new UOMI agent project
npx wasp create
```

This command will:

* Create a new project directory
* Set up the required project structure
* Install necessary dependencies
* Configure the development environment

#### Option 2: Manual Setup

If you prefer more control over the setup process, you can manually clone and configure the project:

1. Clone the repository:

```bash
git clone https://github.com/Uomi-network/uomi-chat-agent-template.git
```

2. Navigate to the agent directory:

```bash
cd uomi-chat-agent-template/agent
```

3. Install dependencies:

```bash
npm install
```

4. Make the build script executable:

```bash
chmod +x ./bin/build_and_run_host.sh
```

5. Start the development environment:

```bash
npm start
```

### Verifying Installation

To verify that WASP is installed correctly:

1. Start the development environment:

```bash
npm start
```

2. You should see the UOMI Development Environment interface:

```
UOMI Development Environment
Type your messages. Use these commands:
/clear - Clear conversation history
/history - Show conversation history
/exit - Exit the program
```

## Available AI Models

Available AI models can be found in the [Models page](available-ai-models.md), using different IDs in the call\_ai\_service call will cause the agent to crash and not work

### Next Steps

After installation, you should:

1. Configure your development environment in `uomi.config.json`
2. Set up your AI model preferences (local node-ai or third-party services)
3. Familiarize yourself with the project structure
4. Try running the example agent

### AI Service Configuration

You have two options for AI service integration:

#### Option 1: Local Node-AI Service (Recommended)

Follow the [node-ai repository setup](https://github.com/Uomi-network/uomi-node-ai) to run the production version locally. With this option, you don't need to specify URL or API keys in your configuration.

#### Option 2: Third-Party Services

If you prefer using external services like OpenAI (This method does not guarantee determinism as the model result in tests, the model result may be different from the one in production), configure your `uomi.config.json`:

```json
{
  "models": {
    "1": {
      "name": "gpt-3.5-turbo",
      "url": "https://api.openai.com/v1/chat/completions",
      "api_key": "your-api-key-here"
    }
  }
}
```

### Troubleshooting

If you encounter issues during installation:

1. **Rust Build Failures**
   * Verify your Rust installation: `rustc --version`
   * Ensure WebAssembly target is installed: `rustup target list`
2. **Node.js Issues**
   * Check Node.js version: `node --version`
   * Verify npm installation: `npm --version`
3. **Permission Issues**
   * Ensure build script is executable
   * Check filesystem permissions

### Getting Help

If you need assistance:

* Check the [GitHub repository](https://github.com/Uomi-network/WASP)
* Submit issues for bugs or questions
* Contribute via pull requests

***

WASP is an open-source project maintained by the UOMI team. For additional support or information, refer to the project documentation or reach out to the community.
