# Oliver2

## 项目说明
这是一个基于HTML、CSS和JavaScript的Web项目，核心功能是展示一个由粒子构成的图形在不同3D形状（如球体、立方体、圆环、金字塔、圆柱体、汽车模型等）之间进行平滑变形的动画效果。用户可以通过交互控件切换形状和颜色主题。

## 项目结构
```
Oliver2/
├── css/          # CSS样式文件目录
├── js/           # JavaScript文件目录
├── images/       # 图片资源目录
└── index.html    # 主页面，实现了粒子变形动画和用户交互功能。
```

## `index.html` 详细说明
`index.html` 文件是本项目唯一的展示页面，它包含了所有的HTML结构、CSS样式和JavaScript逻辑。

### 主要功能：
1.  **3D粒子动画渲染**：使用 Three.js 库在浏览器中渲染一个由大量粒子组成的动态3D图形。
2.  **形状变形**：预设了多种3D形状，粒子可以从当前形状平滑地过渡到下一个形状。目前支持的形状有：
    *   球体 (Sphere)
    *   立方体 (Cube)
    *   圆环 (Torus)
    *   金字塔 (Pyramid)
    *   圆柱体 (Cylinder)
    *   运动跑车 (Sport Car) - 一个更具流线型和运动感的汽车轮廓。
    *   圆锥体 (Cone)
    *   螺旋体 (Helix)
    *   胶囊体 (Capsule)
    *   3D爱心 (3D Heart)
    *   3D五角星 (3D Star)
    *   圆环结 (Torus Knot)
    *   正十二面体 (Dodecahedron)
    *   正二十面体 (Icosahedron)
    *   平面片 (Plane)
    *   八面体 (Octahedron)
3.  **随机不重复的形状切换**：
    *   页面首次加载时，会随机展示一个初始形状。
    *   每次用户请求切换形状（通过按钮或点击画布），程序会随机选择一个在本轮尚未展示过的形状进行变形。
    *   当所有形状都至少展示过一次后，会开始新一轮的随机不重复切换。
4.  **平滑颜色过渡**：当粒子从一种形状变形到另一种形状，或用户切换颜色主题时，粒子的颜色会平滑地过渡，而不是突然跳变。
5.  **交互控制**：
    *   用户可以通过点击页面上的"Next Shape"按钮或直接点击3D画布来触发形状的变形。
    *   页面底部提供了一组颜色按钮，用户可以选择不同的颜色主题（如中国红、克莱因蓝、自定义三色混合、彩虹色）来改变粒子的颜色。
    *   支持使用鼠标拖拽（OrbitControls）来改变观察3D场景的视角。
    *   初始化 `OrbitControls` 以便用户可以通过鼠标控制视角。
    *   初始化形状列表，调整了各个形状的生成参数，以使其在视觉上大小更统一，3D效果更饱满。
    *   实现随机不重复的初始形状选择和后续形状切换逻辑，记录已访问形状并在完成一轮后重置。
    *   为画布添加了 `mousedown`, `mousemove`, 和 `mouseup` 事件监听器，以区分点击（触发变形）和拖拽（仅改变视角）操作。
6.  **信息显示**：页面顶部会显示当前展示的形状名称，并在变形过程中提示"Morphing..."。
7.  **交互优化**：区分了鼠标点击和拖拽操作。用户通过鼠标拖拽（OrbitControls）来改变观察3D场景的视角时，不会意外触发形状的变形；仅当用户单击画布时，才会触发形状变形。

