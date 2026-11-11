# Perspective Illusion Skill

此仓库单独保存 [dotslashsimran/simcraft](https://github.com/dotslashsimran/simcraft) 的 `perspective-illusion` skill，用于制作从平面画面展开为三维透视的循环视频。

- Skill 入口：[SKILL.md](SKILL.md)
- 原始目录：[skills/video/perspective-illusion](https://github.com/dotslashsimran/simcraft/tree/58224d131814572224537fb516b8c5d33dd5fa67/skills/video/perspective-illusion)
- 来源提交：`58224d131814572224537fb516b8c5d33dd5fa67`
- 保存日期：2026-11-11（Asia/Shanghai）
- 来源文件及校验值：[UPSTREAM.json](UPSTREAM.json)

原目录的 23 个文件保持原始内容和结构，包括 5 个场景模板、3 个渲染脚本及 12 张 PNG 素材。本仓库还包含原项目的 [LICENSE](LICENSE)，以及原 skill 引用的 [motion_check.py](scripts/motion_check.py)。该脚本从同一提交的 `skills/video/vox-explainer/scripts/motion_check.py` 原样复制。

这是独立快照，不会自动同步上游。新增的 README 和来源记录用于说明封装方式。

## 独立使用

原始 `SKILL.md` 保留了 `~/simcraft/...` 路径。使用本仓库时，请采用下面的路径。模板位于本仓库的 `templates/`；两个检查脚本均位于本仓库的 `scripts/`。

需要 Node.js、npm、Python 3 和 FFmpeg。下面的命令会安装上游指定版本的 Three.js、Playwright、Chromium，以及 Python 检查依赖。

```bash
git clone https://github.com/guangyuniu8023-arch/perspective-illusion.git
cd perspective-illusion
SKILL_DIR="$(pwd)"

mkdir -p ~/Movies/edits/perspective-illusion/stills
cd ~/Movies/edits/perspective-illusion
printf '{"type":"module"}\n' > package.json
npm install three@0.170.0 playwright@1.60.0
npx playwright install chromium
python3 -m venv .venv
.venv/bin/python -m pip install numpy Pillow
cp -R "$SKILL_DIR/templates/." .
cp scramble_night.html scene.html
```

编辑 `scene.html` 后，先检查静帧和预览，再渲染完整视频：

```bash
node render.mjs scene.html x --stills=0.3,1.5,3,6,8.5
node render_fast.mjs scene.html preview.mp4 --preview
node render_fast.mjs scene.html final.mp4
.venv/bin/python "$SKILL_DIR/scripts/motion_check.py" final.mp4
.venv/bin/python "$SKILL_DIR/scripts/blink_map.py" "${FFMPEG:-ffmpeg}" final.mp4 blink.png
```

FFmpeg 需要位于 PATH 中；否则，将 `FFMPEG` 设置为其可执行文件的完整路径。

上游渲染脚本使用 Chromium 的 `--use-angle=metal` 参数。本快照保留该设置。跨平台兼容性尚未验证。

本次封装核对了原始文件校验值并检查脚本语法，未执行视频渲染。

## 许可证与署名

原项目采用 MIT 许可证。完整许可证及原始署名 `Copyright (c) 2026 Simran Sachdeva` 见 [LICENSE](LICENSE)。本仓库保留原作者署名，不主张原创。
