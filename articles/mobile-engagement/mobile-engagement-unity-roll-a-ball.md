<properties
    pageTitle="Ühtsuse tööks sõnu "pall" õpetus"
    description="Juhised klassikaline ühtsuse loomiseks Roll sõnu "pall" mängu, mis on kõik Mobile kaasamine ühtsuse õpetused"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a id="unity-roll-a-ball"></a>Looge ühtsuse tööks pall mängu

Selles õpetuses juhendab põhitoimingud [ühtsuse Roll sõnu "pall" õpetuse](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial)veidi muudetud. Selle valimi mängu koosneb sfäärilise 'player' objekti, mida kasutaja rakenduse kontrollib ja mängu eesmärk on "koguda" laekuva objektide põrgata Playeri objekti nende laekuva objektidega. See eeldab lihtsa ühtsuse editor keskkonna tundmine. Kui teil tekib probleeme, siis peaksite täielik õppeteema. 

### <a name="setting-up-the-game"></a>Kuidas häälestada mängu
Alltoodud juhised kehtivad [ühtsuse õpetus](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)

1. Avage **Ühtsuse redaktor** ja klõpsake nuppu **Uus**. 
    
    ![][51] 
    
2. **Projekti nime** & **asukoht**, valige **3D** ja klõpsake nuppu **Loo projekt**.
    
    ![][52]

3. Äsja loodud uue projekti nime **MiniGame** sarnaselt sees uus stseen vaikimisi salvestamine ** \_stseenide** **varad** alamkaustas:
    
    ![][53]

4. Luua mõne- **3D objekti -> lennuk** nimega võrdsed ja nimega **põhjusel** See lennuk objekti ümbernimetamine

    ![][1]

5. Lähtesta transformatsioon komponent **põhjusel** objekti nii, et see on teinud. 

    ![][3]

6. Tühjendage ruut **Kuva koordinaatvõrk** **Gizmos** menüüst **põhjusel** objekti.

    ![][4]

7. Värskendage objekti **põhjusel** olla mõeldud komponendi **skaala** [X = 2, Y = 1, Y = 2]. 

    ![][5]

8. Lisamine mõne uue- **objekti 3D -> valdkonnas** projekti ja pange selle valdkonnas objekti **partnerina**. 

    ![][6]

9. **Playeri** objekti ja klõpsake nuppu **Lähtesta muuta** sarnane lennuk objekti. 

10. Värskenduse **Transform -> paigutus -> Y-koordinaatide** mängija y arvuna 0,5 osa.  

    ![][7]

11. Looge uus kaust nimega projekti **materjalid** mille loome materjali mängija värv. 

12. Saate luua uue **materjali** nimega **tausta** selles kaustas. 

    ![][8]

13. Värskendage materjali värvi, värskendades **albeedo** vara. Saate valida RGB väärtused [0,32,64]. 

    ![][9]

14. Lohistage see materjal stseeni vaatesse **põhjusel** objekti värvi rakendamiseks. 

    ![][10]

17. Lõpuks värskendada selle **Transform -> pööre Y ->** 60 selgust objektil suunamata lihtversioon. 

    ![][12]

### <a name="moving-the-player"></a>Teisaldamise Playeri
Alltoodud juhised kehtivad [ühtsuse õpetus](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)

1. Lisage **RigidBody** komponent **Playeri** objekti. 

    ![][13]

2. Looge uus kaust nimega **skriptide** projekt. 

3. Klõpsake **lisada komponent -> Uus skript -> C# skript**. Pange failile nimi **PlayerController**, ja klõpsake nuppu **Loo ja lisamine**. See luua ja lisada skripti Playeri objekti.  

    ![][14]

5. See skript projekti kaustas **skriptide** liikumine 

6. Skripti oma lemmik skriptiredaktor redigeerimiseks avada, värskendage skripti koodi järgmine kood ja salvestage see. 

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour 
        {
            public float speed;
            private Rigidbody rb;
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
                rb.AddForce (movement * speed);
            }
        }
    
8. Pange tähele, et ülaltoodud skript kasutab **kiiruse** atribuut. Ühtsuse redaktoris kiiruse atribuudi värskendamiseks 10.  

    ![][15]

9. Tulemus **esitada** ühtsuse Editoris. Nüüd peaks olema võimalik määrata klaviatuuri abil sõnu "pall" ja see peaks pööramine ja liikumine. 

### <a name="moving-the-camera"></a>Liigub kaamera
Alltoodud juhiseid on [ühtsuse õpetuse](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) ning kuvatakse siduda **Kaamera** **mängija** objektile. 

1. Värskendage **Transform.Position** olema X = 0, Y = 10.5, Y =-10.  
2. Värskendage **Transform.Rotation** olema X = 45, Y = 0, Y = 0.  

    ![][16]

