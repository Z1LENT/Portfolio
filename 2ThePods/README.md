# 2 The Pods

<img src="Image\2ThePodsMain.png" width="100%"/>

## A brief game description

**2 The Pods** is an couch co-op platformer which is taking place in a space station about to blow up, you and your friend are stuck and need to make your way to the evacuation pods as soon as possible. One player controls a little astronaut, the other player moves modules around to create a path for the first player. The astronaut has an oxygen gauge that runs down, inciting the players to hurry. There are also a number of deadly traps strewn across the modules for the astronaut to avoid. 

Repository: [<a href="https://github.com/SebbGameCreator/2ThePods">Link</a>]

Itch: [<a href="https://yrgo-game-creator.itch.io/2-the-pods">Link</a>]

---

## My contributions to this project

Below is a summary of my code written to this game, keep in mind that this is a group effort and we co-developed a lot of features, but all the highlighted features below have been implemented by me. 

---

## Tutorial map

By following the instructions from the personal helper FLEV, the player will learn how to interact with the world and gain an understanding of what to look out for.
     
  <img src="Image\Tutorial.gif" alt="Gif showing tutorial map with the personal helper FLEV" width="75%"/>

<i>Click the dropdown below for the full script.</i>

<details>
<summary>TextWriter.cs</summary>
  
  ```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using TMPro;

public class TextWriter : MonoBehaviour
{
    [SerializeField] TextMeshProUGUI textMeshPro;

    public string[] stringArray;

    [SerializeField] float timeBtwnWords;
    [SerializeField] float timeBtwnChars;

    int i = 0;

    private bool finishedWriting = false;
    private int finishedTask = -1;
    private int nextTask;

    AudioSource audioSource;
    DragDrop ddMod1;

    PlayerCheckInModule ddMod2;
    PlayerCheckInModule ddMod3;
    PlayerCheckInModule ddMod5;
    PlayerCheckInModule ddMod8;
    PlayerCheckInModule airDuct;

    Hatches airDuctHatch;
    OxygenMeter oxygen;
    OxygenPickup oxygenCanister;

    public AudioClip textTutorial;

    private List<TaskType> completedTasks = new List<TaskType>();

    public enum TaskType
    {
        None = -1,
        Dragging = 0,
        OpenedHatch = 1,
        OxygenUsed = 2,
        Module2 = 3,
        Module3 = 4,
        Module5 = 5,
        Module8 = 6,
        AirDuctModule = 7,
        FinishLine = 8,
    }

    void Start()
    {
        ddMod1 = GameObject.Find("Module 1 (. Shape)").GetComponent<DragDrop>();
        ddMod2 = GameObject.Find("Module 2 (I Shape)").GetComponentInChildren<PlayerCheckInModule>();
        ddMod3 = GameObject.Find("Module 3 (O Shape)").GetComponentInChildren<PlayerCheckInModule>();
        ddMod5 = GameObject.Find("Module 5 (L Shape)").GetComponentInChildren<PlayerCheckInModule>();
        ddMod8 = GameObject.Find("Module 8 (Z Shape)").GetComponentInChildren<PlayerCheckInModule>();
        airDuct = GameObject.Find("Airduct Red 2 Left").GetComponentInChildren<PlayerCheckInModule>();

        airDuctHatch = GameObject.Find("Airduct Red 1 Right").GetComponentInChildren<Hatches>();
        oxygen = GameObject.FindWithTag("Oxygen").GetComponent<OxygenMeter>();
        oxygenCanister = GameObject.Find("Oxygen Canister").GetComponent<OxygenPickup>();
        audioSource = GetComponent<AudioSource>();
        oxygen.paused = true;
        EndCheck(0);
    }

    void Update()
    {
        if (!completedTasks.Contains(TaskType.Dragging) && ddMod1.isDragging)
        {
            StartTask(TaskType.Dragging, TaskType.OpenedHatch);
        }
        else if (!completedTasks.Contains(TaskType.OpenedHatch) && airDuctHatch.openedHatch)
        {
            StartTask(TaskType.OpenedHatch, TaskType.OxygenUsed);
        }
        else if (!completedTasks.Contains(TaskType.OxygenUsed) && oxygenCanister.isUsed)
        {
            StartTask(TaskType.OxygenUsed, TaskType.Module2);
        }
        else if (!completedTasks.Contains(TaskType.Module2) && ddMod2.runnerInModule)
        {
            StartTask(TaskType.Module2, TaskType.Module3);
        }
        else if (!completedTasks.Contains(TaskType.Module3) && ddMod3.runnerInModule)
        {
            StartTask(TaskType.Module3, TaskType.Module5);
        }
        else if (!completedTasks.Contains(TaskType.Module5) && ddMod5.runnerInModule)
        {
            StartTask(TaskType.Module5, TaskType.Module8);
        }
        else if (!completedTasks.Contains(TaskType.Module8) && ddMod8.runnerInModule)
        {
            StartTask(TaskType.Module8, TaskType.AirDuctModule);
        }
        else if (!completedTasks.Contains(TaskType.AirDuctModule) && airDuct.runnerInModule)
        {
            StartTask(TaskType.AirDuctModule, TaskType.FinishLine);
        }
    }

    void StartTask(TaskType current, TaskType next)
    {
        completedTasks.Add(current);
        finishedTask = (int)current;
        nextTask = (int)next;
        TextSwitch(nextTask);
        if (current == TaskType.OpenedHatch)
            oxygen.paused = false;
    }

    void TextSwitch(int text)
    {
        EndCheck(text);
    }

    public void EndCheck(int text)
    {
        textMeshPro.text = stringArray[text];
        StartCoroutine(TextVisible());
        finishedWriting = false;
    }

    private IEnumerator TextVisible()
    {
        textMeshPro.ForceMeshUpdate();
        int totalVisibleCharacters = textMeshPro.textInfo.characterCount;
        int counter = 0;

        while (true)
        {
            int visibleCount = counter % (totalVisibleCharacters + 1);
            textMeshPro.maxVisibleCharacters = visibleCount;

            audioSource.PlayOneShot(textTutorial);

            if (visibleCount >= totalVisibleCharacters)
            {
                finishedWriting = true;
                audioSource.Stop();
                break;
            }
            counter += 1;
            yield return new WaitForSeconds(timeBtwnChars);
        }
    }
}
```
</details>

