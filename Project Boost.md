# Project Boost

Player Experience:

- Precision, Skillful

Core Mechanic:

- Skillful fly spaceship and avoid environment hazards

Core Game Loop

- Get from A to B to complete the level, then progress to the next level

Game Flow And Screens

Game Theme (ie. Story & Visuals)

- Experimental early generation spacecraft.
- On an unknown planet, trying to escape

## What's Your Theme?

- Think of a central theme for your game.
- Decide on a starting name for your game.
- Share with the community!

## Onion Design

Common Development Questions

- What features should I include in my game?
- Where should I start development?
- What are my priorities?
- What if I run out of time?
- When should I stop?

![img](https://cdn-images-1.medium.com/max/1200/1*XpM855EICSYvysyGTbDP1A.png)

![img](https://cdn-images-1.medium.com/max/1200/1*CJMA1QbTVEz_efgkt4Irxw.png)

## Sketch Our Onion Design

- What do you believe is the single most important feature of our game?
- What is next most important?

![img](https://cdn-images-1.medium.com/max/1200/1*uYgC7_SabRyigvtt_Rxdzw.png)

# Unity Units

![img](https://cdn-images-1.medium.com/max/1200/1*pwn_Wli4g4-Hp7eY9qYsgw.png)

+X = right

+Y =up

+Z=forward

## Build Our Starting Pieces

**Position**: (0, -15, 0)

**Scale**: (100, 30, 20)

- Create the items from just 1 primitive each (later we'll make them more complex if need be):
  - Ground
  - Launch Pad
  - Landing Pad
  - Rocket

SampleScene --> Sandbox

## Introducing Classes

- Classes are used to organize our code
- Classes are "containers" for variables and methods that allow us to group similar things together
- Usually we aim for a class to do one main thing and not multiple things
  - Easier to read our code
  - Easier to fix issues
  - Easier to have multiple people work on a project

- Classes we create
  - We tend to create a new class each time we create a new script and have that script responsible for one thing. Eg:
    - Movement
    - CollisionHandler
    - Shooting
    - Score
    - EnemyAI

![img](https://cdn-images-1.medium.com/max/1200/1*UE0wZZLlITiG7jgKd7Hhvw.png)

- Cluttered code is like a cluttered desktop

- Also risky because everything can access everything else

## Encapsulating Our Code

- Where possible we encapsulate our code;
  - En-capsule-ating - putting in a capsule
- Think of the different parts of your code as having a **need to know basis level of access**
  - le. Don't let everything access everything else.
- For example, only the Methods in our Movement Class can alter our player's movement speed.

## Classes Already Built For Us

- Unity has many Classes (containing useful methods and variables) already created for us.
  - By accessing the class, we access its content.

- In our code we write the class name then use the `dot` operator to access things in the class:
  - **ClassName.MethodName()**

## Basic Input Binding

- Replace the "somethings" with code
- When we push "A" then print "rotate left"
- When we push "D" then print "rotate right"
- Identify the bug / issue with our logic

```c#
// movement.cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movement : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
            
    }

    // Update is called once per frame
    void Update()
    {
        ProcessThrust();
        ProcessRotation();
    }

    void ProcessThrust()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            Debug.Log("Pressed Space - Thrusting");
        }
    }

    void ProcessRotation()
    {
        // if ?????? ?????? ?????? else if??? ?????????
        // A, B??? ????????? ???????????? ????????? ???????????????
        if (Input.GetKey(KeyCode.A))		
        {
            Debug.Log("Rotating Left");
        }

        else if (Input.GetKey(KeyCode.B))
        {
            Debug.Log("Rotating Right");
        }
    }
}
```

## Using RelativeForce()

- Rocket??? rigidbody ??????

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movement : MonoBehaviour
{
    Rigidbody rb;
    // Start is called before the first frame update
    void Start()
    {
        // Caching
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void Update()
    {
        ProcessThrust();
        ProcessRotation();
    }

    void ProcessThrust()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            // Debug.Log("Pressed Space - Thrusting");
            // Vector3.up = (0, 1, 0) ??? ??????
            rb.AddRelativeForce(0, 4, 0);
            // rb.AddRelativeForce(Vector3.up);
        }
    }

    void ProcessRotation()
    {
        // if ?????? ?????? ?????? else if??? ?????????
        // A, B??? ????????? ???????????? ????????? ???????????????
        if (Input.GetKey(KeyCode.A))
        {
            Debug.Log("Rotating Left");
        }

        else if (Input.GetKey(KeyCode.B))
        {
            Debug.Log("Rotating Right");
        }
    }
}
```

## Variable For Thrusting

- Add a variable so we can tune the main rocket thrust
- Make the force applied frame rate independent

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movement : MonoBehaviour
{
    [SerializeField] float mainThrust = 100f;
    Rigidbody rb;
    // Start is called before the first frame update
    void Start()
    {
        // Caching
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void Update()
    {
        ProcessThrust();
        ProcessRotation();
    }

    void ProcessThrust()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            // Debug.Log("Pressed Space - Thrusting");
            // Vector3.up = (0, 1, 0) ??? ??????
            rb.AddRelativeForce(Vector3.up * mainThrust * Time.deltaTime);
        }
    }

    void ProcessRotation()
    {
        // if ?????? ?????? ?????? else if??? ?????????
        // A, B??? ????????? ???????????? ????????? ???????????????
        if (Input.GetKey(KeyCode.A))
        {
            Debug.Log("Rotating Left");
        }

        else if (Input.GetKey(KeyCode.B))
        {
            Debug.Log("Rotating Right");
        }
    }
}
```

## Transform.Rotate() Our Rocket

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movement : MonoBehaviour
{
    [SerializeField] float mainThrust = 100f;
    Rigidbody rb;
    // Start is called before the first frame update
    void Start()
    {
        // Caching
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void Update()
    {
        ProcessThrust();
        ProcessRotation();
    }

    void ProcessThrust()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            // Debug.Log("Pressed Space - Thrusting");
            // Vector3.up = (0, 1, 0) ??? ??????
            rb.AddRelativeForce(Vector3.up * mainThrust * Time.deltaTime);
        }
    }

    void ProcessRotation()
    {
        // if ?????? ?????? ?????? else if??? ?????????
        // A, B??? ????????? ???????????? ????????? ???????????????
        if (Input.GetKey(KeyCode.A))
        {
            // (0, 0, 1)
            transform.Rotate(Vector3.forward);
        }

        else if (Input.GetKey(KeyCode.D))
        {
            transform.Rotate(-Vector3.forward);
        }
    }
}
```

Tuning Our Rocket's Rotation

- Add a variable so we can tune the rotation speed
- Make the rotation speed frame rate independent

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movement : MonoBehaviour
{
    // ????????? ??????
    [SerializeField] float mainThrust = 100f;
    [SerializeField] float rotationThrust = 1f;
    Rigidbody rb;
    // Start is called before the first frame update
    void Start()
    {
        // Caching
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void Update()
    {
        ProcessThrust();
        ProcessRotation();
    }

    void ProcessThrust()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            // Debug.Log("Pressed Space - Thrusting");
            // Vector3.up = (0, 1, 0) ??? ??????
            rb.AddRelativeForce(Vector3.up * mainThrust * Time.deltaTime);
        }
    }

    void ProcessRotation()
    {
        // if ?????? ?????? ?????? else if??? ?????????
        // A, B??? ????????? ???????????? ????????? ???????????????
        if (Input.GetKey(KeyCode.A))
        {
            ApplyRotation(rotationThrust);
        }

        else if (Input.GetKey(KeyCode.D))
        {
            ApplyRotation(-rotationThrust);
        }
    }

    void ApplyRotation(float rotationThisFrame)
    {
        transform.Rotate(Vector3.forward * rotationThisFrame * Time.deltaTime);
    }
}
```

# Rigidbody Constraints

Tidy Up Our Movement

- Apply changes to prefab
- Add an obstacle
- Freeze constraints for our rocket
- Fix our obstacle-hitting bug

- Set our drag value

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movement : MonoBehaviour
{
    // ????????? ??????
    [SerializeField] float mainThrust = 100f;
    [SerializeField] float rotationThrust = 1f;
    Rigidbody rb;
    // Start is called before the first frame update
    void Start()
    {
        // Caching
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void Update()
    {
        ProcessThrust();
        ProcessRotation();
    }

    void ProcessThrust()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            // Debug.Log("Pressed Space - Thrusting");
            // Vector3.up = (0, 1, 0) ??? ??????
            rb.AddRelativeForce(Vector3.up * mainThrust * Time.deltaTime);
        }
    }

    void ProcessRotation()
    {
        // if ?????? ?????? ?????? else if??? ?????????
        // A, B??? ????????? ???????????? ????????? ???????????????
        if (Input.GetKey(KeyCode.A))
        {
            ApplyRotation(rotationThrust);
        }

        else if (Input.GetKey(KeyCode.D))
        {
            ApplyRotation(-rotationThrust);
        }
    }

    void ApplyRotation(float rotationThisFrame)
    {
        rb.freezeRotation = true; // freezing rotation so we can manually rotate
        transform.Rotate(Vector3.forward * rotationThisFrame * Time.deltaTime);
        rb.freezeRotation = false; // unfreezing rotation so the physics system can take over
    }
}
```

?????? ?????? **Overrides**??? ???????????? Apply All ????????? ????????? ????????? ????????? Rigidbody ?????? ?????? ???????????? ?????? ????????? ??? ??????.

**Rocket**??? **Rigidbody ==> Constraints ==> Freeze Position (Z) + Rotation (X, Y)**

**Drag: ?????????. ???????????? ?????? ?????????=????????? ????????? ?????? ??? ???.**

**(0?????? ??????????????? 0?????? ?????????(INF)??? ???????????? ?????? ?????? ????????? ?????? ???????????? ?????? ??? ???)**

**Drag**: 0.25

**Assets --> Project Settings --> Physics --> Gravity (y: -4)**??? ???????????? ?????? ??? ?????? ???????????? ????????? ?????? ??? ??????.

## Unity Audio Introduction

https://docs.unity3d.com/ScriptReference/AudioSource.html

https://freesound.org/

https://www.audacityteam.org/download/

![img](https://cdn-images-1.medium.com/max/1200/1*hTIaErAMr8atwsGe8ylIVA.png)

1. **Audio Source**??????

- https://freesound.org/
  - Search For Rocket Boost
- https://www.audacityteam.org/download/
  - Make your own sound - Export Audio (????????? ????????? ??????????????? ????????????)

2. **Audio** ????????? ??? ????????? **Project Settings**??? ?????? **Audio** ?????? **System Sample Rate**??? 1??? ????????? ??????.

## Play AudioSource SFX

Play SFX When Thrusting

- Cache a reference to AudioSource called **audioSource**
- Use `audioSource.play()` to play when we are thrusting
- Use `!audioSource.isPlaying` to make sure we only play if we aren't already playing 
- Use an `else` condition and `audioSource.Stop()` to stop our SFX when we aren't thrusting

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movement : MonoBehaviour
{
    // ????????? ??????
    [SerializeField] float mainThrust = 100f;
    [SerializeField] float rotationThrust = 1f;
    Rigidbody rb;
    AudioSource audioSource;
    // Start is called before the first frame update
    void Start()
    {
        // Caching
        rb = GetComponent<Rigidbody>();
        audioSource = GetComponent<AudioSource>();
    }

    // Update is called once per frame
    void Update()
    {
        ProcessThrust();
        ProcessRotation();
    }

    void ProcessThrust()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            rb.AddRelativeForce(Vector3.up * mainThrust * Time.deltaTime);

            if (!audioSource.isPlaying)
            {
                audioSource.Play();
            }
        }

        else
        {
            audioSource.Stop();
        }
    }

    void ProcessRotation()
    {

        if (Input.GetKey(KeyCode.A))
        {
            ApplyRotation(rotationThrust);
        }

        else if (Input.GetKey(KeyCode.D))
        {
            ApplyRotation(-rotationThrust);
        }
    }

    void ApplyRotation(float rotationThisFrame)
    {
        // freezing rotation so we can manually rotate
        rb.freezeRotation = true;
        transform.Rotate(Vector3.forward * rotationThisFrame * Time.deltaTime);
        // unfreezing rotation so the physics system can take over
        rb.freezeRotation = false; 
    }
}
```

## Switch Statements

```c#
switch (variableToCompare)
{
    case valueA:
        ActionToTake();
        break;
    case valueB:
        ActionToTake();
        break;
    default:
        YetAnotherAction();
        break;
}
```

**Print Different Collision Situations**

- Using a Switch Statement, print to the console different messages based on what we collide into
- Remember that our collision tag returns a `string`

- Our variable will be: `other.gameObject.tag`

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CollisionHandler : MonoBehaviour
{
    void OnCollisionEnter(Collision other)
    {
        switch (other.gameObject.tag)
        {
            case "Land":
                Debug.Log("This thing is landing area");
                break;
            case "Finish":
                Debug.Log("Congrats, you finished");
                break;
            case "Fuel":
                Debug.Log("You Picked up fuel");
                break;
            default:
                Debug.Log("Soory, you blew up!");
                break;
        }
        
    }
}
```

## Respawn Using SceneManager

https://docs.unity3d.com/ScriptReference/SceneManagement.SceneManager.html

**File** ==> **Build Settings** ==> **Add Open Scenes**??? ????????? ??????.

??? ?????? ????????? ????????? ?????????????????? ?????? ?????? ??????????????? ???, ????????? ????????? ???????????? ?????? ????????????.

**Reload Current Scene**

- Use `SceneManager.LoadScene()` to load the current scene and therefore respawn our rocket ship when it collides with the ground
- You'll need to use the `UnityEngine.SceneManagement` namespace

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine.SceneManagement;
using UnityEngine;

public class CollisionHandler : MonoBehaviour
{
    void OnCollisionEnter(Collision other)
    {
        switch (other.gameObject.tag)
        {
            case "Land":
                Debug.Log("This thing is landing area");
                break;
            case "Finish":
                Debug.Log("Congrats, you finished");
                break;
            case "Fuel":
                Debug.Log("You Picked up fuel");
                break;
            default:
                // Debug.Log("Soory, you blew up!");
                ReloadLevel();
                break;
        }
    }

    void ReloadLevel()
    {
        // SceneManager.LoadScene(0);
        // ?????? ????????? ?????? ?????????
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        SceneManager.LoadScene(currentSceneIndex);
    }
}
```

# Load Next Level

- When we reach the Landing Pad, load the next level.
- Hint: our scene index is an integer, therefore we can add a number to it.
- Please go to Window > Rendering > Lighting Settings. At the bottom of the Inspector, click on "Generate Lighting". You could also try to disable "auto-generate lighting".

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine.SceneManagement;
using UnityEngine;

public class CollisionHandler : MonoBehaviour
{

    void OnCollisionEnter(Collision other)
    {
        switch (other.gameObject.tag)
        {
            case "Start":
                Debug.Log("??????");
                break;
            case "Finish":
                Debug.Log("??????");
                LoadNextLevel();
                break;
            case "Fuel":
                Debug.Log("????????????");
                break;
            default:
                Debug.Log("????????? ?????? ??????");
                Invoke("StartCrashSequence", 1f);
                break;
        }
    }

    void StartCrashSequence()
    {
        GetComponent<Movement>().enabled = false;
        ReloadLevel();
        GetComponent<Movement>().enabled = true;
    }
    
    void LoadNextLevel()
    {
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        int nextSceneIndex = currentSceneIndex + 1;

        // sceneCountInBuildSettings ??? ?????? Scene??? ??????????????? ????????????
        // current??? 0??????, ?????? Scene??? ???????????? ?????????, 
        // ??? Scene??? index??? 0?????? ????????? 0?????? ?????????.
        // ???, ????????? 1?????? ????????? ???????????? 0?????? ????????????.
        if (nextSceneIndex == SceneManager.sceneCountInBuildSettings)
        {
            nextSceneIndex = 0;
        }

        SceneManager.LoadScene(nextSceneIndex);
    }

    void ReloadLevel()
    {
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        SceneManager.LoadScene(currentSceneIndex);
    }
}
```

# Using Invoke

Using `Invoke()` allows us to call a method so it executes after a delay of x seconds.

**Syntax**

- `Invoke("MethodName", delayInSeconds)`

Pros: Quick and easy to use

Cons: String reference; not as performant as using a Coroutine

- ?????? ????????? `Movment` ????????? ???????????? ?????? ??????????????????.

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine.SceneManagement;
using UnityEngine;

public class CollisionHandler : MonoBehaviour
{

    void OnCollisionEnter(Collision other)
    {
        switch (other.gameObject.tag)
        {
            case "Start":
                Debug.Log("??????");
                break;
            case "Finish":
                Debug.Log("??????");
                LoadNextLevel();
                break;
            case "Fuel":
                Debug.Log("????????????");
                break;
            default:
                Debug.Log("????????? ?????? ??????");
                StartCrashSequence();
                break;
        }
    }

    void StartCrashSequence()
    {
        GetComponent<Movement>().enabled = false;
        Invoke("ReloadLevel", 1f);
    }
    
    void LoadNextLevel()
    {
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        int nextSceneIndex = currentSceneIndex + 1;

        if (nextSceneIndex == SceneManager.sceneCountInBuildSettings)
        {
            nextSceneIndex = 0;
        }

        SceneManager.LoadScene(nextSceneIndex);
    }

    void ReloadLevel()
    {
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        SceneManager.LoadScene(currentSceneIndex);
    }
}
```

**Use Invoke To Delay Next Level**

- Parameterize our delay (ie. make a variable which we can turn in the inspector)
- When we reach the Landing Pad, make sure we have a delay before loading the next level
- Stop player controls during the delay

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine.SceneManagement;
using UnityEngine;
using System;

public class CollisionHandler : MonoBehaviour
{
    [SerializeField] float levelLoadDelay = 2f;

    void OnCollisionEnter(Collision other)
    {
        switch (other.gameObject.tag)
        {
            case "Start":
                Debug.Log("??????");
                break;
            case "Finish":
                Debug.Log("??????");
                StartSuccessSequence();
                break;
            case "Fuel":
                Debug.Log("????????????");
                break;
            default:
                Debug.Log("????????? ?????? ??????");
                StartCrashSequence();
                break;
        }
    }

    private void StartSuccessSequence()
    {
        // Todo add SFX Upon Crash
        // Todo add particle effect upon crash
        GetComponent<Movement>().enabled = false;
        Invoke("LoadNextLevel", levelLoadDelay);
    }

    void StartCrashSequence()
    {
        // Todo add SFX Upon Crash
        // Todo add particle effect upon crash
        GetComponent<Movement>().enabled = false;
        Invoke("ReloadLevel", levelLoadDelay);
    }
    
    void LoadNextLevel()
    {
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        int nextSceneIndex = currentSceneIndex + 1;

        if (nextSceneIndex == SceneManager.sceneCountInBuildSettings)
        {
            nextSceneIndex = 0;
        }

        SceneManager.LoadScene(nextSceneIndex);
    }

    void ReloadLevel()
    {
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        SceneManager.LoadScene(currentSceneIndex);
    }
}
```

# Multiple Audio Clips

`PlayOneShot`??? ????????????, ????????? ????????? ????????? ?????? ???????????? ????????? ??? ??????.

Trigger for audio to be played for these situations:

- When the player crashes into an obstacle
- When the player successfully reaches landing pad

Hints:

- Get and Cache the audio component
- Create variables for each clip
- Play the clip at the appropriate time

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine.SceneManagement;
using UnityEngine;
using System;

public class CollisionHandler : MonoBehaviour
{
    [SerializeField] float levelLoadDelay = 2f;
    [SerializeField] AudioClip success;
    [SerializeField] AudioClip crash;

    // Cache the sound
    AudioSource audioSource;

    private void Start()
    {
        audioSource = GetComponent<AudioSource>();
    }

    void OnCollisionEnter(Collision other)
    {
        switch (other.gameObject.tag)
        {
            case "Start":
                Debug.Log("??????");
                break;
            case "Finish":
                Debug.Log("??????");
                StartSuccessSequence();
                break;
            case "Fuel":
                Debug.Log("????????????");
                break;
            default:
                Debug.Log("????????? ?????? ??????");
                StartCrashSequence();
                break;
        }
    }

    private void StartSuccessSequence()
    {
        audioSource.PlayOneShot(success);        
        // Todo add particle effect upon success
        GetComponent<Movement>().enabled = false;
        Invoke("LoadNextLevel", levelLoadDelay);
    }

    void StartCrashSequence()
    {
        audioSource.PlayOneShot(crash);
        // Todo add particle effect upon crash
        GetComponent<Movement>().enabled = false;
        Invoke("ReloadLevel", levelLoadDelay);
    }
    
    void LoadNextLevel()
    {
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        int nextSceneIndex = currentSceneIndex + 1;

        if (nextSceneIndex == SceneManager.sceneCountInBuildSettings)
        {
            nextSceneIndex = 0;
        }

        SceneManager.LoadScene(nextSceneIndex);
    }

    void ReloadLevel()
    {
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        SceneManager.LoadScene(currentSceneIndex);
    }
}
```

# Bool Variable For State

Finish Our Logic

- We want to stop additional things happening after we have a collision event.
- Use our `isTransitioning` bool and an if statement

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine.SceneManagement;
using UnityEngine;
using System;

public class CollisionHandler : MonoBehaviour
{
    [SerializeField] float levelLoadDelay = 2f;
    [SerializeField] AudioClip success;
    [SerializeField] AudioClip crash;

    // Cache the sound
    AudioSource audioSource;

    bool isTransitioning = false;

    private void Start()
    {
        audioSource = GetComponent<AudioSource>();
    }

    void OnCollisionEnter(Collision other)
    {
        if (isTransitioning) { return; } 
        
        switch (other.gameObject.tag)
        {
            case "Start":
                Debug.Log("??????");
                break;
            case "Finish":
                Debug.Log("??????");
                StartSuccessSequence();
                break;
            case "Fuel":
                Debug.Log("????????????");
                break;
            default:
                Debug.Log("????????? ?????? ??????");
                StartCrashSequence();
                break;
            }    
    }

    private void StartSuccessSequence()
    {
        isTransitioning = true;
        // ??? ??? ???????????? ?????? ?????? ??????
        audioSource.Stop();
        audioSource.PlayOneShot(success);        
        // Todo add particle effect upon success
        GetComponent<Movement>().enabled = false;
        Invoke("LoadNextLevel", levelLoadDelay);
    }

    void StartCrashSequence()
    {
        isTransitioning = true;
        // ??? ??? ???????????? ?????? ?????? ??????
        audioSource.Stop();
        audioSource.PlayOneShot(crash);
        // Todo add particle effect upon crash
        GetComponent<Movement>().enabled = false;
        Invoke("ReloadLevel", levelLoadDelay);
    }
    
    void LoadNextLevel()
    {
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        int nextSceneIndex = currentSceneIndex + 1;

        if (nextSceneIndex == SceneManager.sceneCountInBuildSettings)
        {
            nextSceneIndex = 0;
        }

        SceneManager.LoadScene(nextSceneIndex);
    }

    void ReloadLevel()
    {
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        SceneManager.LoadScene(currentSceneIndex);
    }
}
```

`Under` ????????? `Overrides`??? ???????????? `revert all`??? ???????????? ????????????.void

## Make Rocket Look Spiffy

`Rocket` + Right Click + Cube

## How to Trigger Particles

Right Click ==> Effects ==> Particle System

![img](https://cdn-images-1.medium.com/max/1000/1*7Fj97ecrw7UCaaKhsIq6aA.png)

Particle System is a Component added to a Game Object

Right Click ==> Create Empty ==> Whatever ==> Add Components ==> Particle System

We use Modules for controlling behavior

**Play The Particle Effect**

- We use `ParticleSystem.play()` to play our particle system when triggered...
- When we crash, play the explosion particles
- When we reach landing pad, play success particles

```c#
 using System.Collections;
using System.Collections.Generic;
using UnityEngine.SceneManagement;
using UnityEngine;
using System;

public class CollisionHandler : MonoBehaviour
{
    [SerializeField] float levelLoadDelay = 2f;
    [SerializeField] AudioClip success;
    [SerializeField] AudioClip crash;

    [SerializeField] ParticleSystem successParticles;
    [SerializeField] ParticleSystem crashParticles;

    // Cache the sound
    AudioSource audioSource;

    bool isTransitioning = false;

    private void Start()
    {
        audioSource = GetComponent<AudioSource>();
    }

    void OnCollisionEnter(Collision other)
    {
        if (isTransitioning) { return; } 
        
        switch (other.gameObject.tag)
        {
            case "Start":
                Debug.Log("??????");
                break;
            case "Finish":
                Debug.Log("??????");
                StartSuccessSequence();
                break;
            case "Fuel":
                Debug.Log("????????????");
                break;
            default:
                Debug.Log("????????? ?????? ??????");
                StartCrashSequence();
                break;
            }    
    }

    private void StartSuccessSequence()
    {
        isTransitioning = true;
        // ??? ??? ???????????? ?????? ?????? ??????
        audioSource.Stop();
        audioSource.PlayOneShot(success);
        // Todo add particle effect upon success
        successParticles.Play();
        // ---
        GetComponent<Movement>().enabled = false;
        Invoke("LoadNextLevel", levelLoadDelay);
    }

    void StartCrashSequence()
    {
        isTransitioning = true;
        // ??? ??? ???????????? ?????? ?????? ??????
        audioSource.Stop();
        audioSource.PlayOneShot(crash);
        // Todo add particle effect upon crash
        crashParticles.Play();
        GetComponent<Movement>().enabled = false;
        Invoke("ReloadLevel", levelLoadDelay);
    }
    
    void LoadNextLevel()
    {
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        int nextSceneIndex = currentSceneIndex + 1;

        if (nextSceneIndex == SceneManager.sceneCountInBuildSettings)
        {
            nextSceneIndex = 0;
        }

        SceneManager.LoadScene(nextSceneIndex);
    }

    void ReloadLevel()
    {
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        SceneManager.LoadScene(currentSceneIndex);
    }
}
```

Start Size

3 ==> 5

Shape

- Radius: 8
- Radius Thickness 1
- Scale (10, 10, 10)

## Particles For Rocket Boosters

- Set up your rocket so that it has 3 particle systems, ready for us to trigger in code:
  - Main Booster
  - Left Booster
  - Right Booster
- Add them in your prefab and create variables for each

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movement : MonoBehaviour
{
    // Parameters - for tuning, typically set in the editor
    // Cache - e.g. references for readability or speed
    // State - private instance (member) variables

    [SerializeField] float mainThrust = 100f;
    [SerializeField] float rotationThrust = 1f;
    [SerializeField] AudioClip mainEngine;

    [SerializeField] ParticleSystem leftThrusterParticles;
    [SerializeField] ParticleSystem rightThrusterParticles;
    [SerializeField] ParticleSystem rocketJetParticles;


    Rigidbody rb;
    AudioSource audioSource;

    // State??? ???????????? Cache Variable
    bool isAlive;

    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody>();
        audioSource = GetComponent<AudioSource>();
    }

    // Update is called once per frame
    void Update()
    {
        ProcessThrust();
        ProcessRotation();
    }

    void ProcessThrust()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            // (0, 1, 0)
            rb.AddRelativeForce(Vector3.up * mainThrust * Time.deltaTime);

            if (!audioSource.isPlaying)
            {
                audioSource.PlayOneShot(mainEngine);
            }
            
            if (!rocketJetParticles.isPlaying)
            {
                rocketJetParticles.Play();
            }
        }

        else
        {
            audioSource.Stop();
            rocketJetParticles.Stop();
        }
    }

    void ProcessRotation()
    {
        if (Input.GetKey(KeyCode.A))
        {
            ApplyRotation(rotationThrust);

            if (!rightThrusterParticles.isPlaying)
            {
                rightThrusterParticles.Play();
            }
        }

        else if (Input.GetKey(KeyCode.D))
        {
            if (!leftThrusterParticles.isPlaying)
            {
                leftThrusterParticles.Play();
            }
            ApplyRotation(-rotationThrust);
        }

        else
        {
            rightThrusterParticles.Stop();
            leftThrusterParticles.Stop();
        }
    }

    void ApplyRotation(float rotationThisFrame)
    {
        rb.freezeRotation = true;
        transform.Rotate(Vector3.forward * rotationThisFrame * Time.deltaTime);
        rb.freezeRotation = false;
    }
}
```

## Refactor With Extract Method

- Using the extract method approach, refactor `ProcessThrust()` and `ProcessRotation()` to make them read as clearly as possible.

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movement : MonoBehaviour
{
    // Parameters - for tuning, typically set in the editor
    // Cache - e.g. references for readability or speed
    // State - private instance (member) variables

    [SerializeField] float mainThrust = 100f;
    [SerializeField] float rotationThrust = 1f;
    [SerializeField] AudioClip mainEngine;

    [SerializeField] ParticleSystem leftThrusterParticles;
    [SerializeField] ParticleSystem rightThrusterParticles;
    [SerializeField] ParticleSystem rocketJetParticles;


    Rigidbody rb;
    AudioSource audioSource;

    // State??? ???????????? Cache Variable
    bool isAlive;

    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody>();
        audioSource = GetComponent<AudioSource>();
    }

    // Update is called once per frame
    void Update()
    {
        ProcessThrust();
        ProcessRotation();
    }

    void ProcessThrust()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            StartThrusting();
        }

        else
        {
            StopThrusting();
        }
    }

    private void StopThrusting()
    {
        audioSource.Stop();
        rocketJetParticles.Stop();
    }

    void ProcessRotation()
    {
        if (Input.GetKey(KeyCode.A))
        {
            RotateLeft();
        }

        else if (Input.GetKey(KeyCode.D))
        {
            RotateRight();
        }

        else
        {
            StopRotating();
        }
    }

    void StartThrusting()
    {
        // (0, 1, 0)
        rb.AddRelativeForce(Vector3.up * mainThrust * Time.deltaTime);
        if (!audioSource.isPlaying)
        {
            audioSource.PlayOneShot(mainEngine);
        }

        if (!rocketJetParticles.isPlaying)
        {
            rocketJetParticles.Play();
        }
    }

    private void RotateLeft()
    {
        ApplyRotation(rotationThrust);
        if (!rightThrusterParticles.isPlaying)
        {
            rightThrusterParticles.Play();
        }
    }

    private void RotateRight()
    {
        if (!leftThrusterParticles.isPlaying)
        {
            leftThrusterParticles.Play();
        }
        ApplyRotation(-rotationThrust);
    }

    private void StopRotating()
    {
        rightThrusterParticles.Stop();
        leftThrusterParticles.Stop();
    }

    void ApplyRotation(float rotationThisFrame)
    {
        rb.freezeRotation = true;
        transform.Rotate(Vector3.forward * rotationThisFrame * Time.deltaTime);
        rb.freezeRotation = false;
    }
}
```

## Add Cheat / Debug Keys

Challenge

- When the user pushed the "L" key, load the next level
- When the user pushed the "C" key, disable collisions

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine.SceneManagement;
using UnityEngine;
using System;

public class CollisionHandler : MonoBehaviour
{
    [SerializeField] float levelLoadDelay = 2f;
    [SerializeField] AudioClip success;
    [SerializeField] AudioClip crash;

    [SerializeField] ParticleSystem successParticles;
    [SerializeField] ParticleSystem crashParticles;

    // Cache the sound
    AudioSource audioSource;

    bool isTransitioning = false;
    bool collisionDisabled = false;

    private void Start()
    {
        audioSource = GetComponent<AudioSource>();
    }
       
    void Update()
    {
        RespondToDebugKeys();
    }

    void RespondToDebugKeys()
    {
        if (Input.GetKeyDown(KeyCode.L))
        {
            LoadNextLevel();
        }

        else if (Input.GetKeyDown(KeyCode.C))
        {
            collisionDisabled = !collisionDisabled; // toggle collision
        }

    }

    void OnCollisionEnter(Collision other)
    {
        if (isTransitioning || collisionDisabled) { return; }

        switch (other.gameObject.tag)
        {
            case "Start":
                Debug.Log("??????");
                break;
            case "Finish":
                Debug.Log("??????");
                StartSuccessSequence();
                break;
            case "Fuel":
                Debug.Log("????????????");
                break;
            default:
                Debug.Log("????????? ?????? ??????");
                StartCrashSequence();
                break;
        }
    }

    private void StartSuccessSequence()
    {
        isTransitioning = true;
        // ??? ??? ???????????? ?????? ?????? ??????
        audioSource.Stop();
        audioSource.PlayOneShot(success);
        // Todo add particle effect upon success
        successParticles.Play();
        // ---
        GetComponent<Movement>().enabled = false;
        Invoke("LoadNextLevel", levelLoadDelay);
    }

    void StartCrashSequence()
    {
        isTransitioning = true;
        // ??? ??? ???????????? ?????? ?????? ??????
        audioSource.Stop();
        audioSource.PlayOneShot(crash);
        // Todo add particle effect upon crash
        crashParticles.Play();
        GetComponent<Movement>().enabled = false;
        Invoke("ReloadLevel", levelLoadDelay);
    }

    void LoadNextLevel()
    {
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        int nextSceneIndex = currentSceneIndex + 1;

        if (nextSceneIndex == SceneManager.sceneCountInBuildSettings)
        {
            nextSceneIndex = 0;
        }

        SceneManager.LoadScene(nextSceneIndex);
    }

    void ReloadLevel()
    {
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        SceneManager.LoadScene(currentSceneIndex);
    }

```

