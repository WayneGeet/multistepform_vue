<template>
  <div class="home">
    <section class="main">
      <div class="side"></div>

      <div class="right">
        <Modal v-if="currentPage===1" v-bind="personal">
          <template v-slot:form>
            <form>
              <label for="name">Name</label>
              <input v-model="name" type="text" id="name" required>

              <label for="email">Email</label>
              <input v-model="email" type="email" id="email" required>

              <label for="phone">Phone Number</label>
              <input v-model="phone" type="text" id="phone" required>
          </form>
          </template>
        </Modal>

        <Modal v-else-if="currentPage===2" v-bind="plan">
          <template v-slot:plan>
            <div class="planContainer">
              <div v-for="plan in planObj" :key="plan.id">

                <article class="plan" :class="{active: activeIndex===plan.id}" @click="setActive(plan.id)">
                  <img :src="plan.image" :alt="plan.title"/>

                  <h3>{{ plan.title }}</h3>
                  <p class="price" v-if="isMonth">{{ plan.mp }}/mo</p>
                  <p class="price" v-else>{{ plan.yp }}/yr</p>
                  <p class="discount" :class="{shown :isMonth}">2 months free</p>

                </article>

              </div>
            </div>
            <div class="switch">
              <Switch :checkedValue="isMonth" @click="handleSwitch" v-model="isMonth"/>
            </div>
          </template>
        </Modal>

        <Modal v-else-if="currentPage===3" v-bind="billing">
          <template v-slot:billing>
            <article >
              <section class="add-ons" v-for="option in billingOptions" :key="option.title">
                <article class="option" @click="handleOption(option.id)">
                  <input :checked="optionValues[option.id-1]" type="checkbox">
                  <div class="middle">
                    <h4>{{ option.title }}</h4>
                    <p>{{ option.sub }}</p>
                  </div>
                  <p v-if="isMonth">+${{ option.mp }}/mo</p>
                  <p v-else>+${{ option.yp }}/yr</p>
                </article>
              </section>
            </article>
          </template>
        </Modal>

        <!-- buttons -->
        <article class="btns">
          <button class="btn" @click="handleSteps(2)" type="primary">Go Back</button>
          <button class="btn" @click="handleSteps(1)" type="primary">Next</button>
        </article>
      </div>
      
    </section>



    
  </div>
</template>

<script>
import { ref } from 'vue'
// @ is an alias to /src
import Modal from "../components/Modal.vue" 
import { Button, Switch } from "ant-design-vue";


export default {
  name: 'HomeView',
  components: {Modal, Button, Switch},
  setup(){
    const currentPage = ref(3)
    const personal=ref({title:"Personal Info", sub:"Please provide your name, email and phone number"})
    const name = ref("")
    const email = ref("")
    const phone = ref("")

    
    // PLAN
    const isMonth = ref(true)
    const activeIndex = ref(null)
    const setActive = (id) => {
      activeIndex.value = id
      console.log(activeIndex.value, id)
    }
    const plan = ref({title:"Select your plan", sub:"You have the option of monthly or yearly billing"})
    const planObj = ref([
      { image:require("../assets/images/icon-arcade.svg"), title: "Arcade", mp: 9, yp: 90, id: 0 },
      { image:require('../assets/images/icon-advanced.svg'), title: "Advanced", mp: 12, yp: 120, id: 1 },
      { image:require('../assets/images/icon-pro.svg'), title: "Pro", mp: 15, yp: 150, id: 2 },
    ])
    
    const handleSwitch = () => {
      isMonth.value = !isMonth.value
    }
    const handleSteps = (i) => {
      if(i ===1 ){
        currentPage.value += 1
      }
      if(i ===2 ){
        currentPage.value -= 1
      }
    }

    // Billing
    const billing = ref({title:"Pick add-ons", sub:"Add ons help enhance your gaming experience"})
    const billingOptions = ref([
      {title:"Online service", sub:"Access to multiplayer games", mp:1, yp:10, id:1 },
      {title:"Larger storage", sub:"Extra 1TB of cloud save", mp:1, yp:10, id:2 },
      {title:"Customizable profile", sub:"Custom theme on your profile", mp:2, yp:20, id:3 },
    ])
    const optionValues = ref(Array(3).fill(false))

    const handleOption = (id) => {
      optionValues.value[id-1] = !optionValues.value[id-1]
      console.log(optionValues.value)
    }


    return{currentPage, personal, name, email, phone, planObj, isMonth, handleSwitch, optionValues,
       plan, handleSteps, setActive, activeIndex, billing, billingOptions, handleOption}
  }
}
</script>

<style>
  * {
    margin:0;
    padding:0;
    box-sizing:border-box;
  }

  .main {
    max-width:567px;
    margin:0 auto;
    display:flex;
    gap:2rem;
  }

  .right {
    background-color:rgb(138, 210, 243);
  }

  form{
    text-align:left;
    margin:0 auto;
    /* display:flex; */
    flex-direction:column;
    /* align-items:start; */
    /* gap:0.5rem; */
  }

  label {
    font-size:0.9rem;
    color:#7c7c7c;
    transform:uppercase;
    display:block;
    margin-bottom:0.2rem;
  }

  label:not(:first-child){
    margin-top:1.3rem;
  }

  

  input {
    display:block;
    border:2px solid #ddd;
    border-radius:0.3rem;
    width:100%;
    padding:0.4rem 0.8rem;
    outline:none;
    font-size:0.9rem;
  }

  input :focus {
    border:2px solid rgb(9, 91, 146);
  }

  .btns {
    display:flex;
    justify-content:space-between;
    align-items:center;
  }

  .btn {
    display:inline-block;
    background-color:rgb(64, 102, 206);
    padding:10px 20px;
    border-radius:8px;
    border:none;
    color:white;
    margin-top:2rem;
    font-size:1rem;
    cursor:pointer;
  }

  .btn:hover {
    background-color:rgba(64, 102, 206, 0.8);
  }

  .active {
    background-color:#eee;
  }

  .side {
    width:8rem;
    height:100%;
    border-radius:0.9rem;
    background-color:blue;
    min-height:20rem;
  }

  /* plans */
  .plan {
    display:flex;
    flex-direction:column;
    align-items: start;
    cursor:pointer;
    min-width:7rem;
    padding:0.4rem 0.8rem;
    border-radius:0.5rem;
    border:2px solid #eee;
  }

  .plan:hover {
    border:2px solid #ddd;
  }

  .plan > h3 {
    font-weight:bold;
    font-size:1.3rem;
    padding-top:0.5rem;
  }

  .planContainer {
    display:flex;
    gap:1rem;
    align-items:center;
    margin-bottom:1rem;
  }

  .price {
    font-size:0.8rem;
    font-weight:700;
    color:#9e9e9e;
  }
  
  .discount {
    color:rgba(0, 60, 128, 0.7);
    font-size:0.8rem;
  }

  .shown{
    opacity:0
  }

  /* Billing Options */
  .option{
    display:grid;
    grid-template-columns:repeat(5, 1fr);
    align-items:center;
    width:100%;
    background-color:#eee;
  }

  input[type="checkbox"] {
    width:2rem;
    height:1rem;
    display:inline-block;
  }

  .middle {
    grid-column: span 3 / span 3;
    padding:0.2rem 0;
  }

  .middle > h4 {
    font-weight:700;
    color:rgb(21, 19, 53, 0.8);
  }

  .middle > p {
    color:#7c7c7c;
    font-size:0.9rem;
  }

  .middle > * {
    text-align:left;
  }

  .add-ons {
    padding:0.4rem 0.5rem;

  }


</style>