### JavaScript (`<script>` 标签内) 核心逻辑:
*   **`init()` 函数**:
    *   初始化 Three.js 的场景 (`scene`)、透视相机 (`camera`) 和 WebGL 渲染器 (`renderer`)。
    *   设置相机初始位置，并将渲染器添加到HTML的 `body` 中。
    *   初始化 `OrbitControls` 以便用户可以通过鼠标控制视角。
    *   初始化形状列表，调整了各个形状的生成参数，以使其在视觉上大小更统一，3D效果更饱满。
    *   实现随机不重复的初始形状选择和后续形状切换逻辑，记录已访问形状并在完成一轮后重置。
    *   为画布添加了 `mousedown`, `mousemove`, 和 `mouseup` 事件监听器，以区分点击（触发变形）和拖拽（仅改变视角）操作。
    *   为"Next Shape"按钮和颜色选择按钮绑定了相应的事件处理函数。
    *   调用各个 `get<Shape>Positions()` 函数（如 `getSpherePositions`, `getCarPositions` (已更新为跑车造型), `getConePositions`, `getHelixPositions`, `getCapsulePositions`, `getHeart3DPositions`, `getStar3DPositions`, `getTorusKnotPositions`, `getDodecahedronPositions`, `getIcosahedronPositions`, `getPlanePositions`, `getOctahedronPositions` 等）来预先计算并存储构成不同3D形状的粒子目标位置，并将这些数据存储在 `shapes` 数组中。
    *   创建粒子系统 (`particles`)：
        *   使用 `THREE.BufferGeometry` 存储粒子的位置和颜色属性。
        *   初始粒子位置设置为 `shapes` 数组中的第一个形状。
        *   使用 `THREE.PointsMaterial` 定义粒子的外观（大小、是否使用顶点颜色等）。
    *   调用 `applyColorPalette()` 应用初始的颜色主题。
    *   调用 `updateInfoText()` 更新形状信息显示。
    *   为窗口大小调整、形状切换按钮、画布点击以及颜色切换按钮绑定事件监听器。
    *   启动 `animate()` 动画循环。
*   **`triggerMorph()` 函数**:
    *   当用户请求切换形状时被调用。
    *   如果当前正在变形，则不执行任何操作。
    *   记录当前粒子位置和颜色作为变形的起始状态 (`sourcePositions`, `sourceColors`)。
    *   从当前轮次尚未访问过的形状中随机选择下一个目标形状的索引 (`currentShapeIndex`)，并记录到已访问列表。
    *   如果所有形状均已访问，则重置已访问列表，并从中重新选择（尽量避免立即重复当前形状）。
    *   获取目标形状的粒子位置 (`targetPositions`) 和计算目标颜色 (`targetColors`)。
    *   设置 `isMorphing` 为 `true`，重置 `morphProgress`，并更新信息文本为"Morphing..."。
*   **`updateInfoText()` 函数**: 更新页面顶部显示的当前形状名称。
*   **`get<Shape>Positions()` 系列函数** (例如 `getSpherePositions`, `getCubePositions`, `getTorusPositions`, `getPyramidPositions`, `getCylinderPositions`, `getCarPositions`):
    *   这些函数为每种预设的3D形状计算并返回一个 `Float32Array`，其中包含 `particleCount` 个粒子的三维坐标 (x, y, z)。
    *   它们使用不同的数学算法或几何定义来在各自形状的表面或体积内随机或规律地分布粒子。例如，`getSpherePositions` 使用三角函数在球面均匀分布点，`getCubePositions` 在立方体内随机分布点，`getCarPositions` 则通过组合多个立方体和圆柱体来近似汽车的形状。
*   **`applyColorPalette()` 函数**:
    *   根据传入的调色板名称 (`paletteName`) 更新粒子系统中每个粒子的颜色。
    *   它会计算当前粒子几何体的边界框 (`boundingBox`)，以便根据粒子在形状内的相对位置 (归一化的 x, y, z 坐标) 来应用颜色。
    *   支持的调色板包括：
        *   `'chinese_red'`: 基于中国红 (`0xCD0000`)，强度随X轴位置变化。
        *   `'klein_blue'`: 基于克莱因蓝 (`0x002FA7`)，强度随Y轴位置变化。
        *   `'custom_tri_color'`: 将中国红、克莱因蓝和一种匹配的黄色 (`0xFFD700`) 根据粒子在X, Y, Z轴的归一化位置进行混合。
        *   `'rainbow'` (默认): 根据粒子在X, Y, Z轴的归一化位置直接映射为RGB颜色。
    *   修改颜色后，设置 `colorsAttribute.needsUpdate = true` 以便 Three.js 更新GPU中的数据。