<details>
  <summary>TextFader.cs</summary>
  
  ```csharp
using UnityEngine;
using TMPro;

public class TextFader : MonoBehaviour
{
    public float blinkDuration = 1.0f;
    public float fadeDuration = 1.0f;

    private float timer;
    private int blinkCount;
    private bool isFadingOut;

    public TextMeshProUGUI start;
    public TextMeshProUGUI end;
    public TextMeshProUGUI level;

    void Start()
    {
        timer = 0.0f;
        blinkCount = 0;
        isFadingOut = false;
    }

    void Update()
    {
        if (blinkCount >= 5)
        {
            FadeOutText();
            return;
        }

        timer += Time.deltaTime;

        if (isFadingOut)
        {
            FadeOutTextEffect();
        }
        else
        {
            FadeInTextEffect();
        }

        if (timer >= blinkDuration)
        {
            isFadingOut = !isFadingOut;

            timer = 0.0f;

            if (isFadingOut)
            {
                blinkCount++;
            }
        }
    }

    void FadeInTextEffect()
    {
        float alpha = timer / fadeDuration;
        alpha = Mathf.Clamp01(alpha);

        if (start != null)
        {
            start.color = new Color(start.color.r, start.color.g, start.color.b, alpha);
        }

        if (end != null)
        {
            end.color = new Color(end.color.r, end.color.g, end.color.b, alpha);
        }

        if (level != null)
        {
            level.color = new Color(level.color.r, level.color.g, level.color.b, alpha);
        }

    }

    void FadeOutTextEffect()
    {
        float alpha = 1.0f - (timer / fadeDuration);
        alpha = Mathf.Clamp01(alpha);

        if (start != null)
        {
            start.color = new Color(start.color.r, start.color.g, start.color.b, alpha);
        }

        if (end != null)
        {
            end.color = new Color(end.color.r, end.color.g, end.color.b, alpha);
        }

        if (level != null)
        {
            level.color = new Color(level.color.r, level.color.g, level.color.b, alpha);
        }

    }

    void FadeOutText()
    {
        timer += Time.deltaTime;

        float alpha = 1.0f - (timer / fadeDuration);
        alpha = Mathf.Clamp01(alpha);

        if (start != null)
        {
            start.color = new Color(start.color.r, start.color.g, start.color.b, alpha);
        }

        if (end != null)
        {
            end.color = new Color(end.color.r, end.color.g, end.color.b, alpha);
        }

        if (level != null)
        {
            level.color = new Color(level.color.r, level.color.g, level.color.b, alpha);
        }

        if (alpha <= 0.0f)
        {
            if (start != null)
            {
                start.enabled = false;
            }

            if (end != null)
            {
                end.enabled = false;
            }

            if (level != null)
            {
                level.enabled = false;
            }
        }
    }
}
```
</details>

