<template>
  <div class="checkout-container">
    <div class="product">
      <h3>{{product.name}} </h3>
      <p>{{product.currency}}{{product.price}} </p>
      <img :src="product.image" :alt="product.name">
    </div>
    <form class="credit-card_body">
      <div class="credit-card_front_side"
           :class="{'card-flip_right' : cardFlip.front,'card-show_left card-show_front_side': cardFlip.back}">
        <svg class="card-chip" xmlns="http://www.w3.org/2000/svg" fill="#999" width="251.445" height="197.12" viewBox="0 0 251.445 197.12">
          <path id="Path_1" data-name="Path 1" d="M382.76,318.25a5.857,5.857,0,0,1-5.824,5.824H323.4a5.857,5.857,0,0,1-5.824-5.824V241.418a5.857,5.857,0,0,1,5.824-5.824h53.535a5.819,5.819,0,0,1,5.824,5.824Zm-77.336-76.887V266c0,4.145-2.687,7.672-5.824,7.672H224.448V220.867c0-.336.055-.617.055-.953h75.1c3.137,0,5.824,3.527,5.824,7.672Zm-80.977,97.383V285.937l75.152,0c3.137,0,5.824,3.527,5.824,7.672v38.418c0,4.145-2.687,7.672-5.824,7.672l-75.1,0c0-.336-.055-.617-.055-.953Zm251.44-117.88v52.809H400.735c-3.137,0-5.824-3.527-5.824-7.672V227.585c0-4.145,2.688-7.672,5.824-7.672h75.1c0,.336.055.676.055.953Zm-80.918,97.383V293.608c0-4.145,2.688-7.672,5.824-7.672h75.1v52.809c0,.336-.055.617-.055.953h-75.1c-3.137,0-5.824-3.527-5.824-7.672V318.249ZM264.1,181.279h172.14a39.668,39.668,0,0,1,37.352,26.488H400.736c-8.961,0-16.352,7.281-17.754,16.742a18.177,18.177,0,0,0-6.105-1.121H323.341a18.452,18.452,0,0,0-6.105,1.121c-1.281-9.465-8.676-16.742-17.633-16.742H226.744A39.666,39.666,0,0,1,264.1,181.279ZM436.239,378.4H264.1a39.668,39.668,0,0,1-37.352-26.488H299.6c8.961,0,16.352-7.281,17.754-16.742a18.177,18.177,0,0,0,6.105,1.121H377a18.452,18.452,0,0,0,6.105-1.121c1.344,9.465,8.734,16.742,17.754,16.742h72.855A39.935,39.935,0,0,1,436.238,378.4Z" transform="translate(-224.446 -181.279)"/>
        </svg>

        <label for="cc-cname">Name on Card</label>
        <input id="cc-name"
               type="text"
               autocapitalize="on"
               autofocus
               tabindex="0"
               spellcheck="false"
               class="credit-card_name"
               placeholder="NAME SURNAME"
               v-model="cardData.name">

        <label for="cc-number">Credit card number</label>
        <div class="credit-card_numbers">
          <input
              ref="num"
              type="tel"
              maxlength="4"
              placeholder="0000"
              spellcheck="false"
              v-for="(num, index) in numHolder"
              @input="focusNext(index)"
              @change="cardNumber"
              v-model="this.numHolder[index].numb"
              @focus="$event.target.select()"
              :key="index">
        </div>

        <label>Expiration Date</label>
        <input ref="date"
               type="tel"
               maxlength="5"
               class="credit-card_date"
               @input="()=>{dateSplitter(); flipBack()}"
               v-model="cardData.date"
               placeholder="MM/YY">
        <svg v-show="readyToFlip" class="input-confirm" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
          <path d="M243.8 339.8C232.9 350.7 215.1 350.7 204.2 339.8L140.2 275.8C129.3 264.9 129.3 247.1 140.2 236.2C151.1 225.3 168.9 225.3 179.8 236.2L224 280.4L332.2 172.2C343.1 161.3 360.9 161.3 371.8 172.2C382.7 183.1 382.7 200.9 371.8 211.8L243.8 339.8zM512 256C512 397.4 397.4 512 256 512C114.6 512 0 397.4 0 256C0 114.6 114.6 0 256 0C397.4 0 512 114.6 512 256zM256 48C141.1 48 48 141.1 48 256C48 370.9 141.1 464 256 464C370.9 464 464 370.9 464 256C464 141.1 370.9 48 256 48z"/>
        </svg>
      </div>
      <div class="credit-card_back_side"
           :class="{'card-show_back_side': cardFlip.front, 'card-flip_left': cardFlip.back}">

        <svg class="card-chip" xmlns="http://www.w3.org/2000/svg" fill="#999" width="251.445" height="197.12" viewBox="0 0 251.445 197.12">
          <path id="Path_1" data-name="Path 1" d="M382.76,318.25a5.857,5.857,0,0,1-5.824,5.824H323.4a5.857,5.857,0,0,1-5.824-5.824V241.418a5.857,5.857,0,0,1,5.824-5.824h53.535a5.819,5.819,0,0,1,5.824,5.824Zm-77.336-76.887V266c0,4.145-2.687,7.672-5.824,7.672H224.448V220.867c0-.336.055-.617.055-.953h75.1c3.137,0,5.824,3.527,5.824,7.672Zm-80.977,97.383V285.937l75.152,0c3.137,0,5.824,3.527,5.824,7.672v38.418c0,4.145-2.687,7.672-5.824,7.672l-75.1,0c0-.336-.055-.617-.055-.953Zm251.44-117.88v52.809H400.735c-3.137,0-5.824-3.527-5.824-7.672V227.585c0-4.145,2.688-7.672,5.824-7.672h75.1c0,.336.055.676.055.953Zm-80.918,97.383V293.608c0-4.145,2.688-7.672,5.824-7.672h75.1v52.809c0,.336-.055.617-.055.953h-75.1c-3.137,0-5.824-3.527-5.824-7.672V318.249ZM264.1,181.279h172.14a39.668,39.668,0,0,1,37.352,26.488H400.736c-8.961,0-16.352,7.281-17.754,16.742a18.177,18.177,0,0,0-6.105-1.121H323.341a18.452,18.452,0,0,0-6.105,1.121c-1.281-9.465-8.676-16.742-17.633-16.742H226.744A39.666,39.666,0,0,1,264.1,181.279ZM436.239,378.4H264.1a39.668,39.668,0,0,1-37.352-26.488H299.6c8.961,0,16.352-7.281,17.754-16.742a18.177,18.177,0,0,0,6.105,1.121H377a18.452,18.452,0,0,0,6.105-1.121c1.344,9.465,8.734,16.742,17.754,16.742h72.855A39.935,39.935,0,0,1,436.238,378.4Z" transform="translate(-224.446 -181.279)"/>
        </svg>
        <label>CVV PIN</label>
        <input
            ref="cvv"
            v-model="cardData.cvv"
            maxlength="3"
            type="tel"
            @input="flipFront()"
            class="credit-card_cvv"
            placeholder="CVV">

        <svg v-show="cardData.cvv.length >= 3" class="input-confirm" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
          <path d="M243.8 339.8C232.9 350.7 215.1 350.7 204.2 339.8L140.2 275.8C129.3 264.9 129.3 247.1 140.2 236.2C151.1 225.3 168.9 225.3 179.8 236.2L224 280.4L332.2 172.2C343.1 161.3 360.9 161.3 371.8 172.2C382.7 183.1 382.7 200.9 371.8 211.8L243.8 339.8zM512 256C512 397.4 397.4 512 256 512C114.6 512 0 397.4 0 256C0 114.6 114.6 0 256 0C397.4 0 512 114.6 512 256zM256 48C141.1 48 48 141.1 48 256C48 370.9 141.1 464 256 464C370.9 464 464 370.9 464 256C464 141.1 370.9 48 256 48z"/>
        </svg>

      </div>
      <div class="credit-card_payment_buttons">
        <button class="btn-cancel" type="button" @click="clear()">Cancel</button>
        <button class="btn-confirm" type="button" @click="cardFlipper()" :class="{'btn-active': readyToCheckout}">Confirm Payment {{product.currency}}{{product.price}}</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return{
      cardData:{
        name: "",
        number:"",
        date: "",
        cvv: ""
      },
      numHolder:[
        {name: "aaaa", numb: ""},
        {name: "bbbb", numb: ""},
        {name: "cccc", numb: ""},
        {name: "dddd", numb: ""}
      ],
      cardFlip: {
        front: null,
        back: null,
      },
      readyToFlip: false
    }
  },
  methods: {
    focusNext(index) {
      if(index < 3 && this.numHolder[index].numb.length >= 4) {
        this.$refs.num[index + 1].focus()
      } else if(index === 3 && this.numHolder[index].numb.length >= 4) {
        this.$refs.date.focus()
      }
    },
    flipBack() {
      let i = this.cardData
      if(i.name && i.number.length >= 16 && i.date.length >= 5 ) {
        this.readyToFlip = true
        this.cardFlipper()
        this.$refs.cvv.focus()
      }
    },
    flipFront() {
      let i = this.cardData
      if (i.cvv.length >= 3) {
        this.cardFlipper()
      }
    },
    dateSplitter() {
      if(this.cardData.date.length === 2) {
        this.cardData.date = this.cardData.date + "/"
      }
    },
    cardFlipper() {
      setTimeout(()=>{
        if(this.cardFlip.front) {
          this.cardFlip.front = false
          this.cardFlip.back = true
        } else {
          this.cardFlip.front = true
          this.cardFlip.back = false
        }
      },700)
    },
    cardNumber() {
      this.cardData.number = this.numHolder[0].numb + this.numHolder[1].numb + this.numHolder[2].numb + this.numHolder[3].numb
    },
    clear() {
      this.cardData.name = ""
      this.cardData.number = ""
      this.cardData.date = ""
      this.cardData.cvv = ""
      this.numHolder[0].numb = ""
      this.numHolder[1].numb = ""
      this.numHolder[2].numb = ""
      this.numHolder[3].numb = ""
      this.readyToFlip = false
    }
  },
  props: {
    product: Object,
  },
  computed: {
    readyToCheckout(){
      return !!(this.cardData.number.length >= 16 && this.cardData.name.length && this.cardData.date.length >= 5 && this.cardData.cvv.length >= 3)
    }
  }
}
</script>
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Rubik:wght@300;400;500&display=swap');

