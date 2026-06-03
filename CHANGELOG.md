# Changelog

All notable changes to this project will be documented in this file.

The format is inspired by [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project currently tracks chart-level release notes for the Helm chart in this repository.

## [0.0.1] - 2026-06-03

Initial release of the `qlack-lgtm` Helm chart, which installs the QLACK LGTM observability stack (OpenTelemetry Collector, Grafana Loki, Grafana Mimir, Grafana Tempo, and Grafana).

### Added
- Bundled the following Grafana stack dependency versions:
  - `opentelemetry-collector` `0.158.0`,
  - `tempo` `2.2.0`,
  - `grafana` `12.4.1`.
- Sourced the `tempo` and `grafana` dependencies from the `oci://ghcr.io/grafana-community/helm-charts` OCI registry.
- Added a built-in optional Helm-managed Grafana admin secret template via `grafana.admin.create`, while preserving support for externally managed secrets through `grafana.admin.existingSecret`.
- Added persistent Grafana storage defaults so Grafana state, including admin password changes and dashboard state, survives pod recreation.
- Added expanded Tempo TraceQL troubleshooting coverage in the provisioned Grafana dashboard, including:
  - trace drilldown panels,
  - root-cause hint panels,
  - slow/error trace views,
  - HTTP 5xx trace views,
  - downstream/client span views,
  - exception and large-trace discovery panels.

### Changed
- Updated the provisioned `QLACK OTEL-LGTM Observability Overview` dashboard layout:
  - removed OTEL-LGTM stack pod CPU and memory panels,
  - removed runtime stat summary panels for observed pods, node CPU used, node memory used, and stack restarts in range,
  - repositioned the `Kubernetes and OTEL-LGTM stack runtime` section so it appears above `Operational notes`,
  - tightened downstream panel positioning to eliminate empty dashboard space.
- Switched Grafana admin credential handling away from inline default password values and onto secret-based configuration.
- Sanitized public chart defaults in `helm/values.yaml` for GitHub/public use:
  - removed private-registry-specific pull secret assumptions,
  - replaced cluster-specific resolver defaults with generic Kubernetes defaults,
  - disabled Grafana ingress by default,
  - replaced organization-specific ingress host values with generic examples.

### Documentation
- Reworked `README.md` to document the correct chart path (`./helm`) and recommended install/upgrade flow.
- Clarified when `helm dependency build ./helm` is needed and when vendored dependencies are sufficient.
- Updated deployment examples to use namespace `lgtm`.
- Documented both Grafana admin credential modes:
  - externally created Kubernetes secret, and
  - optional chart-managed secret creation.
- Updated README defaults and prerequisites to match the current public-safe values file.

### Operational Notes
- The chart version for this release is `0.0.1`.
- Default Grafana ingress is now disabled and must be explicitly enabled with user-provided host/TLS settings.
- Default image pull secret lists are empty; private-registry credentials must be supplied by the user when needed.

