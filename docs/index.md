---
hide:
  - navigation
  - toc
---

# Hexu Zhao

<div id="spark-viewer"></div>

<style>
#spark-viewer {
  width: 300px;
  height: 300px;
  border: 2px solid #ddd;
  border-radius: 15px;
  margin: 0 0 0 auto;
  float: right;
  margin-left: 2em;
  margin-top: -5em;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

/* 手机端响应式设计 */
@media (max-width: 768px) {
  #spark-viewer {
    width: 80%;
    max-width: 300px;
    height: 250px;
    float: none;
    margin: 1em auto;
    margin-top: 0;
    display: block;
  }
}

/* 小屏手机 */
@media (max-width: 480px) {
  #spark-viewer {
    width: 90%;
    height: 200px;
  }
}
</style>

<script type="importmap">
{
  "imports": {
    "three": "https://cdnjs.cloudflare.com/ajax/libs/three.js/0.174.0/three.module.js",
    "@sparkjsdev/spark": "https://sparkjs.dev/releases/spark/0.1.8/spark.module.js"
  }
}
</script>

<script type="module">
import * as THREE from "three";
import { SplatMesh } from "@sparkjsdev/spark";

const container = document.getElementById('spark-viewer');
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(60, 1, 0.1, 1000);
const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });

// 动态设置渲染器尺寸
function updateRendererSize() {
  const width = container.offsetWidth;
  const height = container.offsetHeight;
  renderer.setSize(width, height);
  camera.aspect = width / height;
  camera.updateProjectionMatrix();
}

updateRendererSize();
renderer.setClearColor(0x000000, 0); // Transparent background
container.appendChild(renderer.domElement);

// 设置相机位置：从Y轴负方向向原点拍摄
// 调整距离以获得最佳视角 (可根据模型大小调整此值)
const cameraDistance = 0.7;
camera.position.set(-cameraDistance, 0.2, 0.02);
camera.lookAt(0, 0.1, 0);

// 使用本地的点云模型 (直接读取PLY文件)
const splatURL = "assets/IMG_8856_ds2_mcmc_prune_sh1.ply";
const pointCloud = new SplatMesh({ url: splatURL });
pointCloud.quaternion.set(1, 0, 0, 0);
pointCloud.position.set(0, 0, -0.02);
// 上下对称：沿Y轴镜像翻转
// pointCloud.scale.y = -1;
pointCloud.scale.z = -1;
scene.add(pointCloud);

// 添加基本的鼠标控制
let mouseDown = false;
let mouseX = 0;
let mouseY = 0;

container.addEventListener('mousedown', (e) => {
  mouseDown = true;
  mouseX = e.clientX;
  mouseY = e.clientY;
});

container.addEventListener('mouseup', () => {
  mouseDown = false;
});

container.addEventListener('mousemove', (e) => {
  if (mouseDown) {
    const deltaX = e.clientX - mouseX;
    const deltaY = e.clientY - mouseY;
    // 从Y轴负方向观察，调整旋转轴以获得直观的控制
    pointCloud.rotation.y += deltaX * 0.01;
    pointCloud.rotation.z += deltaY * 0.01;
    mouseX = e.clientX;
    mouseY = e.clientY;
  }
});

// 渲染循环
const initialCameraPosition = { x: -cameraDistance, y: 0.2, z: 0.02 };
const radius = Math.sqrt(initialCameraPosition.x * initialCameraPosition.x + initialCameraPosition.z * initialCameraPosition.z);
// 计算初始角度，确保第一帧显示原始视角
const initialAngle = Math.atan2(initialCameraPosition.z, initialCameraPosition.x);
let time = initialAngle;

function animate() {
  requestAnimationFrame(animate);
  
  if (!mouseDown) {
    // 相机自动沿y轴旋转
    time += 0.01;
    camera.position.x = Math.cos(time) * radius;
    camera.position.z = Math.sin(time) * radius;
    camera.position.y = initialCameraPosition.y; // 保持y位置不变
    camera.lookAt(0, 0.1, 0); // 始终看向固定点
  }
  
  renderer.render(scene, camera);
}
animate();

// 响应式调整
window.addEventListener('resize', updateRendererSize);
</script>

