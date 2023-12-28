<template>
 <header class = "TheHeader">
            <h1>Välkommen till &Bscr;urger Online</h1>
            <img src="https://freewpheaders.com/wp-content/gallery/food-gallery/burger-sandwich-header.jpg" alt="headerPicture">
        </header>
        <main>
            <section id = "burger">
                <h2>Select burger</h2> 
                    <p> This is where you execute burger selection</p>
                     
                    <div class = "wrapper">
                      <Burger v-for="burger in burgers"
                      v-bind:burger="burger" 
                      v-bind:key="burger.name"
                      v-on:orderedBurgers="addToOrder($event)"/>
                    </div>
            </section>
            <section id = "contact">
                <h2>Customer information</h2>
                    <p> This is where you provide necessary information</p>
                        <br />
                        <h3>Delivery information:</h3>
                            <form>
                                <fieldset>
                        <p>
                            <label for="name">Full name</label><br>
                            <input type="text" id="name" v-model="name_data" required="required" placeholder="First- and last name">
                        </p>
                        <p>
                            <label for="email">E-mail</label><br>
                            <input type="email" id="email" v-model="email_data" required="required" placeholder="E-mail adress">
                        </p>
                                
                        <br>
                                </fieldset>
                                <br>
                                <fieldset>
                            <p>
                                <label for="payment">Payment options</label><br>
                                    <br>  
                                <select id="payment" v-model="payment_data">
                                    <option disabled value="">Please select one</option>
                                    <option >Credit card</option>
                                    <option>Swish</option>
                                    <option>Cash</option>
                                    <option>Invoice</option>
                                    <option>Bitcoin</option>
                                </select>
                             </p>


                             
                                </fieldset>
                              
                           
                                <br>
                                
                                <fieldset>
                                  <p>
                                  <label for="map">Please indicate point of delivery: </label>
                                  <br>
                                 </p>
                                  <div class = smallerDiv v-bind:style="{ position: absolute}">
                                  <div id="dots" v-on:click="setLocation" v-bind:style="{ background: 'url(' + require('../../public/img/polacks.jpg')+ ')' }">
                                  <div v-bind:style="{ left: location.x + 'px', 
                                    top: location.y + 'px'}">
                                    T
                                  </div>
                                  </div>
                                  </div>
                                </fieldset>
                                
                                <br>
           
                                <fieldset>
                             <p>
                                <label for="gender">Gender</label><br>
                               
                                <input type="radio" id="dude" v-model="gender_data" checked="checked" value = "Dude">
                                <label for="dude">Dude</label><br>

                                <input type="radio" id="girl" v-model="gender_data" value = "Girl">
                                <label for="dude">Girl</label><br>

                                <input type="radio" id="non_binary" v-model="gender_data" value = "Non-binary">
                                <label for="dude">Non-binary</label><br>

                                <input type="radio" id="notYourBusiness" v-model="gender_data" value = "Not your business">
                                <label for="dude">Not your business</label><br>
                            </p>
                                </fieldset>
                              </form>


            </section>
                        
            

            <button v-on:click = "placeOrder(key)" type="submit">
                Order here
                <img src="https://cdn-icons-png.flaticon.com/512/2933/2933054.png?ga=GA1.1.1227129730.1698922326" style= "width: 20px;">
              </button>
        </main>  






        <hr> 
        <footer>
            &copy; 2023
        </footer>    
</template>

<script>
import Burger from '../components/OneBurger.vue'
import menu from '../assets/menu.json'
import io from 'socket.io-client'

const socket = io();


//const burgerArray = [ {name: "The Flaming Hot Burger", kCal: 495, img: "https://lh3.googleusercontent.com/mArWBpFB5Pq9-46vlSHHDrro1DMOOF7LrfHnhkQv23cJPwOco7LHgk6H19XHYn9dn4y6DBHrKCagVePkyF8JEpY=s1200", lactose: true, gluten: true},
//                 {name: "The Mushy Mushroom Burger", kCal: 395, img: "https://images.hola.com/us/images/0277-159d7d0b1a37-90cef55114f9-1000/horizontal-1200/portobello-mushroom-burger.jpg", lactose: false, gluten: true},
//                 {name: "The Dessert Burger", kCal: 2195, img: "https://www.mashed.com/img/gallery/unusual-dessert-combinations-you-need-to-try-at-least-once/intro-1661283110.webp", lactose: true, gluten: true}
//               ]
//               console.log(burgerArray)

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      name_data:'',
      email_data:'',
      payment_data:'',
      gender_data:'',
      orderedBurgers:{},
      location: { x: 0,
                  y: 0
                }
    }
  },
  methods: {
    placeOrder: function() {
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y },
                                orderItems: this.orderedBurgers,
                                customerInformation: {Name: this.name_data, 
                                                      Email: this.email_data,
                                                      Payment: this.payment_data,
                                                      Gender: this.gender_data },
                              }
                 );
      console.log(this.name_data, this.email_data, this.payment_data,this.gender_data);
    },
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
      console.log(this.orderedBurgers)
    },

    setLocation: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
                    
                    this.location.x = event.clientX-10-offset.x
                    this.location.y = event.clientY-10-offset.y
                    console.log(this.location.x,this.location.y)

    }
  }
}
</script>

<style>
/* så här kommenterar man */
body {
    font-family: Arial, Helvetica, sans-serif;
    font-size: 100%;
 }

 .ingredient {
    font-weight: bold;
}

#burger {
    background-color: #000000;
    color: #ffffff;
    margin: 1em 2em;
    padding: 2em;
    border: 2px dashed #ffffff;
 }

section h2 {
font-size: 2em;
margin: auto auto 0.5em 2em;


 }

 section p, h3 {
    font-size: 1em;
    margin: auto auto 1em 4em;
     }

#contact {
    color: #000000;
    margin: 2em;
    padding: 2em;
    border: 2px dashed #000000;
}
button:hover {
    background-color: rgb(16, 142, 52);
    cursor: pointer;
 }

 button {
    margin: 1em 2em 3em;
}

 button img{
margin: 1em 0 0 0;
}

.TheHeader {
    margin: 0 2em;
    height: 19em;
    overflow: hidden;
    }

.TheHeader img {
    opacity: 0.5;
    width: 100%;
    height: auto;
    }

.TheHeader h1 {
    position: absolute;
    padding: 2em;
    font-size: 3em;
    }

.wrapper {
     display: grid;
     grid-gap: 15px;
     grid-template-columns: 33% 33% 33%;
     background-color: #000000;
 }

 #burger div h3{
     margin: 0 auto 0.5em auto;
     font-size: 1.5em; 
     font-weight: bold;
}

#burger div li{
    margin: 0.5em auto auto 1em;
    font-size: 1em;
    list-style-type: disc;
}

 footer {
    margin: 2em auto 1em 2em;
    }






  #map {
    width: 1920px;
    height: 1078px;
    background: url('../../public/img/polacks.jpg');
  }

  .smallerDiv {
    width: 1600px;
    height: 800px;
    overflow: scroll;
    margin: auto
    
  }



</style>