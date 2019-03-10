Go to Scripts Folder through Assets->Scripts, Right click and create->"C# Script" and rename it PlayerM

The script comes with two functions : Start and Update Functions.

          Start function is called only once; that's when the game or scene starts.
          Update function is called many times every second(depending on how performant the hardware is).


Click on "Player" gameObject that's present in the Hierarchy(top left menu). You can now see the "Components" of Player in Inspector(top right menu).

Some Components are already present, like : Transform, Sprite Renderer, etc...

          Add the script to Player by draging and droping it into the Inspector.
  
############################################################################################
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


###########################################################################################
Now let's move the Player:


void Update () {                      


    if (Input.GetKey(KeyCode.D))    
        rb.velocity = speed * Vector2.right;    // if the key pressed is "D" we change the velocity to = 10 * (1, 0). 

    else if (Input.GetKey(KeyCode.A))
        rb.velocity = speed * Vector2.left;     // else it is changed to = 10 * (-1, 0).
}

__________________________________________________________________________________________
Result :

    Click on play button and move the player using "A" and "D" buttons.
    
As you might have noticed, the player rotates when you try to move the him.

To fix this, simply freeze the Rotation by clicking on "Player" and then click on :

    "RigidBody2D->Contraints->Freeze Rotation Z"
