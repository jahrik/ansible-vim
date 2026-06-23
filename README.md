# VIM

[![CICD](https://github.com/jahrik/ansible-vim/actions/workflows/cicd.yml/badge.svg)](https://github.com/jahrik/ansible-vim/actions/workflows/cicd.yml)
[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-jahrik.vim-blue?logo=ansible)](https://galaxy.ansible.com/ui/standalone/roles/jahrik/vim/)

Installs [Vim](https://www.vim.org/) with the [Vundle](https://github.com/VundleVim/Vundle.vim) plugin manager and deploys a fully configured `~/.vimrc` from a Jinja2 template. Plugins, key mappings, set options, and colorscheme are all controlled via role variables.

## OS Support

| OS Family | Version |
|---|---|
| Arch Linux | all |
| Ubuntu | 24.04 (Noble), 22.04 (Jammy), 20.04 (Focal) |

## Role Variables

| Variable | Default | Description |
|---|---|---|
| `install` | `true` | Set to `false` to uninstall Vim and back up config to `/tmp/vim` |
| `vundle` | (list) | [Vundle](https://github.com/VundleVim/Vundle.vim) plugins to install |
| `colorscheme` | `slate` | Vim colorscheme |
| `set` | (list) | Vim `:set` options |
| `map` | (list) | Key mappings |
| `nmap` | (list) | Normal-mode key mappings |
| `let` | (list) | Vim `let` variable assignments |

Default plugins include: NERDTree, vim-syntastic, surround.vim, vim-markdown, Dockerfile.vim, vim-terraform, ansible-vim, powerline, and more. See `defaults/main.yml` for the full list.

## Example Playbook

```yaml
---
- hosts: all
  roles:
    - role: jahrik.vim
```

Custom colorscheme:

```yaml
---
- hosts: all
  vars:
    colorscheme:
      - desert
  roles:
    - role: jahrik.vim
```

To uninstall:

```yaml
---
- hosts: all
  vars:
    install: false
  roles:
    - role: jahrik.vim
```

## Testing

```bash
uv sync
source .venv/bin/activate
yamllint .
ansible-lint
molecule test
```

## License

MIT
