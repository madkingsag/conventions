# MAD Kings Development Conventions

*A sane guide to Web Development*

This guide aims at defining clear guidelines for web development with the following goals in mind:

- **Reduce noice in git diff as much as possible**: When viewing a commit diff or a pull request, it should be easy to tell exactly what changed. A difference in code style can makes this very difficult.
- **Automation**: Good standards make automating the repetitive parts of development possible.
- **Prevent bugs**: Forbid bad practices use modern technologies to detect problems early.
- **Optimise for the majority, and support the minority**: If a majority of our users use Firefox and a minority uses IE11, optimise for Firefox but ensure everything works well on IE too (eg. by using polyfills).

## Technologies

- [JavaScript](JavaScript.md)
- [SQL](SQL.md)
