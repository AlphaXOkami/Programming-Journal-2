# Programming-Journal-2
Semester 2 programming 



# Week 2 package 1 Movement in 3D

## Introduction 

The goal for this semester is to make 4 packages that work on their own and have some relevance to the game that I'm working on and looking to make for my mechanical demo.For my mechanical demo, I'm looking to make a 3D platformer with combat mechanics as a basis. First things first I'd have to work on the meat of the game which would be to make the movement of the game.

## Making the movement

First things first I had to set the scene. I made a 3D scene within Unity and created an empty terrain and moved the camera into a position where it could see the player, but now I needed to have a character that I could work on, which I didn't. So to have something to test, I decided on making a capsule 3D object, and made a material to help make it stand out. Now I needed to add a rigidbody to it so that there's gravity to our player so that whenever he's in the sky. Next I needed to make a new script, that would essentially act as a character controller. After making the character controller in C# I increased the movement speed to see if the character would move, and it did. However, there was now one problem. The character would just roll around the scene instead of maintaining a single position as it moved. So in order to fix this I'd have to go into the rigidbody settings and essentially freeze the rotation of the character on every axis. Now it works, I have basic movement. 

Now it was time to work on jumps, I revisited the script to work on jumps in the scene so that allows our player to jump up and down. So now we have a fully working character with good movement. 

## Making the camera move with the character 

There's many ways to do this, but the easiest way of doing this is using the function known as cinemachine. I could use the Cinemachine function to help the camera essentially focus or "look at" the player as they move. I had to adjust the distance of the camera to make the gameplay feel much more smooth, and actually having an idea of the player's surroundings, makigng it easier to notice surroundings and whatnot. 


# Week 3 package 2 Setting a scene 

## Moving platforms 

First things first, I wanted to make the scene. Considering the goal of the game is to make a 3D platformer with melee abilities I want to work on implementing different types of structures, the first of these being moving platforms. to make sure this works I set up 2 platforms in the air, and thought about how I'd go about doing this. First things first I created an animator and an animation track for the object and animated movement for the platform to move. Once done I made the animation loop making it so that I had something there, and we'd have movement. 

## Collectibles 
  Every 3D platformer tend to have collectibles, for this test I decided to have collectibles I previously made and would use them in this example to try and get something working. I added them around the scene and eventually included a collection script that allowed me to destroy the object when I pick it up. I had a score but now I needed something that would show that. So I created a UI canvas and added some text to show off the Text UI on my unity scene, to display the score, and soon have something to show off the overall scoring of the amount of gems collected by the player. 