2. Uue nimega **CameraController** , et **MainCamera** skripti lisada ja selle skriptide kausta teisaldada. 

    ![][17]

3. Skripti redigeerimiseks avada ja lisage see järgmine kood:

        using UnityEngine;
        using System.Collections;
        
        public class CameraController : MonoBehaviour {
        
            public GameObject player;
        
            private Vector3 offset;
        
            void Start ()
            {
                offset = transform.position - player.transform.position;
            }
            
            void LateUpdate ()
            {
                transform.position = player.transform.position + offset;
            }
        }
    
5. Ühtsuse keskkonnas – lohistage Playeri muutuja Playeri pesa kaamera objekti nii, et kahe on omavahel seotud. 

    ![][18]

6. Nüüd kui tabas Esita ühtsuse redaktoris ja Playeri sõnu "pall" objekti pööramiseks seejärel kuvatakse pärast selle liikumine kaamera.  

### <a name="setting-up-the-play-area"></a>Kuidas häälestada esitusala
[Ühtsuse õppeteema](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141)on alltoodud juhiseid. Loome seinad ümbritseva põhjusel, et mängija sõnu "pall" objekti ei lahkuvad oma liikumisel esitusala. 

1. Klõpsake **-> Loo luua tühja -> mängu objekti** ja pange sellele **seinad**

    ![][19]

2. Jaotises selle seinad objekti - on uue- **objekti 3D -> kuubi** loomine ja nime "Lääne seina". 

    ![][20]

3. **Transform -> paigutus** ja **transformatsioon -> skaala** Lääne seina objekti värskendada. 

    ![][21]

4. Dubleerida Lääne seina luua mõne **Ida seina** värskendatud transformatsioon asukoha ja skaala. 

    ![][22]

5. Ida seina luua **Põhja-seina** värskendatud transformatsioon asukoha ja skaala dubleerida. 

    ![][23]

6. Põhja-seina dubleerimine ja luua **Lõuna seina** värskendatud transformatsioon asukoha ja skaala. 

    ![][24]

### <a name="creating-collectible-objects"></a>Laekuva objektide loomine
[Ühtsuse õppeteema](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141)on alltoodud juhiseid. Loome atraktiivseks ilmega objektid, mis moodustavad laekuva objekte, mis mängija sõnu "pall" objekti kogumiseks, nende kokku määramine. 

1. Looge uus **3D kuubi objekti** ja pange sellele komplekteerimine. 

2. Reguleerige soovitud- **teisenduse -> pööre** & -**teisenduse -> skaala** kättesaamise objekti. 

    ![][25]

3. Luua ja lisada **Uue C# skript** nimega **Rotator** kättesaamise objektile. Veenduge, et luua skripti skriptide kaustas. 

    ![][26]

4. See skript redigeerimiseks avada ja värskendada seda on järgmised: 

        using UnityEngine;
        using System.Collections;
        
        public class Rotator : MonoBehaviour {
        
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }

5. Nüüd tabas Play režiimi ühtsuse meiliredaktori ja oma Kuva kättesaamise objekti pöörata ümber oma telje.

6. Looge uus kaust nimega **Prefabs** 

    ![][27]

7. Lohistage objekti **komplekteerimine** ja panna Prefabs kausta.

    ![][28]

8. Looge uus **tühi mängu objekti** nimega **pickup**. Lähtestage positsioon origin ja lohistage kättesaamise objekti jaotises selle mängu objekti.  

    ![][29]

9. Dubleeritud **komplekteerimine** objekti ja levinud **põhjusel** objekti **Playeri** objekti ümber, värskendades **Transform.Position on X ja Y** -väärtused õigesti. 

    ![][30]

10. Luua **uue materjali** nimetatakse **komplekteerimine** ja värskendage seda olema punane värv, värskendades **albeedo atribuudi** sarnane me ei värskendamise põhjusel objekti. 

    ![][31]

11. 4 kättesaamise objektide materjal rakendada.

    ![][32]

### <a name="collecting-the-pickup-objects"></a>Kogumiseks kättesaamise objektid
[Ühtsuse õppeteema](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141)on alltoodud juhiseid. Uuendame mängija, nii et see oleks võimalik 'koguda' kättesaamise objektide nende kokku. 

1. **PlayerController** skripti, mis on seotud Playeri objekti redigeerimiseks avada ja värskendada seda järgmine:  

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour {
        
            public float speed;
        
            private Rigidbody rb;
        
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
        
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
        
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
        
                rb.AddForce (movement * speed);
            }
        
            void OnTriggerEnter(Collider other) 
            {
                if (other.gameObject.CompareTag ("Pick Up"))
                {
                    other.gameObject.SetActive (false);
                }
            }
        }

2. Loo uus **silt** **Valige ülespoole** (see peab ühtima, mis on skript)  

    ![][33]
    
    ![][34]

3. Selle **sildi** rakendamine ees komplekteerimine objekti. 

    ![][35]

