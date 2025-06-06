---
layout: research
permalink: /dreamland/
title: "Dreamland: Controllable World Creation with Simulator and Generative Models"
page_title: "Dreamland: Controllable World Creation with Simulator and Generative Models"
authors:

- {name: "Sicheng Mo<sup>*</sup>", url: "https://ziyangxie.site/"}
- {name: "Ziyang Leng<sup>*</sup>", url: "#"}
- {name: "Leon Liu", url: "https://pengzhenghao.github.io"}
- {name: "Weizhen Wang", url: "https://wywu.github.io/"}
- {name: "Honglin He", url: "https://wywu.github.io/"}
- {name: "Bolei Zhou", url: "https://boleizhou.github.io/"}

institutions:

- {name: "University of California, Los Angeles"}

nav: false
nav_order: 1
code_link: https://github.com/Vid2Sim/Vid2Sim
pdf_link: https://arxiv.org/abs/2501.06693

---



<style>
.video-container {
  position: relative;
  max-width: 100%; /* Adjust this value to control the maximum width of the video container */
}

.teaser {
  margin: -18px auto -18px; /* Optional: center the video container horizontally */
}

.video-container video {
  display: block;
  margin: 0 auto;
  max-width: 100%;
  max-height: 100%;
}

/* .video-grid {
    margin-top: 18px;
    display: grid;
    grid-template-columns: 1fr 1fr; /* Creates two columns */
    grid-gap: 30px; /* Space between videos */
} */

.video-grid {
    margin-top: 18px;
    display: grid;
    grid-template-rows: 1fr 1fr 1fr; /* Two rows */
    grid-gap: 70px; /* Space between items */
    justify-items: center; /* Horizontally center items */
    align-items: center; /* Vertically center items */
}

.video-grid figure {
    display: flex;
    flex-direction: column; /* Stack video and caption */
    align-items: center; /* Center video and caption */
    justify-content: center; /* Center content */
    margin: 0; /* Reset default margin */
}

.video-grid video {
    display: block;
    width: 80%; /* Adjust as needed */
    height: auto; /* Maintain aspect ratio */
}

.video-section {
  margin-bottom: 60px;
}

video {
  width: 80%;
  max-width: 600px;
  height: auto;
  margin: 0 auto;
}

.dots {
  display: flex;
  justify-content: center;
  gap: 10px;
}

.dot {
  font-size: 24px;
  cursor: pointer;
  color: #aaa;
}

.dot.active {
  color: #333;
}

</style>

<div class="video-container teaser">
  <video loop autoplay muted playsinline src="../assets/img/vid2sim/simulation/sim_nav1.mp4"></video>
</div>



<!--research-section-splitter-->


## TL; DR

:selfie: **Dreamland** is a hybird generation pipeline that connects simulators and generative models to achieve controllable and configurable world creation.

:robot: **Dreamland** demonstrates superior quality and controllability in scene generation, and improves the adaptation of embodied agents to the real world.

<div style="border-top: 1px solid #ccc; margin: 30px 0;"></div>


## Dreamland-Video


