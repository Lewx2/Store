adamt@microsoft.com


https://github.com/adamtuliper/VampKid-vs-Zombie-Apocalypse
https://github.com/adamtuliper/VampKid-vs-Zombie-Apocalyse-Starter

http://www.meetup.com/LAUnity3D/
adamtuliper.com


assets->import package->environment



Gameobject->3d objects->terrain
CLick terrain, click paint brush. Choose Edit Texture->Add Texture, then click "Select" and find the grassrockyalbedo

Lets bring in a character controller
Assets->Import Package->Characters



Disable the mesh renderer component on the cubes to make them invisible walls


To Build
File->Build Settings (or control-shift-b)



Physics - two components
1. Collider - defines the region for collisions
2. Rigidbody - gives an object mass, gravity


Main code methods
Start()
Update()

OnCollisionEnter
OnCollisionExit
OnTriggerEnter


	    Debug.Log("Starting up " + gameObject.name);

Debug.Log("Updating " + gameObject.name);

To control gameobject in code we
1. Integrate into predefined unity events, like OnCollisionEnter above
2. Control a game object in an Update() method which is called every frame
3. In either #1 or #2, we can simply ask unity for a component on the game object the script is assigned to
	//To add a force to a rigidbody
  var rigidBody = GetComponent<RigidBody>();
  rigidBody.AddForce( new Vector3(50,50,50); 
) 
  
To control animations - we ask for the Animator component and set variables
var animator = GetComponent<Animator>();
animator.SetTrigger("Jump");
or
animator.SetBool("Run", true);


using UnityEngine;
using System.Collections;


public class PhysicsTest : MonoBehaviour {
	
	// Use this for initialization
	void Start () {
		Debug.Log("Starting up" + gameObject.name);
	}
	
	// Update is called once per frame
	void Update () {
		Debug.Log ("Starting up" + gameObject.name);
		
	}
	
	
	
	public void OnCollisionEnter(Collision collision)
	{
		//G.O. must have a collider
		//and one of the G.O must have a rigidbody
		if(collision.gameObject.tag == "Enemy")
			//Destroy(collision.gameObject);
			var rigidBody = collision.gameObject.GetComponent<Rigidbody>();
		rigidBody.AddForce( new Vector3(0,50,0)  );
    var animator = GetComponent<Animator>();
    animator.SetTrigger("Scale");

	}
	}
}

    

aka.ms/unitysunnyvale
Adam is the cooliest :D
All Hail Adam

:)
Edit->Project Settings->Input


	    var horizontal = Input.GetAxis("Horizontal");

//----------------------------------
using UnityEngine;
using System.Collections;

public class YetiController : MonoBehaviour
{

    private Animator _animator;
    private Rigidbody2D _rigidBody;
	// Use this for initialization
	void Start ()
	{
	    _animator = GetComponent<Animator>();
	    _rigidBody = GetComponent<Rigidbody2D>();
	}
	
	// Update is called once per frame
	void Update ()
	{
	    var horizontal = Input.GetAxis("Horizontal");

	    if (horizontal < 0)
	    {
	        var scale = transform.localScale;
	        scale.x = -1;
	        transform.localScale = scale;
	    }
	    else if (horizontal > 0)
        {
            var scale = transform.localScale;
            scale.x = 1;
            transform.localScale = scale;
        }


        if (horizontal != 0)
	    {

            //Run!
            _animator.SetBool("Run", true);
            _rigidBody.velocity = new Vector2(horizontal * 20, _rigidBody.velocity.y );
        }
	    else
	    {
            //Idle
            _animator.SetBool("Run", false);
            _rigidBody.velocity = new Vector2(0, _rigidBody.velocity.y);
        }
    }
}

//"Unity gives us strength." -Tassadar



//------------------------------------------------------------------------------------



using UnityEngine;
using System.Collections;

public class TestSpawn : MonoBehaviour {
//Create a prefab and assign it here by dragging and dropping in the editor
    public GameObject Island;

	// Use this for initialization
	void Start ()
	{
	    StartCoroutine(CoRandomSpawn());
	}

    IEnumerator CoRandomSpawn()
    {
        Debug.Log("First Frame");
        yield return null;
        Debug.Log("Second Frame");
        yield return null;
        Debug.Log("Third Frame");

        while (true)
        {
            yield return new WaitForSeconds(5);
            Instantiate(Island, transform.position, Quaternion.identity);
        }
    }

}




/// 2d project vs 3d
2d:
1. Defaults new cameras to orthographic instead of perspective
2. Images import as sprites instead of textures (so you can drag/drop in scene)
3. In the scene view, the 2D button is checked off by default





----------------------------------------------------------------------------------------------




using UnityEngine;
using System.Collections;

public class RandomSpawn : MonoBehaviour
{

    //drag and drop each spawn point onto the name "Spawn Points" in unity on whatever game object
    //this code is assigned to
    public Transform[] SpawnPoints;
    public GameObject TypeToSpawn;

    // Use this for initialization
    void Start()
    {
        StartCoroutine(CoRandomSpawn());
    }

    IEnumerator CoRandomSpawn()
    {
        while (true)
        {
            yield return new WaitForSeconds(3);
            var randomPoint = SpawnPoints[Random.Range(0, SpawnPoints.Length)];
            Instantiate(TypeToSpawn, randomPoint.position, Quaternion.identity);
        }

    }

}






-----------------------------
using UnityEngine;
using System.Collections;

public class Projectile : MonoBehaviour
{

    private Rigidbody2D _rigidBody;
    private Vector3 _playerDirection;
    void Start()
    {
        var player = GameObject.FindGameObjectWithTag("Player");
        _rigidBody = GetComponent<Rigidbody2D>();
        //transform.LookAt(player.transform.position);

        Vector3 dir = player.transform.position - transform.position;
        float angle = Mathf.Atan2(dir.y, dir.x) * Mathf.Rad2Deg;
        transform.rotation = Quaternion.AngleAxis(angle, Vector3.forward);

        _playerDirection = (player.transform.position - transform.position).normalized;

       // transform.rotation.SetLookRotation(_rigidBody.velocity, Vector3.forward);


    }

    void FixedUpdate()
    {
        //_rigidBody.AddForce(shipDirection * 0.5f, ForceMode2D.Impulse);
       // float maxVelocity = 75.0f;
        //if (_rigidBody.velocity.magnitude > maxVelocity)
       // {
            _rigidBody.velocity = _playerDirection * 5; //_rigidBody.velocity.normalized * maxVelocity;
       // }

        //_rigidBody.velocity = (Vector2)transform.TransformDirection(Vector3.up) * 1;// transform.TransformDirection(Vector3.forward);
    }


    public void OnTriggerEnter2D(Collider2D collision)
    {
        _playerDirection = _playerDirection*-1;
    }
}

---------------------------------------------------------------------------------------------------------------------------------------------------------
//Code for changing text when hovering mouse over it

using UnityEngine;
using System.Collections;
[RequireComponent(typeof(BoxCollider))]
[RequireComponent(typeof(TextMesh))]

public class TextBeaviour : MonoBehaviour {
    // Use this for initialization
    //remember initial color and fontsize
    Color initialColor;
    int fontSizeInitial;
    private TextMesh textmesh;

	void Start () {
        textmesh = GetComponent<TextMesh>();
  			initialColor = textmesh.color;
        fontSizeInitial = textmesh.fontSize;
    }

    void OnMouseEnter()
    {
        textmesh.fontSize = 120;
        textmesh.color = Color.red;
    }  

    void OnMouseExit()
    {
        textmesh.fontSize = fontSizeInitial;
        textmesh.color = initialColor;
    }


}







st# Store