4. Luba **IsTrigger** ruut Prefab objekti.

    ![][36]

5. Jäik keha lisada komplekteerimine ees objekti. Jõudluse optimeerimine uuendame staatilise collider, mida kasutatakse dünaamilise collider abil. 

    ![][37]
  
6. Märkige Lõpetuseks ees objekti atribuudi **IsKinematic** . 

    ![][38]

7. Tabas **esitada** ühtsuse meiliredaktori ja teil on võimalik mängida selles **pall** mängu teisaldamise Playeri objekti suunas sisendi oma klaviatuuri klahvide abil. 

### <a name="updating-the-game-for-mobile-play"></a>Mobiilse Esita mängu värskendamine
Ülaltoodud jaotiste sõlmitud lihtsa õpetuse ühtsuse kaudu. Nüüd me muuta oleks mobiilsideseadme ühendusele panna soovitud aeg. Pange tähele, et kasutasime klaviatuur mängu siiani testimiseks. Nüüd saame seda muuta, nii, et me ei kontrolli mängija, st liikumistee telefoni abil kasutades kiirendusmõõtur sisend. 

**PlayerController** skripti redigeerimiseks avada ja värskendada **FixedUpdate** meetodit kasutada liikumistee kaudu kiirendusmõõtur Playeri objekti teisaldamiseks. 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

Selle õpetuse leiab lihtsa mängu loomine koos ühtsuse ja saate juurutada see mängida teie valitud seadmes. 

<!-- Images -->
[1]: ./media/mobile-engagement-unity-roll-a-ball/1.png  
[2]: ./media/mobile-engagement-unity-roll-a-ball/2.png
[3]: ./media/mobile-engagement-unity-roll-a-ball/3.png
[4]: ./media/mobile-engagement-unity-roll-a-ball/4.png
[5]: ./media/mobile-engagement-unity-roll-a-ball/5.png
[6]: ./media/mobile-engagement-unity-roll-a-ball/6.png
[7]: ./media/mobile-engagement-unity-roll-a-ball/7.png
[8]: ./media/mobile-engagement-unity-roll-a-ball/8.png
[9]: ./media/mobile-engagement-unity-roll-a-ball/9.png  
[10]: ./media/mobile-engagement-unity-roll-a-ball/10.png    
[11]: ./media/mobile-engagement-unity-roll-a-ball/11.png    
[12]: ./media/mobile-engagement-unity-roll-a-ball/12.png
[13]: ./media/mobile-engagement-unity-roll-a-ball/13.png
[14]: ./media/mobile-engagement-unity-roll-a-ball/14.png
[15]: ./media/mobile-engagement-unity-roll-a-ball/15.png
[16]: ./media/mobile-engagement-unity-roll-a-ball/16.png
[17]: ./media/mobile-engagement-unity-roll-a-ball/17.png
[18]: ./media/mobile-engagement-unity-roll-a-ball/18.png
[19]: ./media/mobile-engagement-unity-roll-a-ball/19.png    
[20]: ./media/mobile-engagement-unity-roll-a-ball/20.png    
[21]: ./media/mobile-engagement-unity-roll-a-ball/21.png    
[22]: ./media/mobile-engagement-unity-roll-a-ball/22.png    
[23]: ./media/mobile-engagement-unity-roll-a-ball/23.png    
[24]: ./media/mobile-engagement-unity-roll-a-ball/24.png    
[25]: ./media/mobile-engagement-unity-roll-a-ball/25.png    
[26]: ./media/mobile-engagement-unity-roll-a-ball/26.png    
[27]: ./media/mobile-engagement-unity-roll-a-ball/27.png    
[28]: ./media/mobile-engagement-unity-roll-a-ball/28.png    
[29]: ./media/mobile-engagement-unity-roll-a-ball/29.png    
[30]: ./media/mobile-engagement-unity-roll-a-ball/30.png    
[31]: ./media/mobile-engagement-unity-roll-a-ball/31.png    
[32]: ./media/mobile-engagement-unity-roll-a-ball/32.png    
[33]: ./media/mobile-engagement-unity-roll-a-ball/33.png    
[34]: ./media/mobile-engagement-unity-roll-a-ball/34.png    
[35]: ./media/mobile-engagement-unity-roll-a-ball/35.png    
[36]: ./media/mobile-engagement-unity-roll-a-ball/36.png    
[37]: ./media/mobile-engagement-unity-roll-a-ball/37.png    
[38]: ./media/mobile-engagement-unity-roll-a-ball/38.png    
[51]: ./media/mobile-engagement-unity-roll-a-ball/new-project.png
[52]: ./media/mobile-engagement-unity-roll-a-ball/new-project-properties.png
[53]: ./media/mobile-engagement-unity-roll-a-ball/save-scene.png

    
    
    
    
    
    
    
    
    
    
    
    
