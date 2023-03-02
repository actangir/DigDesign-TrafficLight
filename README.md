# DigDesign-TrafficLight
## Digital Design and Implementation of 4-way Traffic Lights
## Anjali Tangirala

### Project Overview
In order to ensure the safety of all vehicles, we must rigorously prove that our logic, design, and implementation avoid all possible dangers. Several conditions must be met to guarantee the safety of the intersection. The first major step in any problem is to define the hazards and/or states that we must avoid in implementation. The biggest hazard in this situation is accidents based on improper signals. This means that we can never have the two lights be green at the same time. However, we must also ensure that cars are able to pass through the intersection based on the weather. Thus, the lights will need to alternate green, and must both be red in transition if the weather allows it. We have information coming from the traffic lights that will be useful in determining the state of each traffic light as well. This information comes from 2 sensors, the weather, and the vehicle counting sensor as well as a sensor enable that corresponds to the state of the vehicle counting sensor. We also know that the country road also must be closed when it is snowing to prevent accidents, so in that situation, only the highway road light will stay green. Now that we have defined the parameters of our problem, let us now discuss how to implement it in the prototype as well as using an HDL.
  #### Background Concepts
  * System Verilog Behavioral/Procedural Code
  * K-Maps
  * Transition Tables
  * Transition Equations
  * Combinational Logic
  * General Knowledge of Digital Systems

  #### Tools and Resources
  * D-FF
  * OR gate
  * 4 AND gates
  * 3 resistors
  * 3 LEDs
  * 3 NOT gates
  * Wires
  * EDA Playground(Yosys 0.9.0,Icarus 0.10.0)
  * Breadboard
  * MyDAQ with Oscillator and Function Generator
    
  #### Design
  
   ![State Diagram](https://github.com/cse241-SP22/final-project-actangir/blob/main/CSE%20241%20Final%20Project%20Anjali%20Tangirala.png)
      
In the state diagram above, we have our state representations with their corresponding transitions based on our weather sensor input x. We can see that when the weather sensor if off i.e., x=0, our lights alternate between green and red.  The green-colored states represent one of the two lights being green, while the red-colored states represent both lights turning red. The reason behind having two red light states is because each one represents the red light transition before either the country or highway road light is green. Furthermore, the red light labeled as state C is the precursor to the country road turning green i.e., state D while the other state where both lights are red is labeled as state A, and is a precursor to the highway light being green i.e., state B. This alternating behavior occurs when it is not snowing, and therefore our lights rotate between red, highway green, red, country green, and back again. Our other behavior is when it is snowing, which results in the country road closing and never having a green light. This means that the input x which represents our weather sensor is returning a 1, and if we are in any state other than B, we must return to state B and stay there with the country road closed until our weather sensor tells us it has stopped snowing and returns a 0. Obviously, if we are at the position in our pattern where our country road light is green, we must have a red light in transition to the highway road light turning green. This is all represented very concisely in the diagram below. The outputs described in the state bubbles labeled as z1, and z2 represent the light color of the highway and country road light respectively with 1 meaning the light is green and 0 meaning the light is red. This style of outputs being dependent only on the present state is indicative of the Moore style of sequential systems.
      
  #### State Table
      
 ![State Table](https://github.com/cse241-SP22/final-project-actangir/blob/bf5ef24cf47d58f0dd48078506b6518cf6241139/Capture.PNG)
      
  #### State Assignments
      
  ![State Assignments](https://github.com/cse241-SP22/final-project-actangir/blob/c011ea4c842fc5a083672de4e3fddba563cb0ff7/StateAssignments.PNG)
      
  #### Transition/Excitation Table
      
   ![Transition Table](https://github.com/cse241-SP22/final-project-actangir/blob/c011ea4c842fc5a083672de4e3fddba563cb0ff7/TransitionTable.PNG)
       
  #### Transition Equations & Output Equations
      
   ![Transition Equations](https://github.com/cse241-SP22/final-project-actangir/blob/c011ea4c842fc5a083672de4e3fddba563cb0ff7/TransitionEquations.PNG)
       
  #### Schematic
   ![Schematic](https://github.com/cse241-SP22/final-project-actangir/blob/c011ea4c842fc5a083672de4e3fddba563cb0ff7/Schematic.PNG)
  #### <ins> System Verilog</ins>
  #### Design file
   ![System Verilog](https://github.com/cse241-SP22/final-project-actangir/blob/c011ea4c842fc5a083672de4e3fddba563cb0ff7/sv1.PNG)
   ![System Verilog Cont'd](https://github.com/cse241-SP22/final-project-actangir/blob/c011ea4c842fc5a083672de4e3fddba563cb0ff7/sv2.PNG)
  #### Testbench file
   ![System Verilog Testbench 1](https://github.com/cse241-SP22/final-project-actangir/blob/c011ea4c842fc5a083672de4e3fddba563cb0ff7/sv3.PNG)
   ![System Verilog Testbench 2](https://github.com/cse241-SP22/final-project-actangir/blob/c011ea4c842fc5a083672de4e3fddba563cb0ff7/sv4.PNG)
  #### Testing
  There is many situations where our circuit could malfunction or not work according to our specifications. In order to assure the safety of our intersection, we will be listing 8 possible cases in which our circuit could go wrong that will need testing for this system.
The first situation is if our weather sensor enables while our country road light is on. We must verify that there will be a transition between our country road light being green and the highway light being green.
We will need to verify the functionality of our system if the highway road light breaks, and what state the intersection will remain in, in this case. In the situation that it is not snowing, then we must verify our country road will remain green so that traffic can continue and that both lights will not remain red indefinitely.
Furthermore, we must check that our system works if the country road light breaks. In this situation, it would be similar to if the weather sensor was enabled and our highway road light would remain green.
Additionally, we must factor into the situation that our weather sensor enable could break. This would result in no input to trigger the transition, which would imply that the system would stay in the same state. We would need a default value for if the weather sensor broke, say perhaps with a default value of 1 as if it was snowing. Better to be safe than sorry in the case of weather.
Next, we should consider the situation in which our weather sensor becomes enabled when the highway light is green. We want to verify that our highway light remains green, and does not alternate to the state of both lights red unnecessarily.
We next want to check the edge case of what happens if our weather sensor becomes enabled when the country road light has just turned green. In this situation, it would be ideal for the country road light to become red immediately, and turn red before allowing the highway light to become green rather than waiting for the next tick in the clock.
We also want to check what happens if our vehicle counting sensor breaks, that the system continues to run smoothly, and alternates based on the weather sensor. 
We want to verify that our system continues to run smoothly if the sensor enables breaks, our system will continue to function based on the weather sensor input.
The most critical cases are if the weather sensor or green lights break because this could cause serious detriment to the integrity of the system whose states are highly dependent on their functionality.
 In order to test the physical implementation you will need to connect the myDAQ to your input and ground/power supply. You will also need to connect the clock and clear pins of the D-FF IC to A0 and ground respectively. Then you will be able to see if the lights alternate based on desired specifications.
  #### Synthesis results
  ![Synthesis Results](https://github.com/cse241-SP22/final-project-actangir/blob/8a2efc40f2ad93520d48478352da8e8f85598f20/Synthesis%20Results.PNG)
  #### Simulation results
   ![Simulation Results](https://github.com/cse241-SP22/final-project-actangir/blob/8a2efc40f2ad93520d48478352da8e8f85598f20/simulation%20results.PNG)
 When analyzing the accuracy and efficiency of our system, my first step would be to look at the log file of inputs/outputs and determine if their behavior corresponded to the desired behavior exactly. This would eliminate any errors in our combinational logic. Next, I would look at the waveform of inputs/outputs in order to determine if there were any dynamic or static hazards that would not appear in the logs, but could still result in inadequate performance in our system. Finally, I would watch video recordings of the intersection in all different sorts of conditions to visually confirm that they are allowing the correct amount of traffic to flow and not impeding it. I would also use these to confirm their functionality realistically when it snowed in order to determine if the behavior we programmed was optimal for this application, or if another state could be added to better optimize its functionality. After all these modes of analysis were complete, I would feel comfortable stating that the system is functioning within the given parameters.
  #### Resources
##### This project was designed and implemented by Anjali Tangirala in Spring 2022 for CSE241 at the University at Buffalo. 
