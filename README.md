# Programming-Journal-2
Semester 2 programming 



# Week 2 package 1 Movement in 3D

## Introduction 

The goal for this semester is to make 4 packages that work on their own and have some relevance to the game that I'm working on and looking to make for my mechanical demo.For my mechanical demo, I'm looking to make a 3D platformer with combat mechanics as a basis. First things first I'd have to work on the meat of the game which would be to make the movement of the game.

## Making the movement

First things first I had to set the scene. I made a 3D scene within Unity and created an empty terrain and moved the camera into a position where it could see the player, but now I needed to have a character that I could work on, which I didn't. So to have something to test, I decided on making a capsule 3D object, and made a material to help make it stand out. Now I needed to add a rigidbody to it so that there's gravity to our player so that whenever he's in the sky. Next I needed to make a new script, that would essentially act as a character controller. After making the character controller in C# I increased the movement speed to see if the character would move, and it did. However, there was now one problem. The character would just roll around the scene instead of maintaining a single position as it moved. So in order to fix this I'd have to go into the rigidbody settings and essentially freeze the rotation of the character on every axis. Now it works, I have basic movement. 



Here's the code: 



using System;
using System.Collections;
using System.Collections.Generic;
using System.IO;
using UnityEngine;
using UnityEngine.Playables;

public class ThirdPersonMovement : MonoBehaviour
{

    public CharacterController controller;

    public float speed= 6f;
    public float turnsmoothtime = 0.1f;
    private float turnsmoothvelocity;
    public float jumphspeed = 1f;
    private float yspeed;
    private Vector3 movementDirection;
    private float velocity;
    private float rotationSpeed;
    
    
    void Update()
    {
        float horizontal = Input.GetAxisRaw("Horizontal");
        float vertical = Input.GetAxisRaw("Vertical");
        Vector3 direction = new Vector3(horizontal, 0f, vertical).normalized;



        yspeed += Physics.gravity.y * Time.deltaTime;


        yspeed += Physics.gravity.y * Time.deltaTime;
        if (Input.GetKeyDown(KeyCode.Space))
        {
            yspeed = jumphspeed;
        }
       

        if (direction.magnitude>=0.1f)
        {
            float targetAngle = Mathf.Atan2(direction.x, direction.z) * Mathf.Rad2Deg;
            float angle = Mathf.SmoothDampAngle(transform.eulerAngles.y, targetAngle, ref turnsmoothvelocity,
                turnsmoothtime);
            var rotation = transform.rotation;
            rotation=Quaternion.Euler(0f,targetAngle,0f);
            rotation=Quaternion.Euler(0f,targetAngle,0f);
            transform.rotation = rotation;
            controller.Move(direction * speed * Time.deltaTime);
        }

        if (movementDirection!=Vector3.zero)
        {
            Quaternion toRotation=Quaternion.LookRotation(movementDirection,Vector3.up);

            transform.rotation =
                Quaternion.RotateTowards(transform.rotation, toRotation, rotationSpeed * Time.deltaTime);
        }
      
    }

  }





Now it was time to work on jumps, I revisited the script to work on jumps in the scene so that allows our player to jump up and down. So now we have a fully working character with good movement.  Here's the code for jumping:

using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Jump : MonoBehaviour
{
    public float speed = 10f;
    public Rigidbody rb;
    public bool Grounded = true;

    private void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space)&& Grounded)
        {
            rb.AddForce(new Vector3(0,5,0),ForceMode.Impulse);
            Grounded = false;
        }
        
    }


}


## Making the camera move with the character 

There's many ways to do this, but the easiest way of doing this is using the function known as cinemachine. I could use the Cinemachine function to help the camera essentially focus or "look at" the player as they move. I had to adjust the distance of the camera to make the gameplay feel much more smooth, and actually having an idea of the player's surroundings, makigng it easier to notice surroundings and whatnot. 


# Week 3 package 2 Setting a scene 

## Moving platforms 

First things first, I wanted to make the scene. Considering the goal of the game is to make a 3D platformer with melee abilities I want to work on implementing different types of structures, the first of these being moving platforms. to make sure this works I set up 2 platforms in the air, and thought about how I'd go about doing this. First things first I created an animator and an animation track for the object and animated movement for the platform to move. Once done I made the animation loop making it so that I had something there, and we'd have movement. 


But then there was a problem, while I do have a moving platform, my character would just slide off the platform whilst the animation played instead of staying on the platform. I wasn't too sure as to why this was. But after doing some research, I looked at doing adding more than just the animation. On the platform I added a second box collider and adjusted the height and position. 

Now the plan is to make a script that uses the trigger I've placed on the platorm. 
The script would trigger the platform to become a parent object of our player object meaning that it should stick the script looks something like this: 


