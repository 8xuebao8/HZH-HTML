# Contour Studio · 轮廓

A browser-based 2D curve workspace for jewelry and product design.

面向珠宝设计与产品设计的网页二维曲线工作台。

Turn product images, clean line drawings, or structured drawing code into editable curves for Rhino. Trace, inspect, set real dimensions, and export — all in your browser.

将产品照片、干净线稿或绘图代码转换为 Rhino 可编辑曲线，减少反复手工描线的工作。

## Features · 功能

| Workflow / 模式 | What it does / 用途 |
| --- | --- |
| **Outline · 轮廓模式** | Trace outer boundaries while ignoring enclosed holes and internal texture. 提取最外层边界，忽略封闭孔洞和内部纹理。 |
| **Line drawing · 全部线稿** | Trace black stroke centerlines or photographic light/dark edges. 提取黑色笔画中心线，或照片明暗边缘。 |
| **Code drawing · 代码生成** | Import JSON to create lines, polylines, circles, ellipses, rectangles and Bézier curves. 导入 JSON 直接绘制标准图形与曲线。 |

- **Millimeter dimensions:** two-point calibration; code drawings also support linked or independent width and height.
- **Editable Rhino output:** `.3dm` in Rhino 7 format, compatible with Rhino 8; open curves remain open.
- **Simple / Pro interface:** everyday controls first, advanced detection and fitting settings in the Pro sidebar.
- **English / 中文:** English on first use; language preference is remembered when browser storage is available.
- **Local processing:** no backend, account or API key required; images are processed on your device.

支持两点毫米标定、宽高联动、节点显示、缩放平移、简洁／专业模式和中英文切换。右上角提供产品简介与使用说明。

## Quick start · 使用流程

### From an image · 图片转曲线

1. Choose **Outline** or **Line drawing** and upload a JPG, PNG or WebP image, up to 25 MB.
2. Inspect the result in **Overlay** or **Lines only**. For clean black line art, use stroke centerlines; for product photographs, use light/dark edges.
3. Adjust smoothing or switch to **Pro** for detection, cleanup and fitting controls.
4. Pick two points whose distance you know, enter the actual distance in millimeters, and apply calibration.
5. Download the Rhino file and continue modeling in Rhino.

选择模式 → 上传图片 → 检查叠加效果 → 调整参数 → 两点尺寸标定 → 下载 Rhino 文件。

Drag with the left mouse button to pan; scroll to zoom; use **Fit view** to fit the drawing. **Show nodes** displays curve nodes.

左键拖动画布，滚轮缩放；点击“适应画布”复位，勾选“显示节点”查看曲线节点。

### From code · 代码生成曲线

1. Open **Code drawing**, then copy the model prompt into an external AI chat.
2. Discuss your design and dimensions, and ask for JSON in the specified format.
3. Paste the JSON or import a `.json` file, then click **Generate drawing**. You can load an example first.
4. Set the overall width or height. **Linked** keeps proportions; **Unlinked** adjusts each dimension independently. Press Enter or leave the field to apply.
5. Optionally calibrate with two points, then download the `.3dm` file.

复制提示词到外部大模型 → 讨论设计并获得 JSON → 粘贴或导入 → 生成线稿 → 调整毫米尺寸 → 导出。

Unlinked resizing can turn circles into ellipses. Two-point calibration always scales the current drawing proportionally. **Reset to code size** restores the dimensions defined by the imported code.

解除联动可能将圆拉成椭圆；两点标定始终保持当前图形比例。“恢复代码尺寸”会恢复导入代码定义的尺寸。

The website does not call an AI service or automatically convert a generated image into code. JSON describes the geometry directly; it is data, not executable code.

网站不直接调用大模型，也不自动将生成图片变成代码。需要外部模型按约定输出几何数据。

### A hand-writable example · 手写示例

A circle with a radius of 10 mm / 半径 10 mm 的圆：

```json
{
  "version": 1,
  "units": "mm",
  "name": "Ring",
  "shapes": [
    {
      "id": "outer-ring",
      "type": "circle",
      "center": [0, 0],
      "radius": 10
    }
  ]
}
```

Coordinates use millimeters, X to the right and Y upward. The code workspace includes format rules and examples. The source distribution also contains `IMPORT_FORMAT.md`, `IMPORT_FORMAT.en.md`, `MODEL_PROMPT.md`, `MODEL_PROMPT.en.md`, `drawing.schema.json` and `examples/`.

