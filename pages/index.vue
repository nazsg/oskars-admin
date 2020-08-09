<template>
  <div class="home">
    <h2>Oskars Barbers Administrative Page</h2>
    <div  v-if="!loggedIn">

    <form class="login" onSubmit="return false">
      <ul>
        <li>
          <label for="">username</label>
          <input type="text" v-model="login.username">
        </li>
        <li>
          <label for="">password</label>
          <input type="password" v-model="login.password" >
          <!-- <input type="text" v-model="login.password" @keyup.prevent="checkEnter($event)"> -->
        </li>
        <li class="actions">
          <label for=""></label>
          <button @click.prevent="clear">Clear</button>
          <button @click.prevent="validate">Submit</button>
        </li>
      </ul>
      <p>{{formError}}</p>

      <!-- <div class="shown-1">
        <span class="val-1">test</span>  <button @click="show(1)">edit</button>
      </div>
      <div class="hidden hidden-1">
        <input type="text" value="test" class="update-1"  > <button @click="hide(1)">submit</button>
      </div>

      <div class="shown-2">
        <span class="val-2">test2</span>  <button @click="show(2)">edit</button>
      </div>
      <div class="hidden hidden-2">
        <input type="text" value="test2" class="update-2" > <button @click="hide(2)">submit</button>
      </div> -->
      
    </form>

    </div> 

    <div v-else class="loggedIn"> 
      <div>
        <button class="logout" @click.prevent="logout">Log out</button>
        <h3>PRICES</h3>
        <p v-for="(p,index) in prices" :key="index">
          <ul :class="'shown-' + p.price_id">
            <li>
              <label for="">item</label>
              <span :class="'valItem-' + p.price_id">{{p.item}}</span>
            </li>
            <li>
              <label for="">price</label>
              <span :class="'valPrice-' + p.price_id">{{p.price}}</span> 
              <iconEdit @click="show(p.price_id)" title="Edit" />
            </li>
          </ul>

          <ul class="hidden" :class="'hidden-' + p.price_id">
            <li>
              <label for="">item</label>
              <input type="text" :value="p.item" class="item" :class="'updateItem-' + p.price_id">
            </li>
            <li>
              <label for="">price</label>
              <input type="text" :value="p.price"  class="price" :class="'updatePrice-' + p.price_id"> 
              <iconSend @click="updatePrice(p.price_id)" title="Submit" />
            </li>
          </ul>
        </p>
        <section>
          <ul>
          Add new item
            <li>
              <label for="">item</label>
              <input type="text" v-model="newItem" class="item">
            </li>
            <li>
              <label for="">price</label>
              <input type="text" v-model="newItemPrice" class="price">
            </li>
            <li>
              <label for=""></label>
              <button @click.prevent="add">Add</button>
            </li>
          </ul>
        </section>        
        <h3 style="margin-top:20px">HOURS</h3>
        <p v-for="(p,index) in hours" :key="index+100">
          <ul :class="'shown-' + p.hour_id + 100">
            <li>
              <label for="">start</label>
              <span :class="'valStart-' + p.hour_id + 100">{{p.start}}</span>
            </li>
            <li>
              <label for="">end</label>
              <span :class="'valEnd-' + p.hour_id + 100">{{p.end}}</span> 
              <iconEdit @click="show(p.hour_id + 100)" title="Edit" />
            </li>
          </ul>

          <ul class="hidden" :class="'hidden-' + p.hour_id + 100">
            <li>
              <label for="">start</label>
              <input type="text" :value="p.start" class="item" :class="'updateStart-' + p.hour_id + 100">
            </li>
            <li>
              <label for="">end</label>
              <input type="text" :value="p.end"  class="price" :class="'updateEnd-' + p.hour_id + 100"> 
              <iconSend @click="updateHour(p.hour_id + 100)" title="Submit" />
            </li>
          </ul>
        </p>
          <!-- <li>
            <label for="">item</label>
            <input type="text" :value="p.item" @blur="edit_item(p.id,$event)" class="item">
          </li>
          <li>
            <label for="">price</label>
            <input type="text" :value="p.price" @blur="edit_price(p.id,$event)" class="price">
          </li> -->
      </div>

    </div>
  </div>
</template>

