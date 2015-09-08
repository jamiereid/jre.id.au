+++
Categories = ["Unity", "Game Development"]
Description = "A simple waypoint system for moving things in Unity"
Tags = ["Unity", "Code", "C#", "Game Development"]
date = "2015-09-08T14:41:00+10:00"
title = "Unity: Simple Waypoint System"
+++

I recently started a night course at the [AIE][1] on Game Development Fundamentals. The first assignment was to make some minor changes to a prebuilt little game they gave us. I ended up throwing out the project they gave us and using the opportunity to extend myself a little bit.

The project was an arcade style SHMUP (think [Galaga][2], [Space Invaders][3], or [Raptor][4]). I needed a way to move the enemies around; after toying around with some simple AI ideas (like [Boids][5]) I ended up deciding to just bake the movement in using patterns.

Below is the code I wrote to achieve this.

{{< highlight csharp "linenos=inline" >}}

using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class EnemyController : MonoBehaviour {

	[Tooltip("How fast is the enemy?")]
	public float speed = 1.0f;

	[Tooltip("File containing path waypoints, one Vector3 per line - x,y,z")]
	public TextAsset pathFile;

	List<Vector3> wayPoints = new List<Vector3>();
	private int wayPointIndex;
	private Vector3 lastVector;
	private Rigidbody myRigidBody;
	private Vector3 targetVector;
	private float startTime;
	private float journeyLength;

	void Start() {
		// load our waypoints from file
		wayPoints = LoadWayPoints(pathFile);
		// get a reference to our transform
		myRigidBody = GetComponent<Rigidbody> ();
		// set up some vars for moving around our waypoints
		lastVector = myRigidBody.transform.position;
		wayPointIndex = 0;
		targetVector = wayPoints[wayPointIndex];
		startTime = Time.time;
		journeyLength = Vector3.Distance (lastVector, targetVector);
	}

	void FixedUpdate(){
		if (wayPointIndex == wayPoints.Count){
			wayPointIndex = 0;
		}
		if (Vector3.Distance (myRigidBody.transform.position, targetVector) < 1f) {
			targetVector = wayPoints [wayPointIndex];
			wayPointIndex++;
			// reset some "journey" vars
			lastVector = myRigidBody.transform.position;
			startTime = Time.time;
			journeyLength = Vector3.Distance (lastVector, targetVector);
		}
		//myTransform.position += ((targetVector - oldPos) * Time.deltaTime * speed);
		float distCovered = (Time.time - startTime) * speed;
		float fracJourney = distCovered / journeyLength;
		myRigidBody.transform.position = Vector3.Slerp(lastVector, targetVector, fracJourney);

	}

	/// <summary>
	/// Loads way points from a text file.
	/// </summary>
	/// <returns>A List of Vector3s containing the waypoint locations.</returns>
	/// <param name="wayPointFile">Way point file.</param>
	List<Vector3> LoadWayPoints(TextAsset wayPointFile) {
		string pathText = wayPointFile.text;
		string[] wps = pathText.Split ("\n" [0]);
		List<Vector3> tempList = new List<Vector3> ();

		foreach(string wp in wps) {
			string[] points = wp.Split ("," [0]);
			tempList.Add(new Vector3 (float.Parse(points[0]), float.Parse(points[1]), float.Parse(points[2])));
		}

		return tempList;
	}
}

{{< / highlight >}}

There are probably better ways to achieve the same thing, but this is what I ended up with. I'm sure I borrowed bits of the movement code from somewhere (I read a lot of webpages to familairise myself with C# and how to move things around in Unity).


<p class='footnote'>Any code in this post is covered under the <a href='/isc.txt'>ISC license for this site</a>.</p>


[1]: //aie.edu.au
[2]: //en.wikipedia.org/wiki/Galaga
[3]: //en.wikipedia.org/wiki/Space_Invaders
[4]: //en.wikipedia.org/wiki/Raptor:_Call_of_the_Shadows
[5]: //en.wikipedia.org/wiki/Boids
