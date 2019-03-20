---
title: The Maze
category: "Project"
cover: cover.jpg
author: oliver cheok
---
#Abstract:  
The Maze is developed for Google VR Cardboard. It requires the user to navigate across a maze with the use of Waypoints.

#Introduction:  
This is my first project that I have built on Android for Udacity's VR Foundation course.
I will be creating a fully interactive VR experience in the form of a maze where I can focus what I have learnt throughout the course and bring a full audio visual experience together.   

#Outcome
This project will focus on learning to code the scripts that bring everything to life.
Such skills include:
* Creating new C# scripts in Unity (If then, loops, arrays, and other programming constructs)
* Attaching scripts to objects by Using the built-in Monobehaviour methods
* Triggers and Gaze Based Interaction
* Creating, moving and animating objects procedurally
* Familiarization with the Unity documentation
* Scripting Dynamic UI Objects
* Debugging
* The Unity Event System.
* Managing and Reloading scenes.
* Controlling particle systems.
* Create an Audio Clip and playing sounds.
* Waypoint Navigation System.
* Profiling scenes for performance.  

![](./lens.jpg 'Start position of Player')

###Creating the GVR Camera Rig
During this step we will create the VR camera by including the GvrEditorEmulator in the scene and configuring the camera.
This helps us to dive into the game without wearing the google cardboard by simulating actual head movement.
1. Add the GvrEditorEmulator prefab to the scene.
2. Make the Main Camera game object a child of the GvrEditorEmulator game object.
3. Reset the Main Camera game object's Transform component.
4. Verify that the Main Camera game object has the Tag MainCamera.
5. Move the GvrEditorEmulator game object to a convenient location for development, for example, Position: 0, 3, 35 and Rotation: 0, 180, 0.
6. Enter game mode and use Alt + Mouse movement and Ctrl + Mouse movement to rotate and tilt the camera viewing angle.

![](./gvreditoremulator.gif 'Simulation of head rotation')

###Preparing the scene for interaction
During this step we will prepare the scene for interaction by setting up the reticle pointer, physics raycaster, and event system, and then test the included waypoint system.
1. Add the GvrReticlePointer prefab to the scene as a child of the Main Camera game object.
2. Increase the Max Reticle Distance value for the GvrReticlePointer from the default 10 to 20.
3. Add the GvrPointerPhysicsRaycaster script as a component on the Main Camera game object.
4. Add the GvrEventSystem prefab to the scene.
5. Enter game mode and navigate the scene by clicking on the waypoints.

![](./gvrreticlepointer.gif 'Waypoint teleportation')

##Animations
During this step we will make the Coin, Key, and Door interactive by adding script and event trigger components to them.

1. Locate and select the Coin game object in the hierarchy.
2. Verify that it has a Collider component.
3. Add the provided Coin script as a component.
4. Add an Event Trigger as a component.
5. Add the PointerClick event to the Event Trigger component.
6. Assign the Coin script component to the object field of the Pointer Click event.
7. Assign the Coin.OnCoinClicked() method as the function for the Pointer Click event.
8. Enter game mode, click on the coin and verify that the message 'Coin.OnCoinClicked()' was called is printed to the Console window.
9. Repeat the same process for the Key game object but use the Key script and the Key.OnKeyClicked() method.
10. Repeat the same process for the first parent Door game object but use the Door script and the Door.OnDoorClicked() method.

###Programming the Coin Behaviour
During this step we will program the Coin behaviour so that when a Coin is clicked it plays a sound, displays a 'poof' effect, and disappears.

![](./coin_animation_components.jpg 'Coin script with Event Trigger')

![](./particle_system.jpg 'Particle System that displays the poof effect')


```csharp
	void Update()
	{
		transform.Rotate(0, rotationSpeed * Time.deltaTime, 0 , Space.Self);		
	}

	public void OnCoinClicked() {
		// Instantiate the CoinPoof Prefab where this coin is located
		Debug.Log("Coin clicked");
        // Rotates coin in the air before Poofing
		rotationSpeed = 2000f;
		Invoke("CoinCollected", 1f);
	}

	void CoinCollected() {
		Instantiate(PoofPrefab, transform.position, Quaternion.identity);
		count++;
		Debug.Log ("count: " + count);
		signpost.SetCollectedText ();
		Destroy(this.gameObject);
	}
```

###Programming the Key Behaviour
Similar to the coin, when the Key is clicked it plays a sound, displays a 'poof' effect, and disappears.
We can use the same steps above and apply it to the key.

###Programming the Door Behaviour
During this step we will program the Door behaviour so that when the Key is collected the Door is unlocked. And when the Door is clicked it plays a sound and starts rotating to an open position. In addition, users who try to open door without having the key collected hear a locked sound.


Add an AudioSource component to the first parent Door game object and disable Play On Awake.
Assign the Door_Opening audio to the AudioClip field.

```csharp
    void Update() {
       	// If the door is opening and it is not fully raised
		if ((transform.position.y < 7.8f) & (opening == true)) {
				// Animate the door raising up
				transform.Translate(0, 2.5f *Time.deltaTime, 0, Space.World);
			}
    }

	public void OnDoorClicked() {
		// If the door is clicked and unlocked
		if (locked == false) 
		{
			opening = true;
			Debug.Log ("Opening.");
			AudioSource.PlayClipAtPoint(soundFiles [1], new Vector3 (0, 1, 4), 1);	
		}
		else 
		{
			//Play a sound to indicate the door is locked	
			AudioSource.PlayClipAtPoint(soundFiles [0], new Vector3 (0, 1, 4), 1);
			Debug.Log("Door is locked");
		}
    }

    public void Unlock()
    {
		locked = false;
    }
```

![](./door_trigger.jpg 'Door script with Event Trigger')


##Video of my project:
`youtube: https://youtu.be/zYi4HsCm9SM`

