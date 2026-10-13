# Changelog

All notable changes to this project are documented here.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project uses [Semantic Versioning](https://semver.org/).

## [1.0.0] - 2026-10-12

### Added
- Apple Silicon (MPS) support, including FP16, attention slicing, VAE tiling, cache cleanup and memory reporting.
- Gradio 6 compatibility (theme passed to `launch()`).
- Project documentation: README, LICENSE, CONTRIBUTING, CODE_OF_CONDUCT, SECURITY.
- GitHub issue templates, pull request template and CI workflow.

### Changed
- `xformers` is now optional and no longer listed in `requirements.txt` (it does not install on macOS).

### Removed
- Unused `dulwich` dependency.
- `push_upstream.sh`, a force-push helper that is not part of the application.

## [0.1.0] - 2026-06-23

### Added
- Initial SDXL + ControlNet Canny pipeline with a Gradio dashboard (CUDA and CPU).
