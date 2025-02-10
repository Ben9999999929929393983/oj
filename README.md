using UnityEngine;
using System.Collections;

public class CarController : MonoBehaviour
{
    public float speed = 20f;
    public float turnSpeed = 50f;
    public GameObject explosionEffect;
    public int level = 1;
    public int money = 0;
    public int carHealth = 100;
    public GameObject[] bmwModels;

    void Update()
    {
        float move = Input.GetAxis("Vertical") * speed * Time.deltaTime;
        float turn = Input.GetAxis("Horizontal") * turnSpeed * Time.deltaTime;
        
        transform.Translate(0, 0, move);
        transform.Rotate(0, turn, 0);
    }
    
    void OnCollisionEnter(Collision collision)
    {
        carHealth -= 20;
        if (carHealth <= 0)
        {
            Instantiate(explosionEffect, transform.position, transform.rotation);
            Destroy(gameObject);
            RestartLevel();
        }
    }
    
    void RestartLevel()
    {
        // Code zum Neustart des Levels
        level++;
        carHealth = 100;
        transform.position = new Vector3(0, 1, 0);
    }
    
    public void UpgradeCar()
    {
        if (money >= 100)
        {
            speed += 5;
            turnSpeed += 2;
            money -= 100;
        }
    }
    
    public void BuyNewCar(int carIndex)
    {
        if (money >= 500 && carIndex < bmwModels.Length)
        {
            Instantiate(bmwModels[carIndex], transform.position, transform.rotation);
            Destroy(gameObject);
            money -= 500;
        }
    }
}
