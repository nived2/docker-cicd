# Docker CI/CD Pipeline

An end-to-end CI/CD pipeline with automated testing, security scanning, and deployment to multiple environments using Docker containers.

## Features

- Automated build and test pipeline
- Multi-environment deployment
- Security scanning with SonarQube
- Docker image optimization
- Automated testing
- Environment-specific configurations
- Deployment rollback capability

## Pipeline Stages

1. **Build**
   - Code checkout
   - Dependency installation
   - Docker image building
   - Multi-stage builds for optimization

2. **Test**
   - Unit testing
   - Integration testing
   - Code coverage analysis
   - SonarQube analysis

3. **Security Scan**
   - SonarQube security rules
   - Docker image scanning
   - Dependency vulnerability check
   - OWASP security scanning

4. **Deploy**
   - Environment validation
   - Configuration injection
   - Rolling updates
   - Health checks

## Prerequisites

- Jenkins server
- Docker
- GitLab/GitHub account
- SonarQube server
- Docker registry access

## Project Structure

```
docker-cicd/
├── Jenkinsfile              # Jenkins pipeline definition
├── docker/                  # Docker configurations
│   ├── Dockerfile          # Application Dockerfile
│   └── docker-compose.yml  # Local development setup
├── scripts/                # CI/CD scripts
│   ├── build.sh           # Build scripts
│   ├── test.sh            # Test scripts
│   └── deploy.sh          # Deployment scripts
└── config/                 # Environment configurations
    ├── dev/               # Development environment
    ├── staging/          # Staging environment
    └── prod/             # Production environment
```

## Quick Start

1. Clone the repository:
```bash
git clone https://github.com/nived2/docker-cicd.git
cd docker-cicd
```

2. Configure Jenkins:
   - Install required plugins
   - Configure Docker credentials
   - Set up SonarQube connection
   - Configure environment variables

3. Set up pipeline:
```bash
# Create Jenkins pipeline using Jenkinsfile
# Configure webhook in GitLab/GitHub
```

## Jenkins Pipeline Configuration

```groovy
pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t myapp .'
            }
        }
        
        stage('Test') {
            steps {
                sh 'docker run myapp npm test'
            }
        }
        
        stage('Security Scan') {
            steps {
                sh 'sonar-scanner'
            }
        }
        
        stage('Deploy') {
            steps {
                sh './scripts/deploy.sh'
            }
        }
    }
}
```

## Security Features

- Automated vulnerability scanning
- Secret management
- Image signing
- Access control
- Audit logging

## Monitoring

- Pipeline execution metrics
- Build success/failure rates
- Deployment statistics
- Security scan results

## Best Practices

1. Use multi-stage Docker builds
2. Implement proper versioning
3. Maintain separate environments
4. Regular security updates
5. Automated rollback procedures

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

MIT License - feel free to use this project for your own portfolio

## Contact

Nived Mahendran - nivedmahendran547@gmail.com
