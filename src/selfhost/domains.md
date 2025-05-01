# Domain Management

There are a variety of (proprietary) tools out there that you can use to manage your domains.
But proprietary sucks.

## The problem with DomainMOD

Nothing really. [DomainMOD](https://domainmod.org/) is a great tool.
However I (@luc) am a stubborn engineer and Porkbun doesn't offer "read-only" API access (only full access 🙈).

## [v3xlabs/dmn](https://github.com/v3xlabs/dmn)

`dmn` is a lightweight docker container that you can use to keep track of domain expiries.
It can be used either as a CLI tool or run as a docker container. It uses a local sqlite database to keep track of your data to ensure ease of use.
