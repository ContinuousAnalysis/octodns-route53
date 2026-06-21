# Developer Agent Guide for octoDNS Route53 Provider

This repository contains the Route53 provider for octoDNS. It enables planning, syncing, and applying DNS record states directly to AWS Route53.

> [!IMPORTANT]
> **Core Workflow and Guidelines**
>
> All agents working on this repository must read and follow the general instructions and workflow guidelines defined in the core octoDNS `AGENTS.md` file.
> - **Local check**: Look for the file at `../octodns/AGENTS.md`.
> - **Remote check**: If the local file is not available, fetch it from GitHub: [octoDNS Core AGENTS.md](https://github.com/octodns/octodns/raw/refs/heads/main/AGENTS.md).
>
> You must align your code structure, style, pull request guidelines, and overall development workflows with the instructions specified there.

## Repository & Module Information

### Key Components

- **Provider Class**: [Route53Provider](file:///home/ross/octodns/octodns-route53/octodns_route53/provider.py) (defined in [octodns_route53/provider.py](file:///home/ross/octodns/octodns-route53/octodns_route53/provider.py)). This class orchestrates communication with AWS Route53 using `boto3`.
- **Source Classes**:
  - [Ec2Source](file:///home/ross/octodns/octodns-route53/octodns_route53/source.py) (defined in [octodns_route53/source.py](file:///home/ross/octodns/octodns-route53/octodns_route53/source.py)): Dynamically fetches DNS records from running AWS EC2 instances.
  - [ElbSource](file:///home/ross/octodns/octodns-route53/octodns_route53/source.py) (defined in [octodns_route53/source.py](file:///home/ross/octodns/octodns-route53/octodns_route53/source.py)): Dynamically sources records from AWS Elastic Load Balancers (ELBs).
- **Custom Route53 Records**: Implemented in [octodns_route53/record.py](file:///home/ross/octodns/octodns-route53/octodns_route53/record.py).
  - [Route53AliasRecord](file:///home/ross/octodns/octodns-route53/octodns_route53/record.py): Supports AWS-specific ALIAS records pointing to ELBs, CloudFront distributions, S3 buckets, etc.
- **Processors**:
  - [AwsAcmMangingProcessor](file:///home/ross/octodns/octodns-route53/octodns_route53/processor/__init__.py) (defined in [octodns_route53/processor/__init__.py](file:///home/ross/octodns/octodns-route53/octodns_route53/processor/__init__.py)): Manages Route53 DNS validation records for AWS ACM (Certificate Manager) certificates.

### Key Workflows & Features

1. **Supported Record Types**: `A`, `AAAA`, `CAA`, `CNAME`, `DS`, `MX`, `NAPTR`, `NS`, `PTR`, `SPF`, `SRV`, `TXT`, and custom `ALIAS` records.
2. **Dynamic Routing Support**: Fully supports Route53 dynamic features (`SUPPORTS_DYNAMIC=True`):
   - Geolocation routing (continent, country, and subdivision matching).
   - Latency-based routing.
   - Weighted routing.
   - Failover routing (primary/secondary).
3. **Dynamic Subnets**: Supported (`SUPPORTS_DYNAMIC_SUBNETS=True`).
4. **Pool Value Status**: Supported (`SUPPORTS_POOL_VALUE_STATUS=True`).

## Development & Testing

- **Setup Script**: Run `./script/bootstrap` to create a virtual environment, install runtime and development dependencies (including `black`, `isort`, `pyflakes`, and `pytest`), and configure pre-commit hooks.
- **Test Suite**: Run unit tests using `pytest` via `./script/test` (or `pytest tests/`). Test files are located in [tests/](file:///home/ross/octodns/octodns-route53/tests).
- **Code Coverage**: Verify code coverage using `./script/coverage`.

## Key Constraints & Behaviors

- **Python Version**: Targets Python `>=3.9`.
- **Formatting**: Code formatting is enforced via `black` (version `>=26.0.0,<27.0.0`) and `isort`.
