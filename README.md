# Implementation of MiRo as a Baby Monitor

## Abstract
This project presents the implementation and evaluation of MiRo as a Baby Monitor, leveraging advancements in robotics, perception, sensing, and AI technologies. The need for innovative solutions in childcare surveillance, amidst the demands of modern lifestyles, has led to the development of the MiRo Baby Monitor, offering enhanced functionalities beyond traditional baby monitors. This study explores the design and methodology behind integrating MiRo’s capabilities for baby detection, crying recognition, stranger detection, and user interaction, ensuring a comprehensive monitoring system that encompasses sensing and perception. The methodology includes AI modules for facial recognition and audio detection, a web server for seamless integration, and low-level MiRo functionalities through ROS topics. A simulation environment using ROS and Gazebo was crucial for testing and evaluating MiRo’s performance in detecting babies in cribs and distinguishing between familiar and unfamiliar faces. Results indicate promising accuracy rates in baby detection, crying perception, stranger detection, and command understanding, demonstrating the efficacy of MiRo as an advanced Baby Monitor with robust sensing and perception capabilities.

## Index Terms
- Baby Monitor
- MiRo
- Perception
- Sensing

---

## I. Introduction and Background

Infants require constant care and supervision to ensure their safety and health. The fast-paced lifestyle of the modern day, however, warrants guardians to juggle several tasks and responsibilities. This drives the necessity for an improved system to aid in the surveillance and care of young children. The advancement of robotic technologies has motivated the innovation of several aspects of contemporary life, resulting in enhanced designs and increased ease of use. This is no exception when considering the sector of childcare.

Traditional baby monitors typically provide audio and visual input to the parent or guardian to allow them to have a watchful eye on the child. This is normally limited to having a single view of the baby in the crib along with a two-way communication of sound. Thus, a constant watch on the monitor is still required, and any issues require manual fixes. Baby monitors have seen improvements over the years, some implementing aspects such as live-streaming the position and activities of the baby, others implementing intelligent systems on the cradle itself, such as automated swinging capabilities or utilising a lullaby toys. However, a number of shortfalls are still present, requiring the prolonged attention and increased labor of the guardian.

This motivated the need for the MiRo Baby Monitor. This advanced robotic system provides innovative technical features allowing for a parent to keep constant watch over every part of their child’s room with MiRo-E’s movement controls and user-friendly interface. With its advanced visual processing, MiRo can let a parent know when their baby is awake and active or asleep. Its crying recognition system can also immediately alert the parent to any situation requiring attention. The MiRo baby monitor aims at keeping your child safe by utilizing stranger detection methods to alert parents of an unknown presence alone with the child. The MiRo-E is the perfect robot for the job, as its endearing animal form provides a comforting presence to the child, bringing joy and companionship. Using facial recognition and voice-activated commands, MiRo is your personalized watchful eye and helper in keeping your child safe and happy.

---

## II. Literature Review

Around 200 seemingly healthy babies die inexplicably every year in the UK from sudden infant death syndrome (SIDS), also known as “cot death”. While the statistic is relatively low, it is still alarming for parents of new babies. Considering that UK parents lose an average of 2.5 months of sleep during the first year of their child’s life, many parents invest in monitors and cams to lower their own stress levels and have some peace of mind knowing how their baby is doing when they are not in the same room.

The first baby monitor was invented in 1938 by Zenith president, Eugen McDonald, Jr. Called the Radio Nurse, this device allowed the transmission of audio coming from a child’s room while the parents are out of earshot. The transmitter, called the Guardian Ear, would be plugged into an electrical outlet near where the child was sleeping, and the Radio Nurse plugged into an electrical outlet near to where the parents were in the house, with the sound being transmitted through the house wiring. The design of the device was completed by Sculptor Isamu Noguchi who made the housing of the receiver look like an abstracted image of a nurse with a cap in order to soften the feeling of intrusion from a high tech device in the domestic setting.

