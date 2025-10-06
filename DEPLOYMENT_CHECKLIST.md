# Java 21 Deployment Checklist

This checklist helps ensure a smooth deployment of the notification-service after upgrading to Java 21.

## Pre-Deployment Verification

### Local Build Test
- [ ] Clone the repository
- [ ] Set Java 21 as JAVA_HOME: `export JAVA_HOME=/path/to/java-21`
- [ ] Run: `mvn clean install`
- [ ] Verify: BUILD SUCCESS

### Distribution Package
- [ ] Run: `cd service && mvn play2:dist`
- [ ] Verify: `notification-service-1.0.0-dist.zip` created
- [ ] Extract and verify contents include Java 21 compatible bytecode

### Docker Image (if using)
- [ ] Update CI/CD pipeline to use Java 21
- [ ] Build Docker image: `docker build -t notification-service:java21 .`
- [ ] Verify base image is `eclipse-temurin:21-jre-alpine`
- [ ] Test container startup

## Environment Setup

### System Requirements
- [ ] Java 21 JRE/JDK installed on target servers
- [ ] Update `JAVA_HOME` environment variable
- [ ] Update PATH to include Java 21 bin directory
- [ ] Verify: `java -version` shows 21.x.x

### Jenkins/CI Configuration
- [ ] Update Jenkins server with Java 21
- [ ] Set `JAVA21_HOME` environment variable in Jenkins
- [ ] Update build agents with Java 21
- [ ] Test build pipeline

### Container Orchestration (if applicable)
- [ ] Update Kubernetes/Docker Compose configurations
- [ ] Update base images to Java 21
- [ ] Update resource limits if needed (Java 21 may have different memory footprint)

## Deployment Steps

### 1. Backup Current Version
- [ ] Backup current application
- [ ] Backup current configuration
- [ ] Document current version number

### 2. Deploy New Version
- [ ] Stop current application
- [ ] Deploy Java 21 version
- [ ] Verify all configuration files are correct
- [ ] Start application

### 3. Verification
- [ ] Check application logs for startup errors
- [ ] Verify health endpoint: `curl http://localhost:9000/health`
- [ ] Test core API endpoints
- [ ] Monitor application metrics

### 4. Smoke Tests
- [ ] Test notification sending functionality
- [ ] Verify database connections
- [ ] Check Kafka integration (if applicable)
- [ ] Test authentication/authorization

## Post-Deployment

### Monitoring
- [ ] Monitor application logs for errors
- [ ] Check memory usage patterns
- [ ] Monitor CPU utilization
- [ ] Review garbage collection logs
- [ ] Compare performance with Java 11 baseline

### Performance Validation
- [ ] Run load tests
- [ ] Compare response times
- [ ] Monitor throughput
- [ ] Check error rates

### Rollback Plan
- [ ] Keep Java 11 version ready
- [ ] Document rollback procedure
- [ ] Test rollback in staging environment

## Known Issues & Workarounds

### PowerMock Test Failures
**Issue**: Some controller tests fail with PowerMock on Java 21

**Impact**: Does not affect application functionality - test infrastructure only

**Action Required**: None for production deployment. Consider updating tests in future iterations.

### Module System Changes
**Issue**: Java 9+ module system changes how reflection works

**Status**: ✅ Resolved - Added necessary `--add-opens` JVM arguments in test configuration

**Action Required**: None

## Success Criteria

The deployment is successful when:
- ✅ Application starts without errors
- ✅ Health endpoint returns 200 OK
- ✅ All critical APIs respond correctly
- ✅ No increase in error rates
- ✅ Performance meets or exceeds Java 11 baseline
- ✅ All integrations working (DB, Kafka, external services)

## Troubleshooting

### Application Won't Start
1. Check Java version: `java -version`
2. Verify JAVA_HOME is set correctly
3. Check application logs
4. Verify all dependencies are Java 21 compatible

### Performance Issues
1. Review GC logs
2. Check heap size configuration
3. Monitor thread usage
4. Compare with Java 11 metrics

### Integration Failures
1. Verify network connectivity
2. Check service credentials
3. Review integration logs
4. Validate configuration files

## Contacts & Support

- **Technical Lead**: [Add contact]
- **DevOps Team**: [Add contact]
- **On-Call Support**: [Add contact]

## Additional Resources

- Java 21 Release Notes: https://www.oracle.com/java/technologies/javase/21-relnotes.html
- Migration Guide: See `JAVA21_UPGRADE_NOTES.md`
- Build Documentation: See `README.md`

---

**Last Updated**: 2025-10-06
**Version**: Java 21 Migration