---
## Module drag/drop system

The builder needed to be able to pick up an module and drag and drop it wherever they wanted, which is why I created a grid droparea formed like cubes for the modules to stick on to. 
Now with a little bit of code, the builder could raycast is way to find a module inside of this grid to either move or drop it somewhere else. The modules will also snap differently according to their pivot points since they all have different scales and shapes which we had to take in mind so the modules were not colliding with each other.

<img src="Image\GridSystem.png" alt="Gif showing droparea for modules" width="75%"/>

<i>Click the dropdown below for the full script.</i>

<details>
  <summary>DragDrop.cs</summary>

  ```csharp
using UnityEngine;

public class DragDrop : MonoBehaviour
{
    [HideInInspector] public Vector3 offset;
    private Vector3 destinationPosition;
    private Vector3 builderPos;

    private Transform gridFixPoint;

    [HideInInspector] public bool isMarked = false;
    [HideInInspector] public bool isDragging = false;
    [HideInInspector] public bool isReleased = false;
    [HideInInspector] public bool isDraggable = true;
    [HideInInspector] public bool isRotatable = true;
    [HideInInspector] public bool isMarkable = true;
    [HideInInspector] public bool canDrop = true;
    [HideInInspector] public bool released = false;
    [HideInInspector] public bool floating = true;
    [HideInInspector] public bool rotating = false;
    [HideInInspector] public bool coolDown = false;

    [HideInInspector] public Collider2D col;
    [HideInInspector] public Rigidbody2D rb;
    [HideInInspector] public SpriteRenderer spriteRenderer;

    [Range(5, 20)] public float coolDownTime = 10f;

    private GameObject[] builder = new GameObject[2];
    private BuilderInput[] builderInput = new BuilderInput[2]; 
    private PlayerCheckInModule playerChecker;

    private string destinationTag = "DropArea";
    private string sortingLayerName = "Module";

    private float _currentRotation;
    private float _lerpTime;
    private float _coolDownTimer;

    private int _index;
    private int _currentBuilder;
    private int _defaultBuilder;

    private const int UI = 32;
    private const int RUNNER = 64;
    private const int MODULE = 8;
    private const int WALL = 11;

    private bool _airduct = false;
    private bool _cancelRotation = false;
    private bool _noRedBuilder;
    
    
    
    private void Start()
    {
        rb = GetComponent<Rigidbody2D>();
        col = GetComponent<Collider2D>();
        spriteRenderer = GetComponent<SpriteRenderer>();
        gridFixPoint = transform.GetChild(2).transform;
        foreach (Transform child in this.transform) if (child.tag == "PlayerChecker") playerChecker = child.gameObject.GetComponent<PlayerCheckInModule>();

        GetBuilder();

        if (this.tag == "Airduct") _airduct = true;
        if (_airduct)
        {
            rb.bodyType = RigidbodyType2D.Static;
            floating = false;
            isReleased = true;
            released = true;
            spriteRenderer.sortingOrder = 2;
        }
        else SetRandomVelocity();
    }

    private void SetRandomVelocity()
    {
        float randomX = Random.Range(-0.1f, 0.1f);
        float randomY = Random.Range(-0.1f, 0.1f);

        Vector2 randomVelocity = new Vector2(randomX, randomY);
        rb.velocity = randomVelocity;
    }

    private void Update()
    {
        if (!coolDown)
        {
            if (isMarked && isMarkable)
                Mark();
            Drag();
            if (isDraggable && !isMarked && isReleased && !rotating)
                Release();
        }
        else
        {
            _coolDownTimer += Time.deltaTime;
            if (_coolDownTimer > coolDownTime)
            {
                coolDown = false;
                builderInput[_defaultBuilder].CoolDown(_index, coolDown);
                _coolDownTimer = 0;
            }
        }
    }

    private void FixedUpdate()
    {
        if (_cancelRotation)
        {
            _currentRotation = Mathf.Lerp(_currentRotation, builderInput[_currentBuilder].originalRotation, _lerpTime);
            _lerpTime += Time.fixedDeltaTime;
            rb.bodyType = RigidbodyType2D.Dynamic;
            rb.MoveRotation(Quaternion.Euler(0, 0, _currentRotation));
            if (Mathf.Abs(_currentRotation - builderInput[_currentBuilder].originalRotation) < 0.05f)
            {
                _currentRotation = builderInput[_currentBuilder].originalRotation;
                rb.freezeRotation = true;
                _cancelRotation = false;
                if (!isDragging) rb.bodyType = RigidbodyType2D.Static;
            }
        }
    }

    void GetBuilder()
    {
        builder[0] = GameObject.Find("PlayerBuilder_Red(Clone)");
        builder[1] = GameObject.Find("PlayerBuilder_Blue(Clone)");

        if (builder[0] == null && builder[1] == null) return;
        else if (builder[0] == null)
        {
            _noRedBuilder = true;
            builderInput[1] = builder[1].GetComponent<BuilderInput>();
        }
        else
        {
            builderInput[0] = builder[0].GetComponent<BuilderInput>();
            _noRedBuilder = false;
            if (builder[1] != null) builderInput[1] = builder[1].GetComponent<BuilderInput>();
        }
        _defaultBuilder = _noRedBuilder.GetHashCode();
        _index = builderInput[_defaultBuilder].modules.IndexOf(this.gameObject);
    }

    public void BuilderIndex(int index)
    {
        _currentBuilder = index;
    }

    private void Mark()
    {
        if (_airduct) return;
        if (playerChecker.runnerInModule) return;

        if (builder[0] == null && builder[1] == null) GetBuilder();
        
        if (isDraggable && isMarked)
        {
            released = false;
            isMarkable = false;
            builderPos = builder[_currentBuilder].transform.position;
            offset = transform.position - builderPos;
            isDragging = true;
            SetSortingOrder(1);
            rb.bodyType = RigidbodyType2D.Dynamic;
            if (floating) floating = false;
        }
    }

    private void Drag()
    {
        if (isDraggable && isDragging && isMarked)
        {
            builderPos = builder[_currentBuilder].transform.position;
            rb.MovePosition(builderPos + offset);
        }
    }

    private void Release()
    {
        isDragging = false;
        Vector3 correction = new Vector3(gridFixPoint.position.x - rb.position.x, gridFixPoint.position.y - rb.position.y, 0f);
        
        if (canDrop)
        {
            NearestValidPosition(correction);
        }
        else                                        // Add conditions and code for !canDrop
        {
            NearestValidPosition(correction);       
        }

        SetSortingOrder(0);
        int rotation = Mathf.RoundToInt(rb.rotation);
        rotation /= 90;
        rotation *= 90;
        rb.rotation = rotation;
        canDrop = true;
        isMarkable = true;
        isReleased = false;
        if (_airduct)
        {
            SetSortingOrder(2);
        }
        rb.bodyType = RigidbodyType2D.Static;
        released = true;
        if (_index >= 0)
        {
            coolDown = true;
            builderInput[_defaultBuilder].CoolDown(_index, coolDown);
        }
    }

    private void NearestValidPosition(Vector3 correction)
    {
        RaycastHit2D hitInfo = Physics2D.Raycast(gridFixPoint.position, Vector2.zero, Mathf.Infinity, UI);
        if (hitInfo.transform != null && hitInfo.collider.tag == destinationTag)
        {
            destinationPosition = hitInfo.transform.position - correction;
            foreach (Transform child in this.transform)
            {
                if (child.tag == "InteractableObject")
                {
                    child.position += (destinationPosition - transform.position);
                    child.position = new Vector3(child.position.x, child.position.y, 0);
                }
            }
            rb.position = destinationPosition;
        }
    }

    private void SetSortingOrder(int order)
    {
        if (spriteRenderer != null)
        {
            spriteRenderer.sortingLayerName = sortingLayerName;
            spriteRenderer.sortingOrder = order;
        }
    }

    private void OnCollisionEnter2D(Collision2D collision)
    {
        if (_airduct) return;
        if (collision.gameObject.layer == WALL  && rotating && !_cancelRotation || collision.gameObject.layer == MODULE && rotating && !_cancelRotation)
        {
            rotating = false;
            rb.bodyType = RigidbodyType2D.Dynamic;
            _cancelRotation = true;
            _currentRotation = rb.rotation;
            _lerpTime = 0;
        }
    }

    private void OnTriggerEnter2D(Collider2D collision)
    {
        if (collision.gameObject.layer == RUNNER)
        {
            isDraggable = false;
        }
    }

    private void OnTriggerExit2D(Collider2D collision)
    {
        if (collision.gameObject.layer == RUNNER)
        {
           isDraggable = true;
        }
    }

}
  ```