using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Platform : MonoBehaviour
{
    public GameObject player;

    private void OnTriggerEnter(Collider other)
    {
        if (other.gameObject==player)
        {
            player.transform.parent = transform;
        }
    }

    private void OnTriggerExit(Collider other)
    {
        if (other.gameObject==player)
        {
            player.transform.parent = null;
        } 
    }
}


Now it should work, at least that's what I initially thought, but even with the script on I had a problem where my character would still not stick to the platform, so I had to look for a fix seeing as everything was the same. But after looking at the script it turns out that there was nothing wrong with the script itself. The solution was in the animator component, and changing it to animating physics. 




## Collectibles 
  Every 3D platformer tend to have collectibles, for this test I decided to have collectibles I previously made and would use them in this example to try and get something working. I added them around the scene and eventually included a collection script that allowed me to destroy the object when I pick it up. I had a score but now I needed something that would show that. So I created a UI canvas and added some text to show off the Text UI on my unity scene, to display the score, and soon have something to show off the overall scoring of the amount of gems collected by the player.

First things first, I needed to set up the script that houses the scoring system in the first place. But in order to do so I'd need to create a new canvas and UI component for text the score to be updated when the collectible has been collected by the player. The script looks something like this: 


using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class ScoringSystem1 : MonoBehaviour
{
    public GameObject scoreText;
    public int theScore;

    void OnTriggerEnter(Collider other)
    {
        theScore += 1;
        scoreText.GetComponent<Text>().text = "HexoGems" + theScore;
        Destroy(gameObject);
    }
}

One thing that needs to be ticked off is the "Is Trigger" option, mainly so that we can actually collect the object. This is so because without the trigger on the script can't function meaning that the box collider makes the item a solid object that collides with the player when we touch it. 

After testing, it looks like I made an error, in the script above I added the line 

 scoreText.GetComponent<Text>().text = "HexoGems" + theScore; 
  
 However, it turns out when I put "HexoGems" it will show as "Hexogems" and not "SCORE: 0" This is because I made the naming convention "HexoGems" and seeing as I'm telling the script to call on that function, it'll essentially deliver that result on screen, which isn't what I want so instead of "HexoGems" I've changed it to "SCORE:" to make sure that it now seems more appropriate in context. 
  
  
  With the changes made the script now looks something like this: 
  
  using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class ScoringSystem1 : MonoBehaviour
{
    public GameObject scoreText;
    public int theScore;

    void OnTriggerEnter(Collider other)
    {
        theScore += 1;
        scoreText.GetComponent<Text>().text = "SCORE:" + theScore;
        Destroy(gameObject);
    }
}

 After doing some more trouble shooting, I realised that I'd have to make two seperate scripts, one that essentially houses the scoring system and another that will allow the player to collect any collectibles to increase their score. So that meant I had one for collection and another for scoring. 
    
    
    After making the scoring script, I attached it to an empty game object along with UI elements to get it to basically change the score text each time a gem was collected by the player. 
  
    Here's the script for the scoring system:
    
    using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class ScoringSystem1 : MonoBehaviour
{
    public GameObject scoreText;
    public static int theScore;

    void Update()
    {
        
        scoreText.GetComponent<Text>().text = "SCORE: " + theScore;
        
    }
}
    
    
Next I added a new script to the collectibles that allowed them to be colleced, using the OnTriggerEnter command, and using colliders so that this works. 
    Here's the script:
    
    using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Collection : MonoBehaviour
{
    
    void OnTriggerEnter(Collider other)
    {
      ScoringSystem1.theScore += 1;
      Destroy(gameObject);
    }
}

    
    After some more tinkering, it works. 

  
  
  # Week 5 package 3, Working on Combat in a 3D platformer 
   
   A key element in any 3D platformer is to have enemies. Enemies will always be something that need to exist in games in order for the player to have something to interact with, whether it be jumping on them, or punching them there's so many ways for this method to occur. In my game, I've decided to go the beat 'em up route. However, the way I've gone about it is fairly different as opposed to how games would normally handle this matter. Normally games would handle it using "Hitboxes" I decided to handle it with something I call a "hit point". A small technique I learned that makes attacks much more realistic and accurate in the nature of how they work. For instance, a punch for example will be the most impactful at the fist, seeing as where that's where all the initial pressure ends up. To emulate this, I essentially added a massive sphere to be in place at the end of the punch, with a damage script that deals damage to the enemies. 

## Enemies 
Enemies are a very core part of how this works since it's them we'll be hitting. I set up some quick enemies to stand idle, as the main focus of this part is to get the enemies low on health. So after importing a couple and copying and pasting them, I decided to work on their health scripts and add a canvas with a number that essentially displays the health of the enemy. First things first, I got some enemies that I made, and added a script to them, this script essentially gives the enemy health as well as a health counter, that I'd made using the canvas in unity. 

First things first, I needed to make the enemy controller, So I made a health script and added an "Enemy layer" this allowed me to  then add a player/combat script to the player.

