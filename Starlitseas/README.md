# *Starlit Seas*

<img src="Image\4fwBKb.png" width="100%"/>

[Itch.io page](https://yrgo-game-creator.itch.io/starlit-seas)  

## *A brief game description*

**Starlit Seas** is an enchanting adventure platformer that follows the journey of a deceased marine biologist on her path to heaven. Guided by the very whales she once devoted her life to protecting, she goes on a transcendent journey to the ethereal realms above.

---

## *My contributions to this project*

Below is a summary of my code written to this game, keep in mind that this is a group effort and we co-developed a lot of features, but all the highlighted features below have been implemented by me. 

---

## *Dynamic Portal*

To start off, I wanted to try and make something that was designed to respond to the player's movement, providing an interactive and visually compelling experience. I came across this [tutorial](https://www.youtube.com/watch?v=a7NQOMpuMKk) about an interactive tunnel, containing a niagara effect and particles which with some math and blueprints could read the players whereabouts and connect a portal.

<table>
  <tr>
     <td width="30%" valign="top" text-align="left"> This shows the boat travelling forward without any stabilizing effects. <br><br> The boat is hovering above ground being pushed forward by the thrusters and sideways by gravity </td>
    <td ><img src="Image\OutsidePortal.gif" width="50%"/> <img src="Image\NormalVortex.gif" width="50%"/> <br> <i>Outside the portal</td>
  </tr>
    <tr>
     <td width="30%" valign="top" text-align="left"> Enabling the <b> Lateral Stabilizer </b> shows significant improvement, but it still has major flaws. <br><br> It keeps going off course and also tipping over due to the back hovers still having contact with the ground for a split second after jumping, causing the boat to pitch down.   </td>
    <td ><img src="Image\InsidePortal.gif" width="50%"/><br> <i>Inside the portal</td> 
</table>