</details>

--- 

## Main menu

So the main menu was more or less pretty rushed since I kind of got thrown into it during the last weeks before deadline but I managed to make a level system so the player could choose which level to load into, and unlock new levels once completing the previous levels.

<img src="Image\StartMenu.gif" alt="Gif showing start menu" width="75%"/>

<i>Click the dropdown below for the full script.</i>

<details>

 <summary>LevelMenu.cs</summary>

```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;

public class LevelMenu : MonoBehaviour
{
    public Button[] buttons;

    private void Awake()
    {
        int unlockedlevel = PlayerPrefs.GetInt("UnlockedLevel", 2);
        for (int i = 0; i < buttons.Length; i++)
        {
            buttons[i].interactable = false;
        }
        for (int i = 0; i < unlockedlevel; i++)
        {
            buttons[i].interactable |= true;
        }
    }
    public void Openlevel(int levelId)
    {
        string levelName = "Level " +  levelId;
        SceneManager.LoadScene(levelName);
    }
}
```
 
</details>



## I also worked with

<b>Camera size shift </b>

Basically the camera can switch between different FOV, either you can watch from a builder perspective to easier move around modules or you can switch to player view so the camera zooms in and follows the players movement.

<b>Level Design </b>

I know it is not much to do for level design since it is actually meant for you as player to build your own, but I placed around everything on the maps and fixed all the colliders for the modules and the levels.

