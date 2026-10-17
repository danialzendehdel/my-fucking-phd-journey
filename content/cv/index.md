---
title: "CV"
description: "Curriculum vitae of Danial Zendehdel: deep reinforcement learning for energy systems and multi-agent coordination, computer vision and robotics."
showHero: false
showTableOfContents: true
---

Research engineer at Honda Research Institute Europe and PhD candidate in electronics engineering at Sapienza University of Rome, defending in October 2026. Specialised in deep reinforcement learning for sequential decision making under uncertainty, in continuous and discrete action spaces and with multi-objective, constrained and multi-agent formulations. Applied to battery dispatch and energy management systems in the PhD, and to multi-agent coordination under shared resource constraints at Honda. Earlier work in computer vision and robotics at EPFL, including deep stereo networks for 360-degree depth estimation and a real-time person detection and tracking pipeline deployed on a mobile robot.

**Available from November 2026.** [LinkedIn](https://www.linkedin.com/in/danialzendehdel/) · [GitHub](https://github.com/danialzendehdel) · [Publications](/publications/) · [Projects](/projects/)

## Experience

<div class="cv-entry">
<div class="cv-head"><span class="cv-org">Honda Research Institute Europe GmbH</span><span class="cv-when">Jun 2026 – present</span></div>
<div class="cv-sub"><span>Research Engineer</span><span>Offenbach, Germany</span></div>
</div>

- Trained multi-agent deep reinforcement learning policies for electric vehicle charging under a shared power budget; reached a wider Pareto front on efficiency and fairness than the group's evolutionary (genetic programming) schedulers.
- Replaced hand-tuned reward weights with an explicit fairness constraint in actor-critic policy gradient training, giving an interpretable and verifiable fairness guarantee between agents for trustworthy deployment.
- Built the simulation environment, cost model and evaluation pipeline for benchmarking against optimisation-based and evolutionary baselines under the group's published protocol.

<div class="cv-entry">
<div class="cv-head"><span class="cv-org">TU Delft</span><span class="cv-when">Sep 2025 – Apr 2026</span></div>
<div class="cv-sub"><span>Guest Researcher</span><span>Delft, Netherlands</span></div>
</div>

- Wrote a reinforcement learning environment and trained agents in it to simulate the long-term energy transition of the Netherlands at national scale.
- Set up multi-year investment and retrofitting choices under uncertainty as a sequential decision problem, covering renewable integration and system planning.

<div class="cv-entry">
<div class="cv-head"><span class="cv-org">Sapienza Università di Roma</span><span class="cv-when">Nov 2023 – Oct 2026</span></div>
<div class="cv-sub"><span>PhD Researcher, Electronics Engineering</span><span>Rome, Italy</span></div>
</div>

- Trained deep reinforcement learning agents for battery dispatch in a real-time energy management system (EMS) of a renewable energy community, in both continuous and discrete action spaces; achieved lower operating cost than forecasting-based and optimisation baselines.
- Reduced battery degradation at comparable operating cost by writing multi-objective rewards that trade the two against each other; published as a degradation-aware RL framework (IJCCI 2025) and an enhanced SAC algorithm (IJCNN 2025, IEEE).
- Built the simulation environment around an extended battery equivalent-circuit model, giving the battery management system (BMS) side of the EMS a physical basis (IJCNN 2024, IEEE).
- Compared seasonal and non-seasonal forecasting methods for energy demand time series in smart grids and selected the forecaster used by the EMS (IJCCI 2023).

<div class="cv-entry">
<div class="cv-head"><span class="cv-org">École Polytechnique Fédérale de Lausanne (EPFL)</span><span class="cv-when">Feb 2022 – Aug 2022</span></div>
<div class="cv-sub"><span>Visiting Researcher, VITA Lab</span><span>Lausanne, Switzerland</span></div>
</div>

- Modified the architecture of existing deep stereo networks so they work on unrectified 360-degree camera images, after first reproducing the published benchmark results on the original datasets.
- Built a data collection rig from a lidar and two 360-degree cameras, recorded a new dataset, debugged the lidar extraction, and released the cleaned dataset in a standard format for the group.
- Ran a person detection and tracking pipeline on a Loomo mobile robot, with all inference on the onboard computer, so the robot followed a chosen person in real time.

## Education

<div class="cv-entry">
<div class="cv-head"><span class="cv-org">Sapienza Università di Roma</span><span class="cv-when">Nov 2023 – Oct 2026</span></div>
<div class="cv-sub"><span>PhD in Electronics Engineering, defence scheduled October 2026</span><span>Rome, Italy</span></div>
</div>

- Thesis: *Smart Control of Renewable Energy*

<div class="cv-entry">
<div class="cv-head"><span class="cv-org">Sapienza Università di Roma</span><span class="cv-when">Sep 2019 – Dec 2022</span></div>
<div class="cv-sub"><span>MSc in Electronics Engineering, cum laude</span><span>Rome, Italy</span></div>
</div>

- Thesis: *Depth Estimation with Stereo 360-Degree Cameras* (EPFL VITA Lab) · [project page](/projects/360-stereo-depth-estimation/)

<div class="cv-entry">
<div class="cv-head"><span class="cv-org">Babol Noshirvani University of Technology</span><span class="cv-when">Sep 2013 – Sep 2018</span></div>
<div class="cv-sub"><span>BSc in Electrical Engineering</span><span>Babol, Iran</span></div>
</div>

## Skills

<div class="cv-skills">

**Reinforcement learning:** deep RL in continuous and discrete action spaces, multi-objective and constrained RL, multi-agent RL, actor-critic and policy gradient methods, reward design, simulation environment design

**Computer vision and perception:** stereo depth estimation, 360-degree and omnidirectional imaging, object detection and tracking, lidar and camera data collection and calibration, dataset construction and cleaning, real-time inference on embedded hardware

**Machine learning:** deep learning, time series forecasting, PyTorch, TensorFlow

**Optimisation and control:** multi-objective and constrained optimisation, linear and mixed-integer programming, modelling and simulation of dynamic systems

**Energy systems:** energy management systems, battery management systems, battery dispatch, degradation modelling, renewable energy communities, smart grids

**Programming and tools:** Python, C/C++, MATLAB/Simulink, Git, Linux, bash, ROS

**Robotics and embedded:** NVIDIA Jetson, Raspberry Pi, Arduino, FPGA digital design (VHDL)

**Languages:** English (fluent), Persian (native), Italian (A2), French (A2)

</div>

## Publications

{{< publications compact="true" >}}

---

<p class="cv-note">Full CV with contact details available on request via <a href="https://www.linkedin.com/in/danialzendehdel/">LinkedIn</a>.</p>