<script>
import iconEdit from 'vue-material-design-icons/Pencil.vue'
import iconSend from 'vue-material-design-icons/Send.vue'
export default {
  components: { iconEdit, iconSend},
  data() {
    return {
      login :{
        loginToOskars: '',
        username: '',
        password: '',
      },
      formError: '',
      loggedIn : false,
      loginStatus: '',
      prices: '',
      hours: '',
      priceUrl: 'https://oskarsbarbers4men.co.uk/indexAPI.php',
      hourUrl: 'https://oskarsbarbers4men.co.uk/indexAPI_hours.php',
      newItem: '', newItemPrice: '',
      timedout: false
    }
  },
  methods: {
    show(n) {
      document.querySelector('.hidden-' + n).style.display = 'inline-block'
      document.querySelector('.shown-' + n).style.display = 'none'
    },
    updatePrice(id) {
      this.checkLoginStatus()
      setTimeout(() => {
        if(this.loginStatus == 'ok') {

          let item = document.querySelector('input.updateItem-' + id).value 
          let price = document.querySelector('input.updatePrice-' + id).value 
          let info = { editItemPrice : true, item, price, price_id: id }
          // console.log(info)
          this.$axios.post(this.priceUrl, info)
          .then( res => {
          })
          document.querySelector('.hidden-' + id).style.display = 'none'
          document.querySelector('.shown-' + id).style.display = 'block'
          document.querySelector('.valItem-' + id).innerHTML = item
          document.querySelector('.valPrice-' + id).innerHTML = price        
        }
      }, 1000);
    },
    updateHour(id) {
      let id_orig = id.replace('100', '')
      this.checkLoginStatus()
      setTimeout(() => {
        if(this.loginStatus == 'ok') {

          let start = document.querySelector('input.updateStart-' + id).value 
          let end = document.querySelector('input.updateEnd-' + id).value 
          let info = { editHours : true, start, end, hour_id: id_orig }
          // console.log(info)
          this.$axios.post(this.hourUrl, info)
          .then( res => {
          })
          document.querySelector('.hidden-' + id).style.display = 'none'
          document.querySelector('.shown-' + id).style.display = 'block'
          document.querySelector('.valStart-' + id).innerHTML = start
          document.querySelector('.valEnd-' + id).innerHTML = end        
        }
      }, 1000);
    },
    clear() {
      this.login.username = '', this.login.password = '', this.formError  = ''
    },
    clearNew() {
      this.newItem = '', this.newItemPrice = ''
    },
    dont() {
      console.log('')
    },
    checkEnter(e) {
      if (e.keyCode === 13) {
        // e.key = null
        e.preventDefault();
        // this.validate()
        return
      }
    },
    validate() {
      this.formError = ''
      if(this.login.username == '' || this.login.password == '') {
        this.formError = 'pls fill out both fields'
        console.log('errors')
      } else {
        this.submit()
      }
    },
    submit() {
      this.$axios.post(this.priceUrl, this.login)
      .then(res => {
        if(res.data == 'success') {
          this.loggedIn = true
          localStorage.setItem('loggedIn', true)
          this.successful_login()
          console.log(res.data)
        } else {
          this.formError = 'login error'
        }
      })
    },
    successful_login() {
      this.$axios.get(this.hourUrl)
      .then(res => this.hours = res.data)
      this.$axios.get(this.priceUrl)
      .then(res => this.prices = res.data)
    },
    logout() {
      this.loggedIn = false
      localStorage.removeItem('loggedIn')
      this.$axios.get(this.priceUrl + '?logout')
      // setTimeout(() => {
      //   window.location.reload()      
      // }, 500);
    },
    edit_item(i,e) {
      this.checkLoginStatus()
      setTimeout(() => {
        if(this.loginStatus == 'ok') {
          let update = { editItem: true, id: i, item: e.target.value}
          // console.log(update)
          this.$axios.post(this.priceUrl, update)
          .then(res => {
            console.log(res.data)
          })  
        }        
      }, 1000);
    },
    edit_price(i,e) {
      this.checkLoginStatus()
      setTimeout(() => {
        if(this.loginStatus == 'ok') {
          let update = { editPrice: true, id: i, price: e.target.value}
          // console.log(update)
          this.$axios.post(this.priceUrl, update)
          .then(res => {
            console.log(res.data)
          })  
        }        
      }, 1000);
    },
    checkLoginStatus() {
      this.$axios.get(this.priceUrl + '?check_status')
      .then(res => {
        console.log(res.data)
        if(res.data == 'loggedin') {
          console.log("you're still logged in")
          this.loginStatus = 'ok'
          } else {
          this.logout()
          this.formError = 'timed out'
          // window.location.reload()
        }
      })
    },
    add() {
      let newItem =  {addItem: true, item: this.newItem, price: this.newItemPrice}
      console.log(newItem)
      this.$axios.post(this.priceUrl, newItem)
      .then(res => {
        console.log(res.data)
      })
      setTimeout(() => {
        this.successful_login()
        this.clearNew()
      }, 1000);
    }
  },
  mounted() {
    this.$axios.get(this.priceUrl + '?check_status')
    .then(res => {
      if(res.data == 'loggedin') {
        localStorage.setItem('loggedIn', true)
        this.loggedIn = true
        this.successful_login()
      } else {
        this.logout()        
      }
    })
  }
}
</script>

