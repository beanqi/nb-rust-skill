# NB Rust Skill for Codex

`nb-rust` 是一个给 Codex 使用的 Rust 开发 skill，适合：

- Rust 编码
- Code review
- 性能优化
- 小 diff、直接落地的实现工作流

这个仓库已经包含 Codex 识别 skill 所需的文件：

- `SKILL.md`
- `agents/openai.yaml`

## 安装方式

按 `karpathy-codex` 的组织方式，建议把你自己的所有 skill 仓库统一放在一个目录下：

- macOS / Linux: `~/codex-skills`
- Windows: `%USERPROFILE%\codex-skills`

然后再把其中的具体 skill 链接到 Codex 的 skills 目录。

## macOS / Linux

```bash
mkdir -p ~/codex-skills
cd ~/codex-skills
git clone git@github.com:beanqi/nb-rust-skill.git nb-rust

mkdir -p ~/.codex/skills
ln -s ~/codex-skills/nb-rust ~/.codex/skills/nb-rust
```

## Windows

建议使用 PowerShell：

```powershell
New-Item -ItemType Directory -Force -Path "$HOME\codex-skills" | Out-Null
Set-Location "$HOME\codex-skills"
git clone git@github.com:beanqi/nb-rust-skill.git nb-rust

New-Item -ItemType Directory -Force -Path "$HOME\.codex\skills" | Out-Null
New-Item -ItemType SymbolicLink -Path "$HOME\.codex\skills\nb-rust" -Target "$HOME\codex-skills\nb-rust"
```

如果 Windows 下创建符号链接失败，通常是因为没有管理员权限，或者没有开启 Developer Mode。

## 安装后使用

安装完成后，重启一次 Codex app。

然后在新对话里直接输入：

```text
$nb-rust 帮我 review 这个 Rust 模块的性能瓶颈
```

如果能看到 `NB Rust` 这个 skill，说明安装成功。

## 更新

因为 Codex 读取的是链接后的目录，后续只需要更新 `~/codex-skills/nb-rust` 这个仓库即可：

```bash
cd ~/codex-skills/nb-rust
git pull
```

Windows 下同理，在 PowerShell 里进入：

```powershell
Set-Location "$HOME\codex-skills\nb-rust"
git pull
```
