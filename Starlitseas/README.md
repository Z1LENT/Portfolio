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
     <td width="30%" valign="top" text-align="left"> This shows the front of the portal, with a stone animation and a vortex that circulate around. <br><br> The vortex in it self is a niagara effect that has been put into a blueprint class</td>
    <td ><img src="Image\OutsidePortal.gif" width="75%"/> <br> <i>Outside the portal</td>
  </tr>
    <tr>
     <td width="30%" valign="top" text-align="left"> Enabling the <b> Lateral Stabilizer </b> shows significant improvement, but it still has major flaws. <br><br> It keeps going off course and also tipping over due to the back hovers still having contact with the ground for a split second after jumping, causing the boat to pitch down.   </td>
    <td ><img src="Image\InsidePortal.gif" width="75%"/><br> <i>Inside the portal</td>
</table>

### Links to blueprints

[Dynamical Portal](https://blueprintue.com/blueprint/he4fnv_z/)   
[Attraction Force](https://blueprintue.com/blueprint/1_njixs9/)

## *Juice*

