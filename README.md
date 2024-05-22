# Implementation of MiRo as a Baby Monitor

---

## Abstract 📜
This project presents the implementation and evaluation of MiRo as a Baby Monitor, leveraging advancements in robotics, perception, sensing, and AI technologies. The need for innovative solutions in childcare surveillance, amidst the demands of modern lifestyles, has led to the development of the MiRo Baby Monitor, offering enhanced functionalities beyond traditional baby monitors. 

**Key Highlights:**
- 🤖 Baby detection
- 🍼 Crying recognition
- 🕵️ Stranger detection
- 🎤 User interaction

---

## I. Introduction and Background 📚

Infants require constant care and supervision to ensure their safety and health. The fast-paced lifestyle of the modern day, however, warrants guardians to juggle several tasks and responsibilities. This drives the necessity for an improved system to aid in the surveillance and care of young children. The advancement of robotic technologies has motivated the innovation of several aspects of contemporary life, resulting in enhanced designs and increased ease of use. This is no exception when considering the sector of childcare.

<img src="https://github.com/William-Droin/MiRo-baby-watch/assets/72973649/d129aa25-dd73-491f-b28b-815a3c008d08" width="48">

### Traditional Baby Monitors 🖥️
Traditional baby monitors typically provide audio and visual input to the parent or guardian to allow them to have a watchful eye on the child. This is normally limited to having a single view of the baby in the crib along with a two-way communication of sound. Thus, a constant watch on the monitor is still required, and any issues require manual fixes. Baby monitors have seen improvements over the years, some implementing aspects such as live-streaming the position and activities of the baby, others implementing intelligent systems on the cradle itself, such as automated swinging capabilities or utilizing lullaby toys. However, a number of shortfalls are still present, requiring the prolonged attention and increased labor of the guardian.

---

## II. Literature Review 📖

Around 200 seemingly healthy babies die inexplicably every year in the UK from sudden infant death syndrome (SIDS), also known as “cot death”. While the statistic is relatively low, it is still alarming for parents of new babies. Considering that UK parents lose an average of 2.5 months of sleep during the first year of their child’s life, many parents invest in monitors and cams to lower their own stress levels and have some peace of mind knowing how their baby is doing when they are not in the same room.

### Evolution of Baby Monitors 🌟
The first baby monitor was invented in 1938 by Zenith president, Eugen McDonald, Jr. Called the Radio Nurse, this device allowed the transmission of audio coming from a child’s room while the parents are out of earshot. The transmitter, called the Guardian Ear, would be plugged into an electrical outlet near where the child was sleeping, and the Radio Nurse plugged into an electrical outlet near to where the parents were in the house, with the sound being transmitted through the house wiring. The design of the device was completed by Sculptor Isamu Noguchi who made the housing of the receiver look like an abstracted image of a nurse with a cap in order to soften the feeling of intrusion from a high tech device in the domestic setting.

---

## III. Objectives 🎯

To reach the desired outcome for the MiRo robot, many objectives have been chosen. The main goal for this project is to produce an innovative baby monitor that has additional features, including the ability to move, providing a more functional device. Obtaining a solution for this goal involved splitting it into parts. These parts were then split between members of the group to make the overall task more manageable. While these objectives are a requirement for the project to succeed, other paramount factors such as maintaining the baby’s safety and minimizing the risk of being hacked also need to be carefully considered.

**Main Objectives:**
1. 🍼 **Baby Detection** - MiRo detects a baby through its camera, permitting the live-streaming function.
2. 😭 **Crying Detection** - MiRo’s audio sensors distinguish when the baby is crying among other background noises.
3. 🕵️ **Stranger Detection** - Recognizing known and unknown users, alerting the carer if an unknown person is detected.
4. 🎙️ **Command Detection** - MiRo understands phrases said by users and operates accordingly.

---

## IV. Methodology 🛠️

This section should be sufficiently detailed so that an external reader can reproduce your results. It should start with the overall description of the proposed architecture and then explain each module individually by describing the methods used.

### A. Technical Structure

The project is structured in 3 main parts:
- **AI modules**: The high-level AI models running in their own Python processes.
- **Web Server**: Python Flask server acting as a pre-processing abstraction layer between MiRo and the AI modules.
- **Low Level MiRo**: ROS topics for sensors and actuators.