<b> Level transition </b> 

I created two hatches that open or closes which act like a loading screen.

<img src="Image\Tutorial2.gif" alt="Gif showing start menu" width="75%"/>

<i>Click the dropdown below for the full script.</i>

<details>

 <summary>MoveHatches.cs</summary>

```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class MoveHatches : MonoBehaviour
{
    public float speed;
    Vector3 targetPos;

    public GameObject ways;
    public Transform[] wayPoints;

    int pointIndex;
    int pointCount;
    int direction = 1;

    private void Awake()
    {
        wayPoints = new Transform[ways.transform.childCount];
        for (int i = 0; i < ways.gameObject.transform.childCount; i++)
        {
            wayPoints[i] = ways.transform.GetChild(i).gameObject.transform;
        }
    }

    private void Start()
    {
        pointCount = wayPoints.Length;
        pointIndex = 1;
        targetPos = wayPoints[pointIndex].transform.position;
    }

    private void Update()
    {
        var step = speed * Time.deltaTime;
        transform.position = Vector3.MoveTowards(transform.position, targetPos, step);
    }

    public void MoveBackToStart()
    {
        pointIndex = 0;
        targetPos = wayPoints[pointIndex].transform.position;
    }
}
```
 
</details>

---

## [Back to Main Page](https://github.com/Z1LENT/Portfolio/blob/main/README.md)
