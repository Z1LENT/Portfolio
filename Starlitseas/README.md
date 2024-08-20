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
    <td ><img src="Images\DustyDeliveries_NoStabilizer.gif" /> <br> <i>No stabilizer</td>
  </tr>
    <tr>
     <td width="30%" valign="top" text-align="left"> Enabling the <b> Lateral Stabilizer </b> shows significant improvement, but it still has major flaws. <br><br> It keeps going off course and also tipping over due to the back hovers still having contact with the ground for a split second after jumping, causing the boat to pitch down.   </td>
    <td ><img src="Images\DustyDeliveries_LateralStabilizer.gif" /><br> <i>With Lateral stabilizer</td> 
  </tr>
      <tr>
     <td width="30%" valign="top" text-align="left"> To keep it from spinning out of control, I added a <b> Yaw Stabilizer</b>. <br><br> This keeps the boats course straight. But we still have the problem of the boat tipping over causing hard crashes into the sand. </td>
    <td ><img src="Images\DustyDeliveries_YawStabilizer.gif" /><br> <i>With Lateral stabilizer and Yaw Stabilizer</td>
  </tr>
    </tr>
      <tr>
     <td width="30%" valign="top" text-align="left"> Finally, I added an <b> Air Stabilizer </b> that tries to keep the boat upright. Also to handle the rough landings this component looks at the grounds normal and tries to align accordingly. <br> It does this by projecting each hover components distance to a plane, then feeding the error into a <a href="https://en.wikipedia.org/wiki/Proportional%E2%80%93integral%E2%80%93derivative_controller">PID Controller</a> to apply a countering force. </td>
    <td ><img src="Images\DustyDeliveries_AirStabilizer.gif" /> <br><i>With Lateral stabilizer, Yaw Stabilizer and Air Stabilizer (and more speed)</td>
  </tr>
</table>

