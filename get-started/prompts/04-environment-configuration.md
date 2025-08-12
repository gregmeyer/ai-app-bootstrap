# Prompt 04: Environment Configuration

## Context
You are an AI development assistant helping a user configure their development and production environments. The user has completed the architecture planning and now needs to set up proper environment management.

## Your Task
Guide the user through setting up environment-specific configurations, secrets management, and deployment environments that follow security best practices.

## Prompt Template

```
Perfect! Now let's set up your environment configuration so you can develop locally and deploy to production with confidence. Proper environment management is crucial for security and smooth development workflows.

## What We'll Configure

### 1. **Environment Variables Structure**
- **Development**: Local development settings and mock services
- **Staging**: Pre-production testing environment
- **Production**: Live application with real services
- **Testing**: Automated testing environment

### 2. **Configuration Files**
- **Environment Templates**: `.env.example` files for each environment
- **Configuration Classes**: Environment-specific settings management
- **Validation**: Ensuring required variables are present
- **Type Safety**: Strong typing for configuration values

### 3. **Secrets Management**
- **API Keys**: External service credentials
- **Database Credentials**: Connection strings and passwords
- **JWT Secrets**: Authentication token signing keys
- **Encryption Keys**: Data encryption and decryption

### 4. **Environment-Specific Settings**
- **Database Connections**: Local vs remote databases
- **External APIs**: Mock vs production endpoints
- **Logging Levels**: Debug vs production logging
- **Feature Flags**: Environment-specific feature toggles

### 5. **Security Best Practices**
- **Never commit secrets**: Using `.gitignore` and environment files
- **Secret rotation**: Regular credential updates
- **Access control**: Who can access which environments
- **Audit logging**: Tracking configuration changes

## Implementation Approach
1. **Create Environment Templates**: Base configuration files
2. **Set Up Secret Management**: Secure credential handling
3. **Configure Validation**: Ensure required settings are present
4. **Test Environments**: Verify configurations work correctly
5. **Documentation**: Clear setup instructions for team members

## Getting Started
Let me know:
- What external services will you need API keys for?
- Do you have different database requirements for each environment?
- Are there any compliance or security requirements I should consider?
- What's your preferred approach to secrets management?

I'll then create a robust environment configuration system that follows the AI App Bootstrap environment configuration guide.
```

## Expected User Response
The user should provide:
- External services requiring API keys
- Database requirements for different environments
- Compliance and security requirements
- Preferred secrets management approach

## Next Steps
After this prompt, proceed to the backend foundation prompt (05-backend-foundation.md) to start building the core backend services.

