https://github.com/microsoft/node-jsonc-parser/blob/e82ebd90d91f026521d88d11c9749e2f02b9a79f/src/impl/parser.ts#L386

The json LSP inside your vscode uses this library internally. For example, if you make some syntactic errors right now, the error message that you see will have come from this code base.

This parser builds up the json syntax tree based on the output of the tokenizer.
