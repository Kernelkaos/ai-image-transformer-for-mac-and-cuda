# Contributing

Thanks for your interest in improving AI Image Transformer.

## Reporting bugs and requesting features

Open an [issue](../../issues/new/choose) using one of the templates. For bugs, include your OS, Python version, device (CUDA, MPS or CPU) and the full error message.

For security problems, do **not** open a public issue. See [SECURITY.md](SECURITY.md).

## Development setup

```bash
git clone https://github.com/Kernelkaos/ai-image-transformer-for-mac-and-cuda.git
cd ai-image-transformer-for-mac-and-cuda
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
pip install ruff
```

## Making changes

1. Fork the repository and create a branch from `main`, for example `feat/short-description` or `fix/short-description`.
2. Keep each pull request focused on one change.
3. Check your code before pushing:
   ```bash
   ruff check .
   python -m compileall -q app.py src
   ```
4. Update `README.md` and `CHANGELOG.md` when behavior changes.
5. Open a pull request and fill in the template.

## Commit messages

Use [Conventional Commits](https://www.conventionalcommits.org/): `feat:`, `fix:`, `docs:`, `refactor:`, `chore:`.

## Code style

- Follow PEP 8. Ruff is the linter.
- Add docstrings to public functions and classes.
- Do not commit model weights, generated images, API tokens or `.env` files.

By contributing, you agree that your contributions are licensed under the [MIT License](LICENSE).