坐标单位为毫米，X 向右、Y 向上。完整规则和示例可在网页专业模式中查看，也包含在源码目录的上述文件中。

## Run locally · 本地运行

These commands require the **source distribution**, Node.js and npm. Use Node.js 22.12+ or another version supported by the installed Vite release.

以下命令在包含 `package.json` 的源码目录运行；建议使用 Node.js 22.12+。

```sh
npm install
npm run dev
```

Open **http://127.0.0.1:5181/**. On macOS, after installing dependencies, you can also double-click `启动网页.command`.

安装依赖后，也可在 macOS 双击 `启动网页.command` 启动。

```sh
npm test       # Run regression tests / 回归测试
npm run build # Type-check and build / 类型检查与构建
```

The production website is generated in `dist/`. Open it through an HTTP(S) server, not by double-clicking `index.html`; Worker and WASM resources need normal web loading.

构建后的网页位于 `dist/`。请通过本地服务器或在线网址访问，不要直接双击 HTML 文件。

## Publish with GitHub Pages · 发布到 GitHub Pages

For a simple static deployment:

1. Run `npm run build` from the source directory, or use the supplied, up-to-date `dist/`.
2. Upload the **contents of `dist/`** to the root of a publishing repository or branch. Keep `index.html`, `favicon.svg` and the entire `assets/` folder together. You may also include this README.
3. In the repository, open **Settings → Pages → Build and deployment**, choose **Deploy from a branch**, then select your publishing branch and **/ (root)**.
4. Save and wait for GitHub to finish deployment. The Pages settings will show the site URL.

最简单的发布方式：把 **dist 里面的全部内容**放到发布仓库根目录，再在 Pages 设置中选择该分支的根目录。不要只上传一个 HTML，也不要把 `dist` 外层文件夹当成可直接选择的发布目录。

If you want source and deployment in the same repository, use a separate publishing branch or configure a GitHub Actions workflow to build and publish `dist/`. Uploading Vite source files alone does not deploy the built website. This project does not include an automatic deployment workflow.

若同一仓库同时保存源码，建议使用独立发布分支，或另行配置 GitHub Actions 构建并发布 `dist/`。不要把编译后的文件覆盖到源码目录；目前未附带自动部署工作流。

The build uses relative asset paths for repository subpaths. Do not publish `node_modules/`, `.cache/`, private reference images or historical backups.

构建使用相对资源路径。无需上传依赖目录、测试缓存、私人原图或历史备份。

Publishing-source options: [GitHub Pages documentation](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site).

## Rhino output · Rhino 文件

- Rhino 7 `.3dm` format, compatible with Rhino 8.
- Millimeters, XY plane, Z = 0; editable curves without an embedded source image.
- Image workflows place the overall lower-left boundary at the origin and convert image Y-down to CAD Y-up.
- Code drawings preserve their declared origin and use Y-up coordinates.
- Open and closed states are preserved. Invalid geometry or pending edits may block export; follow the on-screen message.

文件按毫米保存二维曲线；图片导出以整体左下角为原点，代码导出保留代码原点。导入使用其他单位的 Rhino 模型时，请正确处理单位换算。

## Limits and saving · 边界与保存

- This is a 2D tracing and drawing tool, not automatic 3D modeling.
- Image calibration does not remove perspective distortion or establish manufacturing accuracy.
- Dense junctions, reflections, tiny details and low-resolution images may need manual cleanup. Fine sampling cannot recover missing source detail.
- No SVG/DXF export, manual curve editing, cloud storage or built-in AI API integration is included.
- Images, drawings and calibration are held in page memory. **Save your JSON and exported files before refreshing or closing the tab.** Language preference is stored locally when available.

工具不自动生成完整三维模型，不校正透视，也不保证复杂线稿全部识别正确。刷新或关闭网页会清除当前图片、标定和绘图，请提前保存代码与导出文件。

## Built with · 技术栈

Vite · TypeScript · Canvas / SVG · Web Workers · OpenCV.js · Paper.js · rhino3dm

The interface uses a GitHub Primer-inspired light palette. Contour Studio is an independent project and is not affiliated with GitHub or Rhino.