The design of this project is heavily inspired by the Microservice architecture from the web development world. A microservice is a small, independent unit that performs a specific task and communicates with other microservices with very specific and defined communication channels. This allows for a flexible and modular structure: one module can be easily taken out without heavy redesign of the application.

![Miro_code_graph_3 (1)](https://github.com/William-Droin/MiRo-baby-watch/assets/72973649/abd0d8fb-43d4-4f36-8563-c1cd14e649dc)


### B. Modules of the Pipeline

1. **Facial Recognition and Stranger Detection**: 
    - The video feed returned by the MiRo Baby Monitor is utilized to perform facial recognition. 
    - A histogram of oriented gradients (HOG) model is used for object detection and facial recognition.
    - This model is trained on several images of each person wished to be known. 
    - When receiving the video feed from MiRo, each frame is processed to identify faces, generating encodings that are compared to the training results. 
    - Unknown faces trigger an alert to the user.
  
<img width="1289" alt="Screenshot 2024-05-22 at 23 05 44" src="https://github.com/William-Droin/MiRo-baby-watch/assets/72973649/2b8313c5-ccba-499e-8b30-4f1cb8302227">

2. **Baby Detection**:
    - MiRo’s live video feed performs constant baby detection using a YOLO (You Only Look Once) model, optimized for speed and accuracy.
    - The model, pre-trained on the COCO dataset, is fine-tuned to detect babies with high accuracy, minimizing false alarms.
    - The system provides a visual overlay on the detected baby and alerts the parents about the baby’s location relative to the crib.

<img width="1280" alt="Screenshot 2024-05-22 at 22 49 57" src="https://github.com/William-Droin/MiRo-baby-watch/assets/72973649/80b95728-adf1-4064-8007-71d829155b59">


3. **User Interaction**:
    - MiRo can greet known users and request commands using text-to-speech technology.
    - Commands are recognized using speech-to-text technology, and MiRo can perform actions such as playing a lullaby.
    - The system confirms the command by repeating it back to the user.


4. **Audio Detection**:
    - The audio-detection module identifies baby distress signals using microphones that capture audio data at 20kHz.
    - A deep learning model trained on baby crying sounds and domestic noises processes the audio data.
    - The model converts audio clips to WAV files, extracts mel spectrograms, and returns predictions. Detected cries trigger notifications to the user.

5. **Evaluation Suite (Simulation)**:
    - A simulation environment using ROS and Gazebo was developed to test the key functionalities of MiRo.
    - Virtual representations of the robot, environment, and objects (e.g., crib) facilitate realistic testing of perception algorithms.
    - The simulation assesses baby detection and stranger detection, ensuring robust performance before real-world deployment.

<img width="1287" alt="Screenshot 2024-05-22 at 23 12 32" src="https://github.com/William-Droin/MiRo-baby-watch/assets/72973649/d7b1c3c2-6639-47a5-8e51-3c77205559cf">

---

## V. Results 📊

### A. Simulation Results

The performance of the MiRo Baby Monitor’s AI modules was evaluated through rigorous testing. The following table presents the accuracy rates and confusion matrix for each AI module:

| AI Model              | Accuracy | True Positive (TP) | False Positive (FP) | False Negative (FN) | True Negative (TN) |
|-----------------------|----------|--------------------|---------------------|---------------------|--------------------|
| Stranger Detection    | 87.9%    | 86                 | 8                   | 17                  | 97                 |
| Baby Position         | 91%      | 178                | 10                  | 0                   | 1                  |
| Understanding Commands| 78.6%    | 76                 | 27                  | 19                  | 93                 |
| Cries Perception      | 89.8%    | 101                | 8                   | 13                  | 83                 |


---

## VI. Conclusion and Future Work 🏁

The implementation of MiRo as a Baby Monitor showcases the fusion of advanced robotics, AI algorithms, and sensing technologies to create a robust childcare surveillance system. The project’s success in achieving accurate object and individual detection underscores its potential to enhance caregiver awareness and child safety significantly.

**Future Work:**
- Integration with Home Assistant systems for seamless connectivity and smart functionalities.
- Implementation of stringent safety protocols to prevent potential accidents, such as trapping fingers or collisions with objects.
- Expansion of technology to serve as a caregiving tool for the elderly, leveraging the same AI and sensing capabilities to assist in monitoring and ensuring the safety of elderly individuals.
- Enhancements in the user interface to make the system more user-friendly and accessible for caregivers of all technological proficiencies.
- Development of additional AI modules to detect other significant events, such as baby’s temperature fluctuations or unusual movements, providing even more comprehensive monitoring capabilities.
- Incorporation of multi-language support for the command detection system to cater to a diverse range of users.
- Continuous improvement of data privacy and security measures to ensure the protection of sensitive information.


---

## VII. Ethical and Societal Implications ⚖️

The integration of advanced robotic systems like MiRo as a baby monitor raises several ethical and societal considerations:

### A. Privacy and Data Security 🔒
- **Data Protection**: Ensuring that all data collected by MiRo is securely stored and transmitted, with strong encryption and access controls.
- **User Consent**: Obtaining explicit consent from users for data collection and usage, with clear communication about what data is being collected and how it will be used.
- **Transparency**: Maintaining transparency about the AI models and data processing methods used in the system.

### B. Human-Robot Interaction 🤖
- **Trust and Reliability**: Building trust in the system by ensuring high accuracy and reliability in MiRo’s performance.
- **User Education**: Providing adequate training and resources for users to understand how to effectively and safely use MiRo.

### C. Societal Impact 🌍
- **Accessibility**: Making the technology accessible to a broad range of users, including those with limited technological proficiency or resources.
- **Ethical AI**: Ensuring that the AI algorithms used in MiRo are developed and tested with ethical considerations in mind, avoiding biases and ensuring fairness.
- **Regulatory Compliance**: Adhering to relevant regulations and standards for robotic and AI systems in healthcare and childcare settings.

---

## References 📚

1. AR Telepatil, PP Patil, SS Yajare, and SR Jadhav. Intelligent baby monitoring system. International Journal of Research in Advent Technology, 7(6):191–194, 2019.
2. Waheb A Jabbar, Hiew Kuet Shang, Saidatul NIS Hamid, Akram A Almohammedi, Roshahliza M Ramli, and Mohammed AH Ali. IoT-BBMS: Internet of Things-based baby monitoring system for smart cradle. IEEE Access, 7:93791–93805, 2019.
3. Marian Willinger, L. Stanley James, and Charlotte Catz. Defining the sudden infant death syndrome (SIDS): Deliberations of an expert panel convened by the National Institute of Child Health and Human Development. Pediatric Pathology, 11(5):677–684, Jan 1991.
4. Ylva Parfitt and Susan Ayers. Transition to parenthood and mental health in first-time parents. Infant Mental Health Journal, 35(3):263–273, 2014.
5. Marc Greuther. An Industrial Design Classic: Isamu Noguchi and the Radio Nurse. The Henry Ford, 2021.
6. IKEA. UNDVIKA Baby monitor. IKEA, 2024.
7. Infant Optics. DXR-8 PRO Full Kit. Infant Optics, 2024.
8. Cubo. Cubo AI Plus Baby Monitor. 2024.
9. Mehmet Söylemez, Bedir Tekinerdogan, and Ayça Kolukısa Tarhan. Challenges and solution directions of microservice architectures: A systematic literature review. Applied Sciences, 12(11), 2022.
10. Oscar Déniz, Gloria Bueno, Jesús Salido, and Fernando De la Torre. Face recognition using histograms of oriented gradients. Pattern recognition letters, 32(12):1598–1603, 2011.
11. Paul Taylor. Text-to-speech synthesis. 2009.
12. Bhoomika Valani. donate-a-cry-corpus-features-dataset, 2023.
13. J. Salamon, C. Jacoby, and J. P. Bello. A dataset and taxonomy for urban sound research. In 22nd ACM International Conference on Multimedia (ACM-MM’14), pages 1041–1044, Orlando, FL, USA, Nov. 2014.
14. Sebastian Ruder. An overview of gradient descent optimization algorithms, 2017.
15. Mostafa Ibrahim. An introduction to audio classification with keras. Weights and Biases, 2024.
16. Chess robot grabs and breaks seven-year-old’s finger — youtube.com. https://www.youtube.com/watch?v=K5T2Odn-T-U. [Accessed 03-04-2024].

---
