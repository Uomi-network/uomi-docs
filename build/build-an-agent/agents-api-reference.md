# Agents API Reference

### Overview

UOMI provides a set of high-level API functions that wrap the low-level WebAssembly calls, making it easy to develop agents without dealing with memory management and unsafe code. These functions are designed to be safe, efficient, and easy to use.

### Core Functions

#### Logging

**`log(message: &str)`**

Logs a message to the console for debugging purposes.

```rust
pub fn log(message: &str)
```

**Example:**

```rust
fn process_data() {
    log("Starting data processing...");
    // Process data
    log("Data processing completed");
}
```

#### Input/Output Operations

**`read_input() -> Vec<u8>`**

Reads the input data provided to the agent.

```rust
pub fn read_input() -> Vec<u8>
```

**Example:**

```rust
fn handle_request() {
    let input_data = read_input();
    let request = String::from_utf8(input_data).unwrap();
    log(&format!("Received request: {}", request));
}
```

**`save_output(data: &[u8])`**

Saves the agent's output data.

```rust
pub fn save_output(data: &[u8])
```

**Example:**

```rust
fn process_and_save() {
    let result = process_data();
    save_output(result.as_bytes());
    log("Result saved successfully");
}
```

#### File Operations

**`get_input_file_service() -> Vec<u8>`**

Retrieves the content of an input file.

```rust
pub fn get_input_file_service() -> Vec<u8>
```

**Example:**

```rust
fn analyze_file() {
    let file_content = get_input_file_service();
    log(&format!("File size: {} bytes", file_content.len()));
    // Analyze file content
}
```

**`get_cid_file_service(cid: Vec<u8>) -> Vec<u8>`**

Retrieves a file from IPFS using its CID.

```rust
pub fn get_cid_file_service(cid: Vec<u8>) -> Vec<u8>
```

**Example:**

```rust
fn fetch_ipfs_content() {
    let cid = "QmExample...".as_bytes().to_vec();
    let content = get_cid_file_service(cid);
    log(&format!("Retrieved IPFS content: {} bytes", content.len()));
}
```

#### AI Model Integration

**`prepare_request(body: &str) -> Vec<u8>`**

Prepares a request body for AI model interaction.

```rust
pub fn prepare_request(body: &str) -> Vec<u8>
```

**`call_ai_service(model: i32, content: Vec<u8>) -> Vec<u8>`**

Calls an AI model with the prepared request. `Model` can be found on the [Models page](available-ai-models.md)

```rust
pub fn call_ai_service(model: i32, content: Vec<u8>) -> Vec<u8>
```

**Example:**

```rust
fn process_with_ai() {
    // Prepare AI request
    let prompt = format!("{{\"messages\": [{\"role\":\"user\",\"content\":\"hey\"}] }}");
    
    let request = prepare_request(prompt);
    
    // Call AI model
    let response = call_ai_service(1, request);
    
    // Process response
    let result = String::from_utf8(response).unwrap();
    log(&format!("AI response: {}", result));
}
```

### Common Usage Patterns

#### Complete Agent Example

```rust
mod utils;

#[no_mangle]
pub extern "C" fn process() {
    // Log start of processing
    log("Starting agent execution");
    
    // Read input
    let input = read_input();
    let input_str = String::from_utf8(input).unwrap();
    log(&format!("Received input: {}", input_str));
    
    // Prepare AI request
     let prompt = format!("{{\"messages\": [{\"role\":\"user\",\"content\":{}] }}", input_str);
    
    // Call AI model
    let ai_response = call_ai_service(1, request);
    let result = String::from_utf8(ai_response).unwrap();
    
    // Save output
    save_output(result.as_bytes());
    log("Agent execution completed");
}
```

#### File Processing Example

```rust
mod utils;

fn process_file_content() {
    // Get file content
    let content = get_input_file_service();
    
    // Process with AI
    let request = prepare_request(format!("{{\"messages\": [{\"role\":\"user\",\"content\":{}] }}", String::from_utf8(content).unwrap()));
    
    let summary = call_ai_service(1, request);
    
    // Save results
    save_output(&summary);
}
```



### Performance Tips

1. **Minimize AI Calls**
   * Use efficient prompts
2. **Optimize Data Handling**
   * Process data in appropriate chunks
   * Avoid unnecessary copies
   * Use efficient data structures
3. **Smart Logging**
   * Remove logs for production agent
   * Avoid logging sensitive data

### Security Considerations

Remember that these functions are part of the protected API and should not be modified. They provide a safe interface to interact with the UOMI blockchain and AI capabilities.
