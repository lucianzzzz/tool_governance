# Tool Governance Framework

This is a practical exercise for implementing a tool governance framework with transfer functionality.

## Project Structure

- `tool_governance.py` - Main governance framework implementation
- `test_tool_governance.py` - Test cases for the transfer tool functionality

## Description

This project demonstrates a comprehensive tool governance framework that includes:
- Pydantic validation
- Permission state machine
- One-time approval
- Timeout recovery
- Result redaction
- Audit trail

The exercise focuses on integrating a "transfer" tool into the existing governance framework.

## Running Tests

To run the tests and verify the implementation:

```bash
python -m pytest test_tool_governance.py -v -k "transfer"
```

## Getting Started

1. Review the TODO comments in `tool_governance.py` for implementation tasks
2. Implement the transfer tool functionality
3. Run tests to verify your implementation
