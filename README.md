# z-docs

Documentation for the [ZeroicAI](https://github.com/zeroicai/zeroicai) multi-agent framework.

## Building

Install [mdbook](https://rust-lang.github.io/mdBook/):
```bash
cargo install mdbook
```

Build and serve locally:
```bash
mdbook serve --open
```

## Structure
```
src/
├── introduction.md
├── getting-started/
│   ├── installation.md
│   ├── quickstart.md
│   └── architecture.md
├── crates/
│   ├── core.md
│   ├── messaging.md
│   ├── cognition.md
│   ├── patterns.md
│   └── runtime.md
├── guides/
│   ├── examples.md
│   ├── first-agent.md
│   ├── communication.md
│   └── patterns.md
└── reference/
    ├── api.md
    └── glossary.md
```

## License

MIT OR Apache-2.0