.checkout-container {
  background: #e5e5e5;
  width: 45rem;
  height: 20rem;
  display: flex;
  justify-content: space-between;
  padding: 1.5rem 2rem;
  border-radius: 1.5rem;
}
.credit-card_body {
  position: relative;
  perspective: 100rem;
  top: -4rem;
  --duration: .3s;
}

/* Card Designs */
.credit-card_front_side {
  position: absolute;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: start;
  width: 25.5rem;
  max-width: 95vw;
  height: 15rem;
  padding: 1rem 1.5rem;
  border-radius: 1rem;
  transform: rotate3d(0,1,0,0deg);
  box-shadow: rgba(0, 0, 0, 0.45) 0 25px 20px -10px;
  animation: blur-in var(--duration) ease var(--duration) forwards;
  background: radial-gradient(circle at center ,#00000011, #00000011 10%, transparent 10%, transparent  20%, #00000011 20%, #00000011 30%, transparent 30%, transparent 40%, #00000011 40%, #00000011 50%, transparent 50%, transparent 60%, #00000011 60%, #00000011 70%, transparent 70%, transparent 80%, #00000011 80%, #00000011 90%, transparent 90%);
  background-size: 40em 40em;
  background-color: rgb(130, 8, 239);
}
.credit-card_front_side:before {
  content: "";
  position: absolute;
  inset:0;
  background: linear-gradient(135deg, rgba(255, 99, 2, 0.7) 0%, rgba(255, 2, 204, 0.4) 35%, rgba(252,0,255,0) 55%, rgba(5, 236, 147, 0.6) 100%);
  mix-blend-mode: overlay;
  z-index: -1;
}

.credit-card_back_side {
  overflow: hidden;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: start;
  width: 25.5rem;
  max-width: 95vw;
  height: 15rem;
  padding: 1rem 1.5rem;
  border-radius: 1rem;
  transform: rotate3d(0, 1, 0, -90deg);
  box-shadow: rgba(0, 0, 0, 0.45) 0 25px 20px -10px;
  animation: blur-in var(--duration) ease var(--duration) forwards;
  background: #5d6164;
  opacity: 1
}
.credit-card_back_side::before {
  content:"";
  position: absolute;
  inset:0;
  top:3rem;
  height: 4rem;
  z-index: -1;
  background: rgba(0, 0, 0, 0.15);
}
.credit-card_back_side:after {
  content: "";
  position: absolute;
  inset:0;
  z-index: -1;
  background: linear-gradient(47deg, rgba(29, 18, 59, 0.34) 0%, rgba(252,0,255,0) 55%, rgba(108, 96, 126, 0.53) 100%);
}
.credit-card_date {
  width: 7rem !important;
}
.product p {
  font-size: 2rem;
  font-weight: 300;
}
/* Transition classes */
.card-flip_right {
  transform: rotate3d(0,1,0,90deg);
  transition-duration: var(--duration);
  transition-timing-function: ease-in;
  z-index: -1;
  animation: blur-out var(--duration) ease-in forwards;
  background-color: #222;
  opacity: 1
}
.card-flip_left {
  transform: rotate3d(0,1,0,-90deg);
  transition-duration: var(--duration);
  transition-timing-function: ease-in;
  z-index: -1;
  animation: blur-out var(--duration) ease forwards;
  background-color: #222;
}
.card-show_back_side {
  transform: rotate3d(0,1,0,0deg);
  transition-duration: var(--duration);
  transition-delay: var(--duration);
  transition-timing-function: ease-out;
  z-index: 1;
}
.card-show_front_side {
  transform: rotate3d(0,1,0,0deg);
  transition-duration: var(--duration);
  transition-delay: var(--duration);
  transition-timing-function: ease-out;
  z-index: 1;
}
@keyframes blur-out {
  to {filter: blur(.2rem); box-shadow: rgba(0, 0, 0, 0) 0 25px 20px -10px;}
}
@keyframes blur-in {
  from {filter: blur(.2rem); box-shadow: rgba(0, 0, 0, 0) 0 25px 20px -10px;}
}

.credit-card_front_side > .card-chip {
  position: absolute;
  top:1rem;
  height: 3rem;
  right: 1.5rem;
  width: 3rem;
  fill: rgba(248, 155, 61, 0.79);
}
.credit-card_back_side > .card-chip {
  position: absolute;
  top:1rem;
  height: 3rem;
  left: 1.5rem;
  width: 3rem;
  fill: #9d9d9d;
}
.credit-card_numbers {
  display: flex;
  justify-content: center;
}
.credit-card_payment_buttons {
  position: relative;
  display: flex;
  justify-content: center;
  margin-top: 1.7rem;
  z-index:2;
}
input {
  background: transparent;
  border:none;
  padding:.5rem 1rem;
  font-size: 1.3rem;
  font-family: 'Rubik', sans-serif;
  outline: none;
  word-spacing: .5rem;
  color: #ffffff;
  margin: .2rem 0;
  text-shadow: .1rem .1rem 0 rgba(0, 0, 0, 0.13);
  border-bottom: .2rem solid #ffffff00;
}
input::placeholder {color: #ffffff
}
input:focus {
  border-bottom: .2rem solid #ffffff77;
  padding: .3rem 1rem .7rem 1rem;
}
input[type="tel"] {
  width: 25%;
}
label {font-size: .9rem; color:#FFFFFF77}
button {
  border: none;
  padding: .7rem 1.2rem;
  margin: .3rem;
  border-radius: 1.5rem;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  transition: all .2s;
  background: #ccc;
  font-size: 1.1rem;

}

.btn-confirm {background: #7a7a7a; color:#fff;}
.btn-active {background: #f36a09;}
.btn-active:hover {background: #ff8900;box-shadow: 0 0 .3rem #fc6100AA}
.input-confirm {
  position:absolute;
  right:1rem;
  bottom:1.5rem;
  fill: #39e80a;
  width: 2rem;
  animation: check .2s forwards;
}
@keyframes check {
  from {opacity: 0}
}
img {width: 90%}

@media screen and (max-width: 767px) {
  .checkout-container {
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    padding: 1rem;
    width: 80vw;
    height: 90dvh;
  }
  .credit-card_body {
    top: 0;
  }
  img {width: 70vw}
}
</style>