This original straight-forward concept for the baby monitor has been iterated on ever since. Audio-based baby monitors are now mostly wireless, and many are two-way devices so that the parents can speak over microphone to their child to comfort them. Rechargeable audio-based monitors are affordable, such as the UNDVIKA baby monitor by IKEA retail for £12 and are functionally akin to a walkie talkie, using radio frequency to wirelessly transmit audio.

Traditional audio monitors have evolved into sophisticated systems incorporating video feeds, movement sensors, and connectivity features. These mid-range monitors, sometimes referred to as baby cams, give parents further insight into how their baby is behaving while they are out of the room. The Infant Optics DXR-8 baby cam is a popular option that retails for $199.99 USD. This baby cam offers 1000mW speakers for high volume and crisp sound playback as well as 720P video resolution playback on a portable 5” LCD display. The product boasts interchangeable lenses, swapping between an up-to-6x zoom lens and wide-angle lens, to offer parents a larger range of vision of their baby and the surrounding environment. Contemporary baby monitors often integrate wireless connectivity, enabling remote monitoring through smartphone applications, and even smart home integration. Smart baby monitors such as the CuboAi Smart Baby Monitor use AI to detect whether the baby has covered their face with a blanket or rolled over, as well as if the baby is crying. The system notifies parents via smartphone application, and also gives parents the option to play a lullaby from the device. This device can be wall-mounted or placed on a tripod, offering 360° pan rotation and 140° tilt.

The market for baby monitors is brimming with options for parents to keep track of their child remotely. A market gap exists, however, in offering mobile baby monitors. While existing monitors offer a large range of wide-lenses and 360° rotation, none offer the flexibility and versatility of having a monitor that can cover the entire range of a floorplan.

---

## III. Objectives

To reach the desired outcome for the MiRo robot, many objectives have been chosen. The main goal for this project is to produce an innovative baby monitor that has additional features, including the ability to move, providing a more functional device. Obtaining a solution for this goal involved splitting it into parts. These parts were then split between members of the group to make the overall task more manageable. While these objectives are a requirement for the project to succeed, other paramount factors such as maintaining the baby’s safety and minimizing the risk of being hacked also need to be carefully considered.

The first objective is baby detection, which involves MiRo being able to detect a baby through its camera, permitting the live-streaming function. The second is crying detection, which consists of using MiRo’s audio sensors to distinguish when the baby is crying amongst other background noises such as traffic, pets, and children playing. The third is stranger detection, which is comprised of recognizing known and unknown users. If the identity is known, then the associated name is found and returned. Whereas if the face is unknown, then an alert is sent to the carer and the data is saved. The final objective is command detection, which allows MiRo to understand phrases said by users and operate accordingly to them.

The report will be structured in the following manner:
- Section IV sets out the methodology for the project.
- Section V details the results and provides an analysis of the findings.
- Section VI consists of an overview of the report and potential future work.

---

## IV. Methodology

This section should be sufficiently detailed so that an external reader can reproduce your results. It should start with the overall description of the proposed architecture and then explain each module individually by describing the methods used.

### A. Technical Structure

The project is structured in 3 main parts:
- AI modules: The high level AI models running in their own Python processes.
- Web Server: Python Flask server acting as a pre-processing abstraction layer between MiRo and the AI modules.
- Low Level MiRo: ROS topics for sensors and actuators.

The design of this project is heavily inspired by the Microservice architecture from the web development world. A microservice is a small, independent unit that performs a specific task and communicates with other microservices with very specific and defined communication channels. This allows for a flexible and modular structure: one module can be easily taken out without heavy redesign of the application.

On the following figure, the different modules can be clearly identified as well as their respective mode of communication. This design allows for any part of the project to be altered, added, or deleted without breaking the entire system as long as all the communication channels are respected. This enables for easier addition of new AI features in the future or easily updating existing ones without any interdependency. Collaboration is also made easier since modules can be split between teams and each team won’t need to be aware of other modules’ implementation, just how to communicate with them.

![Code Structure and Communication Channels](path_to_figure1.png)

### B