*:fontawesome-solid-building: Office: [424, 60 5th Ave, New York, NY 10011](https://maps.app.goo.gl/N7m2fM5EbM3TToB79)*

*:fontawesome-solid-inbox: Work Email: [hz3496 [at] nyu [dot] edu](mailto:hz3496@nyu.edu)*


<span style=font-size:2em;">[:academicons-cv:](assets/HexuZhao_CV_20251105.pdf) [:fontawesome-solid-envelope:](mailto:hz3496@nyu.edu) [:academicons-google-scholar:](https://scholar.google.com/citations?hl=en&user=ylKFMAkAAAAJ) [:academicons-dblp:](https://dblp.org/pid/293/9714.html) [:fontawesome-brands-github:](https://github.com/TarzanZhao) [:fontawesome-brands-linkedin:](https://www.linkedin.com/in/hexu-zhao-203304244/) [:fontawesome-brands-zhihu:](https://www.zhihu.com/people/zhao-he-xu-61)</span>

## Bio

I am a Ph.D. student of Computer Science at [NYU Courant](https://cs.nyu.edu/home/index.html), advised by Prof. [Jinyang Li](https://jinyangli.github.io) and Prof. [Aurojit Panda](https://cs.nyu.edu/~apanda/). My research focuses on Machine Learning System, especially the emerging niche of **System for Gaussian Splatting**. From a systems perspective, I leverage both distributed computation and kernel optimization techniques. From a workload perspective, I optimize for both static and dynamic scene reconstruction. Previously, I obtained my bachelor’s degree in 2023 from Honored Yao Class, Tsinghua University. 
<!-- In high school, I taught myself to win [Zibo City](https://en.wikipedia.org/wiki/Zibo)’s first National Informatics Olympiad gold medal (NOI2018).  -->

_"I’ve always had a taste for stepping away from the mainstream — but not too far — to look for ideas that are overlooked yet full of potential, in research, in life, and beyond."_

<!-- _"I’ve always tried to find and take on the small things in this world that I’m meant, more than anyone else, to do."_  -->

<details>
<summary><strong>News</strong></summary>

[01/2025] I will join [NVIDIA Spatial Intelligence Lab](https://research.nvidia.com/labs/sil/) as a Research Scientist Intern in 2025 Summer. See you in Santa Clara!

[03/2024] I will join Microsoft DeepSpeed Team as a Research Scientist Intern in 2024 Summer. See you in Seattle!

</details>

## Education

### New York University Courant Institute![Image title](images/nyu.png){ align=right style="height:6em; border-radius: 0.5em;"}

*Sept. 2023 -- Present*

***Ph.D. in Computer Science**, advised by Prof. [Jinyang Li](https://www.news.cs.nyu.edu/~jinyang/) and Prof. [Aurojit Panda](https://cs.nyu.edu/~apanda/)*

### Tsinghua University![Image title](https://github.com/TarzanZhao/TarzanZhao.github.io/assets/45677459/cdd93597-e2c5-472f-bfb2-7e0fb20961b7){ align=right style="height:6em; border-radius: 0.5em;"}

*Sept. 2019 -- June 2023*

***B.Eng. in Computer Science (Honored Yao Class)***

## Experience

**NVIDIA Spatial Intelligence Lab**![Image title](images/NVLogo_2D.jpg){ align=right style="height:5em; border-radius: 0.5em;"}

*May. 2025 – Dec. 2025*

*Research Scientist Intern*

**Microsoft Deepspeed**![Image title](images/ms.png){ align=right style="height:5em; border-radius: 0.5em;"}

*May. 2024 – Aug. 2024*

*Research Scientist Intern*

## Publications

**Scaling Point-based Differentiable Rendering for Large-scale Reconstruction**

<u>Hexu Zhao</u>, Xiaoteng Liu, Xiwen Min, Jianhao Huang, Youming Deng, Yanfei Li, Ang Li, Jinyang Li, Aurojit Panda

*In submission*&nbsp;&nbsp;[:academicons-arxiv: arXiv](assets/gaian_1222.pdf)&nbsp;&nbsp;
<!-- [:fontawesome-brands-github: Code](https://github.com/nyu-systems/CLM-GS)&nbsp;&nbsp;[:fontawesome-solid-link: Project Page](https://tarzanzhao.github.io/CLM-GS/) -->

**CLM: Removing the GPU Memory Barrier for 3D Gaussian Splatting**

<u>\*Hexu Zhao</u>, \*Xiwen Min, Xiaoteng Liu, Moonjun Gong, Yiming Li, Ang Li, Saining Xie, Jinyang Li, Aurojit Panda

*ASPLOS 2026*&nbsp;&nbsp;[:academicons-arxiv: arXiv](https://arxiv.org/abs/2511.04951)&nbsp;&nbsp;[:fontawesome-brands-github: Code](https://github.com/nyu-systems/CLM-GS)&nbsp;&nbsp;[:fontawesome-solid-link: Project Page](https://tarzanzhao.github.io/CLM-GS/)

**On Scaling Up 3D Gaussian Splatting Training**

<u>Hexu Zhao</u>, \*Haoyang Weng, \*Daohan Lu, Ang Li, Jinyang Li, Aurojit Panda, Saining Xie

*ICLR2025 Oral*&nbsp;&nbsp;[:academicons-arxiv: arXiv](https://arxiv.org/abs/2406.18533)&nbsp;&nbsp;[:fontawesome-brands-github: Code](https://github.com/nyu-systems/Grendel-GS)&nbsp;&nbsp;[:fontawesome-solid-link: Project Page](https://daohanlu.github.io/scaling-up-3dgs/)

**On Optimizing the Communication of Model Parallelism**

\*Yonghao Zhuang, <u>\*Hexu Zhao</u>, Lianmin Zheng, Zhuohan Li, Eric P. Xing, Qirong Ho, Joseph E. Gonzalez, Ion Stoica, Hao Zhang

*MLSys2023*&nbsp;&nbsp;[:academicons-arxiv: arXiv](https://arxiv.org/pdf/2211.05322.pdf)&nbsp;&nbsp;

**Fully Hyperbolic Neural Networks**

Weize Chen, Xu Han, Yankai Lin, <u>Hexu Zhao</u>, Zhiyuan Liu, Peng Li, Maosong Sun, Jie Zhou

*ACL2022*&nbsp;&nbsp;[:academicons-arxiv: arXiv](https://arxiv.org/pdf/2105.14686.pdf)&nbsp;&nbsp;

**Development of a Doctor-in-the-loop Interpretation Framework for Insulin Titration in Diabetes: a Proof-of-concept Study**

\*Haowei He, \*Zhen Ying, Biao Li, Yujuan Fan, Ping Wang, Jiaping Lu, Liming Wu, <u>Hexu Zhao</u>, Xiaoying Li, Yang Yuan, Ying Chen

*In submission to The Lancet Diabetes & Endocrinology Journal*&nbsp;&nbsp; [:academicons-google-scholar:](https://scholar.google.com/citations?view_op=view_citation&hl=en&user=ylKFMAkAAAAJ&citation_for_view=ylKFMAkAAAAJ:d1gkVwhDpl0C)&nbsp;&nbsp;