*   **`onWindowResize()` 函数**: 当浏览器窗口大小改变时，调整相机宽高比和渲染器尺寸，并更新投影矩阵。
*   **`animate()` 函数**:
    *   使用 `requestAnimationFrame` 创建动画循环，每一帧都会被调用。
    *   调用 `controls.update()` 更新鼠标控制器的状态。
    *   如果 `isMorphing` 为 `true`：
        *   增加 `morphProgress` (变形进度)。
        *   使用 `THREE.MathUtils.smootherstep` 或线性插值 (`lerp`) 计算当前变形阶段的中间位置，并更新粒子几何体的位置属性。
        *   标记位置属性需要更新 (`currentPosAttribute.needsUpdate = true`)。
        *   当变形完成 (`morphProgress >= morphDuration`)，设置 `isMorphing` 为 `false`，更新信息文本，并重新应用颜色调色板（因为形状的边界框可能已改变）。
    *   轻微旋转整个粒子系统 (`particles.rotation.y += 0.0005`)，提供一个持续的、缓慢的自动旋转效果。
    *   调用 `renderer.render(scene, camera)` 渲染当前帧。
*   **脚本末尾调用 `init()`**: 启动整个应用。

## 功能特性
- 响应式设计，适应不同屏幕尺寸
- 基于 Three.js 实现的炫酷3D粒子动画效果
- 多种预设3D形状（球体、立方体、圆环、金字塔、圆柱体、汽车等）之间的平滑变形
- 支持多种颜色主题切换（中国红、克莱因蓝、三色混合、彩虹色），并根据粒子位置动态着色
- 现代化UI界面和用户友好的交互体验（点击切换、拖拽查看）

## 使用说明
1.  直接在现代浏览器中打开 `index.html` 文件即可查看和交互。
2.  **外部依赖**:
    *   本项目使用了 [Three.js](https://threejs.org/) (r128版本) JavaScript库来实现3D渲染。该库通过CDN (`https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js`) 引入，因此首次加载页面时需要网络连接。
    *   Three.js 的 `OrbitControls` 用于鼠标视角控制，已包含在 r128 的 `three.min.js` 中，通过 `THREE.OrbitControls` 访问。
3.  所有自定义CSS样式直接写在 `index.html` 的 `<style>` 标签内。
4.  所有JavaScript逻辑也直接写在 `index.html` 的 `<script>` 标签内。

## 未来可能的改进方向 (TODO)
- **代码模块化**: 将CSS样式和JavaScript逻辑分别提取到独立的 `.css` 和 `.js` 文件中，放到 `css/` 和 `js/` 目录下，以符合项目结构规划。
- **本地化依赖**: 将 `three.min.js` 下载到本地（例如 `js/libs/` 目录），并修改 `index.html` 中的引用路径，以避免CDN依赖，确保项目可以离线运行。
- **新增形状**: 增加更多有趣的3D形状供用户选择。
- **自定义形状**: 允许用户通过某种方式（例如上传模型文件或参数化输入）创建自定义形状。
- **性能优化**: 针对大量粒子的情况，探索更优的粒子生成和更新策略。
- **更多交互**: 增加例如粒子大小、变形速度等参数的控制选项。
- **完善汽车模型**: 当前汽车模型较为基础，可以考虑使用更精细的模型数据或生成算法。

## 未来展望
*   **加载外部3D模型**: 允许用户上传或指定外部的3D模型文件（如 `.obj`, `.gltf` 格式），让粒子构成的形状更加复杂和逼真，例如实现真正的"法拉利跑车"模型。
*   **更高级的粒子行为**: 例如，让粒子在变形时能够感知到其他粒子的存在，产生碰撞、排斥或吸引的效果。
*   **用户自定义形状**: 允许用户通过参数或简单的绘图方式定义自己的3D形状。

---
*文档更新于 {当前日期和时间}*