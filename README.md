# Go Production Checklist

This is a work-in-progress checklist for go-services to keep in mind before going to production. 
If you feel like something is missing, please open an issue or PR.

## Startup and shutdown
- [ ] Exit early (`panic`) on fatal errors (e.g. database not found or configuration invalid) 
  - [ ] Check that all configuration values are present and valid on startup
  - [ ] Ping required services (DB, 3rd party APIs, ...) on startup to verify correct configuration and valid credentials
- [ ] Migrate your database on startup
- [ ] Implement graceful-shutdown unless your gateway does that for you

## Code
- [ ] Use [Dep](https://github.com/golang/dep) to manage your dependencies (and install them on bu
- [ ] Configure your logger to output structured machine-readable logs, to be processed later (e.g. with ELK)
- [ ] Configure a timeout for all `http.Client`s (the default client has none)
- [ ] Close all `ReadClosers` (e.g. `http.Client#Do` returns a body that has to be closed manually)
- [ ] Implement health-check
- [ ] Add CORS if your services exposes an API

## Middleware
- [ ] Implement circuit breaker if needed
- [ ] Implement rate-limiting if needed 
- [ ] Set up sensible metrics (error-rate, response-time, etc.)
- [ ] Enforce body-size-limits if your gateway is not already doing this

## Preparation
- [ ] Load-test and/or benchmark the service, especially memory footprint
- [ ] Verify that your production configuration is valid (this should be the only thing different to staging, so make sure it’s correct)

## Deployment
- [ ] Use a minimal `Dockerfile` (multi-stage) if you use Docker
- [ ] Compile your service(s) w/ `-buildmode=pie` (position independent executables)
- [ ] Configure SSL so that you get an A+ on [SSL Labs](https://www.ssllabs.com/) if you use SSL