<div style="display: grid; grid-template-columns: 1fr; gap: 20px; margin: 0 auto;">
  <figure style="display: flex; flex-direction: column; gap: 8px;">
    <video id="videoPlayer1" style="display:block; width:80%; height:auto;" muted autoplay loop controls playsinline>
      <source src="../assets/img/vid2sim/simulation/sim_nav1.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
    <div class="dots" data-player="videoPlayer1">
      <span class="dot active" data-src="../assets/img/vid2sim/simulation/sim_nav1.mp4">●</span>
      <span class="dot" data-src="../assets/img/vid2sim/simulation/sim_nav2.mp4">●</span>
      <span class="dot" data-src="../assets/img/vid2sim/simulation/sim_nav1.mp4">●</span>
    </div>
    <figcaption style="text-align: center; font-size: 18px;">
        Simulator-Conditioned Generation
    </figcaption>
  </figure>

  <figure style="display: flex; flex-direction: column; gap: 8px;">
    <video id="videoPlayer2" style="display:block; width:80%; height:auto;" muted autoplay loop controls playsinline>
      <source src="../assets/img/vid2sim/realworld/realworld_nav1.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
    <div class="dots" data-player="videoPlayer2">
      <span class="dot active" data-src="../assets/img/vid2sim/realworld/realworld_nav1.mp4">●</span>
      <span class="dot" data-src="../assets/img/vid2sim/realworld/realworld_nav2.mp4">●</span>
      <span class="dot" data-src="../assets/img/vid2sim/realworld/realworld_nav1.mp4">●</span>
    </div>
    <figcaption style="text-align: center; font-size: 18px;">
        Diverse Scene Generation
    </figcaption>
  </figure>

  <figure style="display: flex; flex-direction: column; gap: 8px;">
    <video id="videoPlayer3" style="display:block; width:80%; height:auto;" muted autoplay loop controls playsinline>
      <source src="../assets/img/vid2sim/realworld/realworld_nav1.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
    <div class="dots" data-player="videoPlayer3">
      <span class="dot active" data-src="../assets/img/vid2sim/realworld/realworld_nav1.mp4">●</span>
      <span class="dot" data-src="../assets/img/vid2sim/realworld/realworld_nav2.mp4">●</span>
      <span class="dot" data-src="../assets/img/vid2sim/realworld/realworld_nav1.mp4">●</span>
    </div>
    <figcaption style="text-align: center; font-size: 18px;">
        Safety-Critical Scene
    </figcaption>
  </figure>
</div>

<script>
  document.querySelectorAll('.dots').forEach(dotContainer => {
    const videoId = dotContainer.getAttribute('data-player');
    const videoElement = document.getElementById(videoId);
    const dots = dotContainer.querySelectorAll('.dot');

    dots.forEach(dot => {
      dot.addEventListener('click', () => {
        const src = dot.getAttribute('data-src');
        videoElement.src = src;
        videoElement.play();
    
        dots.forEach(d => d.classList.remove('active'));
        dot.classList.add('active');
      });
    });
  });
</script>


  
## Dreamland Architecture


<div class="img-container" style="width: 100%; margin: 5px auto;">
    <img src="../assets/img/dreamland/pipeline.png" class="my-image" alt="Image" />
</div>

Dreamland pipeline consists of three key stages: (1) *Stage-1 Simulation*: scene construction with physics-based simulator, (2) *Stage-2 LWA-Sim2Real*: transferring the Sim-LWA from simulation to Real-LWA with an instructional editing model and user instructions, and (3) *Stage-3 Mixed-Condition Generation*: rendering an aesthetic and realistic scene with a large-scale pretrained image or video generation model

<div style="margin-bottom: 15px"></div>

## Experiments

<div class="img-container" style="width: 100%; margin: 5px auto;">
    <img src="../assets/img/dreamland/experiments.png" class="my-image" alt="Image" />
</div>

Dreamland pipeline demonstrate superior quality and controllability, with scalability that benefits from stronger pre-trained model deployed for *Stage-3*.

<div style="margin-bottom: 15px"></div>

## Dreamland Extension
<div class="img-container" style="width: 100%; margin: 5px auto;">
    <img src="../assets/img/dreamland/extension.png" class="my-image" alt="Image" />
</div>

Dreamland pipeline is generalized to various downstream tasks, including **scene editing**, **safety-critical scene generation**, and **zero-shot generation on urban simulator**.



<!-- <pre><code class="language-plain">@article{xie2024vid2sim,
  title={Vid2Sim: Realistic and Interactive Simulation from Video for Urban Navigation},
  author={Ziyang Xie and Zhizheng Liu and Zhenghao Peng and Wayne Wu and Bolei Zhou},
  journal={Preprint},
  year={2024}
}
</code></pre> -->


