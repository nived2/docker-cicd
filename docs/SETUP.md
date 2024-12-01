# CI/CD Pipeline Setup Guide

## Prerequisites

1. Install Jenkins:
   ```bash
   # Windows (using chocolatey)
   choco install jenkins

   # macOS
   brew install jenkins-lts
   ```

2. Install Docker:
   ```bash
   # Windows (using winget)
   winget install Docker.DockerDesktop

   # macOS
   brew install --cask docker
   ```

3. Install SonarQube:
   ```bash
   # Using Docker
   docker run -d --name sonarqube -p 9000:9000 sonarqube:latest
   ```

## Jenkins Setup

1. Start Jenkins:
   ```bash
   # Windows
   net start jenkins

   # macOS
   brew services start jenkins-lts
   ```

2. Access Jenkins:
   - URL: http://localhost:8080
   - Get initial admin password:
     ```bash
     # Windows
     type C:\ProgramData\Jenkins\.jenkins\secrets\initialAdminPassword

     # macOS
     sudo cat /var/lib/jenkins/secrets/initialAdminPassword
     ```

3. Install required plugins:
   - Docker Pipeline
   - SonarQube Scanner
   - GitLab/GitHub Integration
   - Blue Ocean

## Pipeline Configuration

1. Configure credentials:
   - Jenkins → Credentials → System → Global credentials
   - Add Docker registry credentials
   - Add GitLab/GitHub credentials
   - Add SonarQube token

2. Configure SonarQube:
   - Manage Jenkins → Configure System
   - Find "SonarQube servers"
   - Add SonarQube URL and authentication token

3. Configure Docker:
   - Ensure Jenkins user has Docker permissions
   - Test Docker connectivity:
     ```bash
     sudo usermod -aG docker jenkins
     systemctl restart jenkins
     ```

## Pipeline Usage

1. Create new pipeline:
   - New Item → Pipeline
   - Configure Git repository
   - Use provided Jenkinsfile

2. Pipeline stages:
   ```groovy
   stage('Build') {
       // Builds Docker image
   }
   stage('Test') {
       // Runs tests in container
   }
   stage('Security Scan') {
       // SonarQube analysis
   }
   stage('Deploy') {
       // Deploys to environment
   }
   ```

3. Environment variables:
   ```bash
   DOCKER_IMAGE=myapp
   DOCKER_TAG=${BUILD_NUMBER}
   SONAR_PROJECT_KEY=my-app
   ```

## Security Configuration

1. SonarQube setup:
   - Access SonarQube: http://localhost:9000
   - Default credentials: admin/admin
   - Create new project
   - Generate authentication token

2. Docker security:
   ```bash
   # Enable Docker content trust
   export DOCKER_CONTENT_TRUST=1

   # Scan images
   docker scan myapp:latest
   ```

3. Jenkins security:
   - Enable CSRF protection
   - Configure authentication
   - Set up authorization strategy

## Deployment Configuration

1. Development:
   ```bash
   ./scripts/deploy.sh development
   ```

2. Staging:
   ```bash
   ./scripts/deploy.sh staging
   ```

3. Production:
   ```bash
   ./scripts/deploy.sh production
   ```

## Monitoring

1. Jenkins monitoring:
   - Pipeline trends
   - Build statistics
   - Test results

2. SonarQube monitoring:
   - Code quality
   - Security issues
   - Technical debt

## Troubleshooting

1. Pipeline issues:
   - Check Jenkins logs
   - Verify Jenkinsfile syntax
   - Test Docker connectivity

2. Build failures:
   - Check build logs
   - Verify Docker build context
   - Check resource constraints

3. Deployment issues:
   - Verify environment variables
   - Check deployment scripts
   - Validate configurations

## Backup and Maintenance

1. Jenkins backup:
   ```bash
   # Backup Jenkins home
   tar -czf jenkins_backup.tar.gz /var/lib/jenkins
   ```

2. Docker cleanup:
   ```bash
   # Remove unused images
   docker image prune -a

   # Remove unused volumes
   docker volume prune
   ```

## Contributing

1. Fork the repository
2. Create feature branch
3. Make changes
4. Submit pull request

## Support

For issues and support:
- Create GitHub issue
- Email: nivedmahendran547@gmail.com
