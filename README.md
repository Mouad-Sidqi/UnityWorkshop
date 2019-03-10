Clone the Repository and Open the folder using Unity

![alt text](https://static1.squarespace.com/static/56ca25c2b654f92cd6ea08ad/59b1a8898419c20a25ae2210/59b1a88912abd93aa28fa01f/1504815243258/Group+16.png?format=1000w)

###########################################################################################
###########################################################################################
Double Click on Assets->Scenes->SampleScene.

Go to Scripts Folder through Assets->Scripts, Right click and create->"C# Script" and rename it PlayerM

The script comes with two functions : Start and Update Functions.

          Start function is called only once; that's when the game or scene starts.
          Update function is called many times every second(depending on how performant the hardware is).


Click on "Player" gameObject that's present in the Hierarchy(top left menu).



![alt text](https://docs.unity3d.com/uploads/Main/Hierarchy.png)

You can now see the "Components" of Player in Inspector(top right menu).



![alt text](https://docs.unity3d.com/uploads/Main/RigidBodyGO.png)

Some Components are already present, like : Transform, Sprite Renderer, etc...

          Add the script to Player by draging and droping it into the Inspector.





############################################################################################
###########################################################################################


Let's make the player move :

Double click on the script and

Create 2 variables:

                        // Rigidbody2D is a Component that controls physics(like Velocity, Gravity, Mass, etc..)
                        // Here, we created a Rigidbody2D-Type variable and we called it "rb"
     private Rigidbody2D rb; 

                        // this is the variable that will controll how fast the character will move

     private float speed; 

__________________________________________________________________________________________
Result:

          public class PlayerM : MonoBehaviour {

                    private Rigidbody2D rb;

                    private float speed;


                    void Start () {                      


                        speed = 10;                               // we initialized speed by 10
                        rb = GetComponent<Rigidbody2D>();         // we initialized rb variable with the player's Rigidbody2D
                    }

                    void Update () { 

                    }

          }


#Click on Play!
...And nothing happens.

###########################################################################################
###########################################################################################




Now let's move the Player:

we do this by changing velocity(the speed of something in a given direction).


          void Update () {                      


              if (Input.GetKey(KeyCode.D) == true)    
                  rb.velocity = speed * Vector2.right;    // if the key pressed is "D" we change the velocity to = 10 * (1, 0). 

              else if (Input.GetKey(KeyCode.A) == true)
                  rb.velocity = speed * Vector2.left;     // else it is changed to = 10 * (-1, 0).
          }

__________________________________________________________________________________________
Result :

    Click on play button and move the player using "A" and "D" buttons.
    
As you might have noticed, the player rotates when you try to move the him.

To fix this, simply freeze the Rotation by clicking on "Player" and then click on :

    "RigidBody2D->Contraints->Freeze Rotation Z"
    


#Click on Play!


###########################################################################################
###########################################################################################


If you try moving again you might notice that the gravity doesn't effect the Player as it should. that's because we keep resetting the y position. i.e the 0 in "10 * (-1, 0)"

To fix this, let's change how we control his movements to


        void Update () {

	        if (Input.GetKey(KeyCode.A) || Input.GetKey(KeyCode.D))
        	    rb.velocity = new Vector2(Input.GetAxis("Horizontal") * speed, rb.velocity.y);
    	}


Input.GetAxis("Horizontal") is a value in the range -1...1 depending on if you're pressing: "A" or "D" || "LeftArrow" or "RightArrow"




###########################################################################################
###########################################################################################


Now let's add a Jump to the Player by first creating a variable
        
        private float jumpSpeed;
        
        void Start () {
                    rb = GetComponent<Rigidbody2D>();
                    speed = 10;
                    jumpSpeed = 10;
        }
	

and then adding this condition to the script

	void Update () {

        if (Input.GetKeyDown(KeyCode.Space) == true)
        {
            rb.velocity = new Vector2(Input.GetAxis("Horizontal") * speed, Mathf.Abs(rb.velocity.y + jumpSpeed));
        }
        else if (Input.GetKey(KeyCode.A) || Input.GetKey(KeyCode.D))
            rb.velocity = new Vector2(Input.GetAxis("Horizontal") * speed, rb.velocity.y);
    }



#Click on Play!


Change these values in Player's Rigidbody2D: (You can experiment with these value as much as you want.)
	
	Mass : 60
	Linear Drag : 5
	Gravity : 2

You might want to change jumpSpeed variable which is hard-coded in the script, but there's a built in feature to make things easier

Go to Player's Script and add this following field above the variables you want to tweak.

		[SerializeField]
    	private float speed;
    	[SerializeField]
    	private float jumpSpeed;

You can now see these variables in the script that's attached to the Player and you can tweak them in-real-time.





########################################################################################### ###########################################################################################


Finally, let's make the camera follows the player by creating a new script, renaming it "CameraFollowPlayer" and attaching it to "Main Camera" in Hierarchy.

	public class CameraFollowPlayer : MonoBehaviour {

		private Transform camPos;
		private Transform playerPos;

		void Start () {
			camPos = GetComponent<Transform>();
			playerPos = FindObjectOfType<PlayerM>().transform;
		}

		void Update () {
				
			// we change the camera's x position to the player's x position and keep y and z the same, simple as that
			
			camPos.position = new Vector3(playerPos.position.x,  camPos.position.y, camPos.position.z) ; 
				
		}
	}

#Click on Play!















########################################################################################### ###########################################################################################
########################################################################################### ###########################################################################################










Bonus:


Let's animate our player!


First  create a variable in PlayerM-Script with "SpriteRenderer" type as the following shows:

	        private SpriteRenderer sR;

Initialize in Start function it by

	        sR = GetComponent<SpriteRenderer>();


let's first flip the direction the player is facing depending on what direction we're pressing.
add the following to Update function

	if (Input.GetKey(KeyCode.A))
            sR.flipX = true;
        else if (Input.GetKey(KeyCode.D))
            sR.flipX = false;
	    
#Click on Play!


now add the following statements which will set a preconfigured Bool variable to false/true depending on if the Player is moving or not.

	
        if (Input.GetKey(KeyCode.A))
        {
            animationController.SetBool("Moving", true);
            sR.flipX = true;
        }
        else if (Input.GetKey(KeyCode.D))
        {
            animationController.SetBool("Moving", true);
            sR.flipX = false;
        }
        else
            animationController.SetBool("Moving", false);

You can add/change different bool/float/int variables for different animations in Animator tab.

#Click on Play!




###########################################################################################


Voila!

You can now move and jump with your Player and apply different animations!