## Make Environment From Cubes

Window ==> Rendering ==> Lighting

Lighting ==> Environment Tab ==> Skybox Material ==> Set Color

Skybox ==> Shader ==> Skybox ==> Procedure ==> Sky Tint + Ground (Test Purpose)

- Sun Size: 0, Sun Size Convergence: 0, Atmosphere Thickness: 0, Exposure: 0

Main Camera ==> Background ==> Black

Main Camera ==> Clear Flags ==> Solid Color

Prefabs ==> Obstacle ==> Overrides ==> Apply All ==> ????????? ????????? ??????????????? ?????????

#### Make Your Levels Look Interesting

- Using just cubes or other primitives, add some depth to the look of your level.
- Try to frame your world so the player can't fly off where they shouldn't.

## How to Add Lights in Unity

![img](https://cdn-images-1.medium.com/max/1000/1*drjmEY_UWgpzsAx011Pj-g.png)

Right Click ==> Effects ==> Light ==> Point Light (Light Bulb??? ???????????? ????????? ?????? ?????????)

Directional Light ==> Intensity - 0.35

Landing Pad, Launch Pad ==> Point Light ??????

Rocket ==> Spot Light (Range ??????)

- 1. spot light??? ???????????? ????????????
  2. spot light??? ?????? ????????????

Material ==> Emission => Similar Color Tone

## Move Obstacle With Code

![img](https://cdn-images-1.medium.com/max/1000/1*MUlyz0xl-SzFRUVZVEY00w.png)



![img](https://cdn-images-1.medium.com/max/1000/1*VAJF4ON6eRiSXBR_y79n4w.png)

(10, 0, 0) ==> Movement Factor is 0 ==> (0, 0, 0)

Movement Factor is currently 0.5, offset is (5, 0, 0) = (10 * 0.5, 0 * 0.5, 0 * 0.5)

## Mathf.Sin() For Oscillation

![img](https://cdn-images-1.medium.com/max/1000/1*eWpJlTeiJef_jvGeKoH7Sg.png)

https://en.wikipedia.org/wiki/Sine

Circular Movement - Number Between 1 and -1

https://en.wikipedia.org/wiki/Turn_(angle)

https://docs.unity3d.com/ScriptReference/Mathf.Sin.html

![img](https://cdn-images-1.medium.com/max/1000/1*8OVcPob9lQ56FF_GDFt4Bw.png)

Amplitude - Total height of the wave

Period - The amount of time before it repeats iteself

![img](https://cdn-images-1.medium.com/max/1000/1*y74P4jtnfU2XznBvOXWQZw.png)

2?????? or 2 Tau

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Oscillator : MonoBehaviour
{
    Vector3 startingPosition;
    [SerializeField] Vector3 movementVector;
    // Slider ????????? ??????
    // [SerializeField] [Range(0,1)] float movementFactor;
    float movementFactor;
    [SerializeField] float period = 2f;

    // Start is called before the first frame update
    void Start()
    {
        // Current Position
        startingPosition = transform.position;
        Debug.Log(startingPosition);
    }

    // Update is called once per frame
    void Update()
    {
        // How much has been elapsed
        // 2 sec = 1 cycles, 4 sec = 2 cycles
        float cycles = Time.time / period; // Continually Growing over time
        // constant variable which means it doesn't change
        const float tau = Mathf.PI * 2; // constant value of 6.2834
        float rawSinWave = Mathf.Sin(cycles * tau); // going from -1 to 1

        // Debug.Log(rawSinWave);
        // 0 to 2 --> 0 <--> 1 
        movementFactor = (rawSinWave + 1f) / 2f; // recalculated to go from - to 1 so its cleaner
        
        Vector3 offset = movementVector * movementFactor;
        transform.position = startingPosition + offset;
    }
}
```

## Protect Against NaN Error

10 / 0 ==> Cannot divide by Zero

- Try and put some protection code in.
- The goal is to eliminate the NaN error when period is 0.

  Notes About Comparing floats

- Two floats can vary by a tiny amount.
- It's unpredictable to use `==` with floats
- Always specify the acceptable difference
- The smallest float is `Mathf.Epsilon`
- Always compare to this rather than zero
- For example... `if (period <= Mathf.Epsilon) { return; }`

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Oscillator : MonoBehaviour
{
    Vector3 startingPosition;
    [SerializeField] Vector3 movementVector;
    // Slider ????????? ??????
    // [SerializeField] [Range(0,1)] float movementFactor;
    float movementFactor;
    [SerializeField] float period = 2f;

    // Start is called before the first frame update
    void Start()
    {
        // Current Position
        startingPosition = transform.position;
        Debug.Log(startingPosition);
    }

    // Update is called once per frame
    void Update()
    {
        // ?????? ????????? 0?????? ????????? ????????? ???????????? ?????? ???????????? ?????????
        // 0f??? ????????? 0.1234124 ??????????????? ?????? ??? ??????. ????????? ?????????
        // Unity?????? ???????????? ?????? ?????? 0?????? ???????????? Epsilon
        if (period == Mathf.Epsilon) { return; } 
        // How much has been elapsed
        // 2 sec = 1 cycles, 4 sec = 2 cycles
        float cycles = Time.time / period; // Continually Growing over time
        
        
        
        
        // constant variable which means it doesn't change
        const float tau = Mathf.PI * 2; // constant value of 6.2834
        float rawSinWave = Mathf.Sin(cycles * tau); // going from -1 to 1

        // Debug.Log(rawSinWave);
        // 0 to 2 --> 0 <--> 1 
        movementFactor = (rawSinWave + 1f) / 2f; // recalculated to go from - to 1 so its cleaner
        
        Vector3 offset = movementVector * movementFactor;
        transform.position = startingPosition + offset;
    }
}
```

## Designing Level Moments

`Lighting` - Create Empty??? ?????? ?????????, ????????? ????????? `Lighting`??? ??? ????????????.

`Main Camera`??? `Prefabs`??? ???????????????, `Lighting`?????? ?????? ????????? `light`??? ????????????.

### Useful Game Design Approach

Design "moments" and then expand them into a level. Moments that use the environment:

- Fly under
- Fly over
- Fly Through a gap
- Time your flight through moving obstacle
- Land on moving platform
- Fly through narrow tunnel



Moments that use tuning of our existing game:

- Slower rocket (eg. it got damaged)
- Faster rocket (eg. got a boost)
- Darker level
- Closer camera
- Bigger rocket (carrying something)
- Reversed controls
- And so on...

## Hit Escape to Quit

- Create a new script called `QuitApplication.cs`
- Create logic so that if the player hits escape on their keyboard we call `Application.Quit()`
- Add a debug line to make sue that hitting escape works.

```c#
        public class QuitApplication : MonoBehavior
        {
            void Update() 
            {
                if (Input.GetKeyDown(KeyCode.Escape))
                {
                    Debug.Log("We pushed escape");
                    Application.Quit();
                }
            }
        }
```

## How to Build & Publish a Game

File ==> Build Settings ==> Build == >  ???????????? Builds ==> ????????? ???????????? `Project Boost` ??????

?????? Build Settings ???????????? ==> WebGL ==> Switch Platform ==> Player Settings ==> Player ==> Publishing Settings ==> Compression Format: Disabled ==> Build ==> ?????? WebGL Builds ==> 

sharemygame.com ==> ????????? ==> ??? ==> Upload Game ==> WebGL Builds ==>

- Make a WebGL build of your game
- Publish to sharemygame.com
- Tell our community about your game and where to find it
- Play at least one other person's game (good karma)

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine.SceneManagement;
using UnityEngine;
using System;

public class CollisionHandler : MonoBehaviour
{
    [SerializeField] float levelLoadDelay = 2f;
    [SerializeField] AudioClip success;
    [SerializeField] AudioClip crash;

    [SerializeField] ParticleSystem successParticles;
    [SerializeField] ParticleSystem crashParticles;

    // Cache the sound
    AudioSource audioSource;

    bool isTransitioning = false;
    bool collisionDisabled = false;

    private void Start()
    {
        audioSource = GetComponent<AudioSource>();
    }
       
    void Update()
    {
        RespondToDebugKeys();
    }

    void RespondToDebugKeys()
    {
        if (Input.GetKeyDown(KeyCode.L))
        {
            LoadNextLevel();
        }

        else if (Input.GetKeyDown(KeyCode.C))
        {
            collisionDisabled = !collisionDisabled; // toggle collision
        }

    }

    void OnCollisionEnter(Collision other)
    {
        if (isTransitioning || collisionDisabled) { return; }

        switch (other.gameObject.tag)
        {
            case "Start":
                Debug.Log("??????");
                break;
            case "Finish":
                Debug.Log("??????");
                StartSuccessSequence();
                break;
            case "Fuel":
                Debug.Log("????????????");
                break;
            default:
                Debug.Log("????????? ?????? ??????");
                StartCrashSequence();
                break;
        }
    }

    private void StartSuccessSequence()
    {
        isTransitioning = true;
        // ??? ??? ???????????? ?????? ?????? ??????
        audioSource.Stop();
        audioSource.PlayOneShot(success);
        // Todo add particle effect upon success
        successParticles.Play();
        // ---
        GetComponent<Movement>().enabled = false;
        Invoke("LoadNextLevel", levelLoadDelay);
    }

    void StartCrashSequence()
    {
        isTransitioning = true;
        // ??? ??? ???????????? ?????? ?????? ??????
        audioSource.Stop();
        audioSource.PlayOneShot(crash);
        // Todo add particle effect upon crash
        crashParticles.Play();
        GetComponent<Movement>().enabled = false;
        Invoke("ReloadLevel", levelLoadDelay);
    }

    void LoadNextLevel()
    {
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        int nextSceneIndex = currentSceneIndex + 1;

        if (nextSceneIndex == SceneManager.sceneCountInBuildSettings)
        {
            nextSceneIndex = 0;
        }

        SceneManager.LoadScene(nextSceneIndex);
    }

    void ReloadLevel()
    {
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        SceneManager.LoadScene(currentSceneIndex);
    }

```













