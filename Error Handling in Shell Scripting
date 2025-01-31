# Error Handling in Shell Scripting

## Basic Error Handling

### Exit Codes
Shell scripts use exit codes to indicate success (0) or failure (non-zero):
```bash
command
echo $?  # Prints last command's exit code
```

### Exit on Error
Set script to exit immediately on error:
```bash
set -e  # Exit on any error
set -o errexit  # Alternative syntax
```

### Error Handling with || and &&
Use conditional operators for basic error handling:
```bash
command || echo "Command failed"
command && echo "Command succeeded"
```

## Advanced Error Handling

### Trap Command
Catch and handle signals:
```bash
trap 'echo "Error on line $LINENO"; exit 1' ERR
trap 'cleanup_function' EXIT
```

### Custom Error Function
Create reusable error handling:
```bash
error_handler() {
    local line_number=$1
    local error_code=$2
    echo "Error on line $line_number: Exit code $error_code"
    exit "$error_code"
}
trap 'error_handler ${LINENO} $?' ERR
```

### Conditional Error Handling
Check specific conditions:
```bash
if ! command; then
    echo "Error: command failed"
    exit 1
fi
```

## Best Practices

### Input Validation
```bash
validate_input() {
    if [[ -z "$1" ]]; then
        echo "Error: Missing required argument"
        exit 1
    fi
}
```

### Cleanup on Exit
```bash
cleanup() {
    rm -f "$TEMPFILE"
    kill "$BACKGROUND_PID" 2>/dev/null
}
trap cleanup EXIT
```

### Logging
```bash
log_error() {
    echo "[ERROR] $(date '+%Y-%m-%d %H:%M:%S') - $1" >&2
}
```

### Debug Mode
```bash
set -x  # Enable debug mode
set +x  # Disable debug mode

# Alternative with function
debug() {
    [[ "$DEBUG" == "true" ]] && echo "[DEBUG] $*"
}
```

## Complete Example

```bash
#!/bin/bash

set -e

# Error handling function
error_handler() {
    echo "Error on line $1: Exit code $2"
    cleanup
    exit "$2"
}

# Cleanup function
cleanup() {
    echo "Performing cleanup..."
    rm -f "$TEMPFILE"
}

# Set traps
trap 'error_handler ${LINENO} $?' ERR
trap cleanup EXIT

# Main script
TEMPFILE=$(mktemp)
echo "Working with temporary file: $TEMPFILE"

# Example operation that might fail
if ! grep "pattern" "$TEMPFILE"; then
    echo "Pattern not found"
    exit 1
fi

echo "Script completed successfully"
exit 0
```

## Common Pitfalls

1. Not handling cleanup properly
2. Ignoring exit codes from commands
3. Missing input validation
4. Not redirecting error output (stderr)
5. Forgetting to set error traps early

## Additional Considerations

### Strict Mode
Enable strict mode for safer scripts:
```bash
set -euo pipefail
IFS=$'\n\t'
```

### Error Output Redirection
```bash
command 2>/dev/null  # Silence errors
command 2>&1  # Redirect stderr to stdout
```

### Background Process Error Handling
```bash
command & pid=$!
wait $pid || echo "Background process failed"
```