## Animations and Attacks

Attacks and animations come from mixamo, for each animation I coded a different script, for each action/attack. Each animation had it's own attack point at the end of a limb where I'd add a small sphere, to the end of it to act as a point where damage is dealt, I'd add a damage value as an integer to each attack, and added a public void to the enemy script that has a value for health so that health would be subtracted from the max health once attacked. 
After mapping each attack to a key, I tested to see if a punch labelled at 5 attack power, would kill an enemy with 5 health. After pressing J, it killed and destroyed the object on impact. Meaning that we got the combat segment of the game to work using the hitball technique. 
  


 # Week 5 package 3 continued , Adding more options to our character
 
 ## Introduction
 
 In a 3D platformer, the movement is the deciding factor for how the game plays and feels, the game should feel more akin to suit the player's experiences and should feel fun and flow well in the moment. In today's package I'll be working on giving the player more options in a game and working on those aspects, for example being able to roll, or being able to double jump. Any extra animations I'll be using will be taken from Adobe's Mixamo for this section.
 
 ## Setting the scene
 
Just like the last three packages I'll be using the third person character controller from the ubity asset store as a basis for this, meaning I don't have to code a whole new character controller. Using the scene that comes with it allows me to work on adding animations more efficiently. 


## Downloading animations 

Well, first things first I needed to figure out what Animations I'd need to download, seeing as I have very simple animations for basic movements. Or Idle animations and some attacks. So that we can give the character more personality and flair. 

The animations I downloaded included: 
-Roll animation
-New idle animation
-Kick animation 
-Punch animation

Now I have to apply these to the character. But how ?


## Animation tree
Each component has it's own animation tree, the third person controller has it's own thing, now I can add to it and or replace the animations in the tree, so first things first I'll start with the idle animation. So I click on the "PlayerArmature" component, and open up the animator. I click on the "Idle Walk run Blend" and click on the blend tree, clicked on the properties and changed the idle animation with the one I had taken from Mixamo. Once playing it, my character clipped through the floor, which caused some confusion. I could still move but the problem was there was no animation and my character remained clipped through the floor as it moved, with no animations stuck to a pose. I then realised the reason for this issue was due to the fact that, the animation type or the rig wasn't set to "humanoid" so I'd changed the animation type to "Humanoid" and made it so that the animation would play when it needed to, only when my character was idle. Next thing was to get the attack animation done, the easy stuff was done, now I had to focus on scripting. 

## Writing the scripts for any attack animations. 

So first things first, I had to make sure that I had to make sure that the animation would play in the first place so I made sure to set the animation to the right rigging type which again was humanoid. After doing so I made a new script, and named it "Attack" Here's the script:



using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Attack : MonoBehaviour
{
    private Animator anim;
    public float cooldownTime = 2f;
    private float nextFireTime = 0f;
    public static int noOfClicks = 0;
    float lastclickedtime = 0;
    float maxcombodelay = 1;

    // Start is called before the first frame update
    void Start()
    {
        anim = GetComponent<Animator>();   
    }
    private void Hit()
    {
        anim.SetTrigger("Hit");
    }

    // Update is called once per frame
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Mouse0))
        {
            Hit();
        } 
       
    }



    

}





After doing the script I needed to add the animations so I clicked on the animation I was looking for, which was the "combo punch", first things first I created an empty state and named it "Combo", after doing so, I added the punch combo animation to the state and branched a transition to the idle animation. I then created a new bool and added that to the animation transitions setting one to true and the other to false. Remembering to keep the exit time off on one state.  

The initial problem I had first was the attack didn't play at all, so I thought maybe changing the input type would initially work, and I was very wrong. I changed it and inststead going back to the old input system broke it more. So I changed the input system to "both" instead. That seemed to have reverted it but the input issue persisted, So to fix this I revised the script over again, and changed around some naming conventions, instead of using a boolean I used triggers instead and that seemed to fix the issue. 
    
    
    
# Week 5 package 4 Combat, Shooting a projectile. 
    
    For this, I wanted to wotrk on shooting a projectile from our character that would kill any enemies we have. Some games have this and I feel like it's more enjoyable than being able to shoot enemies at a range. 
    
    First things first I needed to write a script that would instantiate an object (being the projectile), and also tell the object what way to fire, and what the force and speed of the projectile should be. 
    
    
    Here's the code:
    using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Fireball : MonoBehaviour
{
    public GameObject Projectile;

    public Transform Firepoint;

    public float force;

    private Vector3 direction;

    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.O))
        {
            Fireball P = Instantiate(Projectile, Firepoint.position, Quaternion.identity).GetComponent<Fireball>();
            P.direction = transform.forward;
            P.Speed =force;
        }
    }

    public float Speed { get; set; }
}

    
    After adding this, I needed to make sure that the projectile would destroy any objects, being the enemies here. After doing so, it worked. 
 

