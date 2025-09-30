# Cat Animation with No Code

## Set up the animations

2. Slice them in the Sprite Editor if needed.  You can just open each in the Sprite Editor and verify the frames look correct.
>[!WARNING]
> Some animations, like Sleep need to sliced as a 32x32 animation. Unity does not do this correctly automatically. For my version of Unity this was the only one that needed manual slicing.
5. Import your sprite sheets into the project by dragging them from the **Project** ```Sprites``` folder into the hierachy one at a time.

### Create Animation Clips:

1. Expand each sprite sheet in the **Project** window (click the small triangle next to the sheet).
2.	Select all frames for one animation (e.g., all Idle frames).
3.	Drag them into the **Hierarchy** (drop onto the Scene).
4.	Unity will pop up a dialog: “Save new Animation Clip”.
5. Save as Cat_Idle.anim for example for the Idle animation.
>[!NOTE]
> Unity will also auto-create an Animator Controller and assign it.
6. Repeat for the following. (each time drag the right set of frames, then save the clip).
	* Idle
	* Sleepy
	* Sleep
	* Dance
	* Surprised
	* Laydown

## Build the Animator state machine
1. In the **Project** window, right-click and select ```Create → Animator Controller```.
<img width="792" height="285" alt="Screenshot 2025-09-30 at 10 49 20 AM" src="https://github.com/user-attachments/assets/737e974f-ceb9-46e8-89de-a0c422b83457" />

2. Name it ```CatAnimator```
3. In the **Hierarchy** select all animation state game objects except Idle and right click and select delete to remove them from the hierachy.
4. Select the Idle animation.  Drag the CatAnimator Animator Object from the **Project** pane to the Controller field of the Idle animation in the **Inspector**
<img width="565" height="75" alt="Screenshot 2025-09-30 at 11 14 45 AM" src="https://github.com/user-attachments/assets/d1cafad3-fd4e-4c41-9af7-aac2d9c0fb9e" />



### Adding Animation States
1.	In the **Project** pane double click the CatAnimator controller which will bring up the **Animator** pane
<img width="669" height="216" alt="Screenshot 2025-09-30 at 10 54 09 AM" src="https://github.com/user-attachments/assets/97746d6e-a9d8-454f-bfd5-884744b82609" />

2. In the **Animator** window, right-click → ```Create State → Empty``` (or drag an animation clip in directly).
3. If you dragged a clip, the state will automatically use that clip.
4. Repeat for each animation (Idle, Sleepy, Sleep, Dance, Surprised, Laydown).
5. Right-click Idle and select ```Set as Layer Default State``` (it turns orange).
<img width="1044" height="376" alt="Screenshot 2025-09-30 at 10 57 03 AM" src="https://github.com/user-attachments/assets/5a40f90e-c4ed-4402-9302-71453ca6d4ee" />

6.  Right click the Idle animation and select ```Make Transition```
7.  Click on the Sleepy animation to make a transition from Idle to Laydown.
8.  Right click the Sleepy animation and select ```Make Transition```
7.  Click on the Idle animation to make a transition from Laydown to Idle.
9.  Add the rest of the transitions until your state diagram looks like the following:
<img width="1044" height="265" alt="Screenshot 2025-09-30 at 11 02 45 AM" src="https://github.com/user-attachments/assets/6bedd47f-9425-404a-b3cd-bc52d301a2bc" />

### Add parameters
1. In the Animator window, click the Parameters tab.
<img width="462" height="211" alt="Screenshot 2025-09-30 at 11 03 32 AM" src="https://github.com/user-attachments/assets/55a5daf7-3bed-459c-bc94-f165547996f7" />
   
2. Add six ```Triggers``: MouseMoved, NoMouse30s, NoMouse40s, Dance, Laydown, Surprised
* (Optional) An int called RandomChoice if you want to handle random rolls.
3.	Use these parameters in transition conditions. Example:
4. In the **Animator** pane click a transition. It will turn blue
  <img width="661" height="105" alt="Screenshot 2025-09-30 at 11 06 55 AM" src="https://github.com/user-attachments/assets/ff6217a6-44d3-4ad1-8119-728a75719f96" />

5. In the **Inspector** click + on Conditions and set the ```Conditions``` to NoMouse30s.
6. Set the ```Transition Conditions``` from Sleepy to Sleep to the condition NoMouse40s trigger.
7. Set the ```Transition Conditions``` from Sleep to Idle to the condition MouseMoved trigger.
8. For the remaining transitions leading to Idle add a condition ```MouseMoved```
9. For the rest of the transitions from Idle add a condition ```IdleRoll``

## Enable Visual Scripting
1.	Go to Window → Visual Scripting -> Graph Window.
2. In the **Hierarchy** select your Cat GameObject, it may still be named Idle_0
3. In the **Inspector** click ```Add Component -> Visual Scripting -> Script Machine```.
4. In the **Inspector** set Source = Graph.
5. In the **Inspector** click New to create a new graph asset (e.g., CatLogic).

## Detecting Mouse Movement
1. Click ```Edit Graph```
2. Inside your graph:
3.	Add a Update unit (runs every frame).
4.	Right click to bring up the ```Node``` context menu and search ```GetMousePosition```.
5.	Add Get Mouse Position.
6. In the ``Script Graph``` pane create a new Graph Variable called ```lastMousePosition```
  <img width="715" height="412" alt="Screenshot 2025-09-30 at 11 29 07 AM" src="https://github.com/user-attachments/assets/89452b5e-dcd0-46ac-b0b0-d748359f6540" />

7. In the graph right click and add a ```Vector 3 Distance``` node.  It's Ok that the variable is Vector 2 and this is a Vector 3 node.
8. In the graph right click and add a ```Get lastMousPosition``` node.
9. Connect the output of ```Get Mouse Position``` to input B of ```Distance```
10. Connect the output of ```Get Variable lastMousePosition``` to input A of ```Distance```
<img width="456" height="330" alt="Screenshot 2025-09-30 at 11 35 01 AM" src="https://github.com/user-attachments/assets/cbdea42e-cf98-4829-a3c7-ee968e365d99" />

11. Construct the rest of the graph:
<img width="2735" height="1004" alt="Screenshot 2025-09-30 at 12 56 42 PM" src="https://github.com/user-attachments/assets/6d9b7810-c390-4d1d-88f1-c423c6427c19" />
