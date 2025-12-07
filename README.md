# Spring Cloud Config Repository

This repository serves as a centralized configuration store for Spring Cloud Config Server, enabling dynamic configuration management across multiple Spring applications.

## Overview

This setup demonstrates how to use Spring Cloud Config to externalize application configuration in a distributed system. The Config Server reads configuration files from this GitHub repository and serves them to client applications at runtime.

## Architecture

```
GitHub Repository (this repo)
    ↓
Spring Config Server (reads configurations)
    ↓
Spring Client Applications (consume configurations)
```

## Configuration Files

### Current Setup

- `application-dev.properties` - Development environment configuration

### Naming Convention

Configuration files follow Spring Cloud Config naming patterns:

- `{application}.properties` - Default configuration for an application
- `{application}-{profile}.properties` - Profile-specific configuration
- `application.properties` - Shared configuration across all applications
- `application-{profile}.properties` - Shared profile-specific configuration

## How It Works

1. **Config Server** connects to this GitHub repository
2. **Client Applications** request their configuration from the Config Server
3. **Dynamic Updates** - Configurations can be refreshed without restarting applications using Spring Cloud Bus or `/actuator/refresh` endpoint

## Usage

### For Config Server

Configure your Spring Config Server `application.properties`:

```properties
spring.cloud.config.server.git.uri=https://github.com/your-username/spring-config-repo
spring.cloud.config.server.git.default-label=main
server.port=8888
```

### For Client Applications

Add dependency and configure `bootstrap.properties`:

```properties
spring.application.name=your-app-name
spring.profiles.active=dev
spring.cloud.config.uri=http://localhost:8888
```

## Testing

Access configurations via Config Server endpoints:

- `http://localhost:8888/{application}/{profile}`
- `http://localhost:8888/{application}/{profile}/{label}`

Example: `http://localhost:8888/application/dev`

## Benefits

- **Centralized Management** - All configurations in one place
- **Environment Separation** - Different configs for dev, test, prod
- **Version Control** - Track configuration changes via Git
- **Dynamic Updates** - Change configs without redeployment
- **Security** - Encrypt sensitive properties using Config Server encryption

## Future Enhancements

- Add encryption for sensitive data
- Implement multiple environment profiles (test, staging, prod)
- Add application-specific configuration files
- Integrate with Spring Cloud Bus for automatic refresh

## Notes

This is a learning and testing setup for understanding Spring Cloud Config architecture and dynamic configuration management in microservices.
